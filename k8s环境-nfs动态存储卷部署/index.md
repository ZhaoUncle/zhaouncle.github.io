# K8s环境 nfs 动态存储卷部署


<!--more-->

# NFS动态存储卷

## 环境

- 系统：CentOS7
- K8s 1.20

## 预备知识

如果你不知道NFS是做什么的，那么请参考下面链接（包括但不限于）进行学习：

- [NFS](https://baike.baidu.com/item/NFS/812203)
- nfs 是基于网络共享存储，所以在大数据量场景下不适用，在环境搭建测试学习 k8s 阶段反而很合适，当然，小业务量场景下也非常合适使用。
- NFS 允许系统将其目录和文件共享给网络上的其他系统。通过 NFS，用户和应用程序可以访问远程系统上的文件，就象它们是本地文件一样。

##使用前要注意 k8s 版本

 在v1.20.0中禁用了selfLink，在 v1.21 中将会彻底的移除，In k8s 1.20 they have removed selfLink as per [release notes](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.20.md).

报错：`selfLink was empty`

```
[root@node01 deploy]# kubectl logs -f nfs-client-provisioner-cf84d684-vtns2
.....
E0324 15:05:21.127112       1 controller.go:1004] provision "default/test-claim" class "managed-nfs-storage": unexpected error getting claim reference: selfLink was empty, can't make reference
```

参考：

- https://github.com/kubernetes-sigs/nfs-subdir-external-provisioner/issues/25

- https://github.com/kubernetes/kubernetes/pull/94397

- https://wqblogs.com/2021/01/27/k8s%E5%AF%B9%E6%8E%A5nfs%E5%AD%98%E5%82%A8/

解决办法：

```
如果是 kubeadm 基于 yaml 安装 kube-apiserver方式安装：
Current workaround is to edit /etc/kubernetes/manifests/kube-apiserver.yaml

Under here:

spec:
  containers:
  - command:
    - kube-apiserver
Add this line:
- --feature-gates=RemoveSelfLink=false

The do this:
kubectl apply -f /etc/kubernetes/manifests/kube-apiserver.yaml
kubectl apply -f /etc/kubernetes/manifests/kube-apiserver.yaml

如果是 二进制安装的话，add --feature-gates=RemoveSelfLink=false to /opt/kubernetes/cfg/kube-apiserver.conf，然后 restart 一下， 
(I had to do it twice to get it to work)
```

## 创建 NFS 服务器

### 安装nfs-utils

- 在集群每一个节点安装`nfs-utils`，master 和 node 节点等。

```
yum install -y nfs-utils
```

### 配置nfs-server

- 创建共享目录

```
mkdir -p /ifs/kubernetes/
```

- 编辑`/etc/exports`文件添加需要共享目录，每个目录的设置独占一行，编写格式如下：

```
NFS共享目录路径客户机IP段(参数1,参数2,...,参数n)
```

- 例如：

```
/ifs/kubernetes 192.168.110.0/24(rw,no_root_squash)
```

参数说明:

> ro只读访问
>
> rw读写访问
>
> sync所有数据在请求时写入共享
>
> asyncnfs在写入数据前可以响应请求
>
> securenfs通过1024以下的安全TCP/IP端口发送
>
> insecurenfs通过1024以上的端口发送
>
> wdelay如果多个用户要写入nfs目录，则归组写入（默认）
>
> no_wdelay如果多个用户要写入nfs目录，则立即写入，当使用async时，无需此设置hide在nfs共享目录中不共享其子目录
>
> no_hide共享nfs目录的子目录
>
> subtree_check如果共享/usr/bin之类的子目录时，强制nfs检查父目录的权限（默认）
>
> no_subtree_check不检查父目录权限
>
> all_squash共享文件的UID和GID映射匿名用户anonymous，适合公用目录
>
> no_all_squash保留共享文件的UID和GID（默认）
>
> root_squashroot用户的所有请求映射成如anonymous用户一样的权限（默认）
>
> no_root_squashroot用户具有根目录的完全管理访问权限
>
> anonuid=xxx指定nfs服务器/etc/passwd文件中匿名用户的UID
>
> anongid=xxx指定nfs服务器/etc/passwd文件中匿名用户的GID



- 注1：尽量指定IP段最小化授权可以访问NFS 挂载的资源的客户端
- 注2：经测试参数insecure必须要加，否则客户端挂载出错mount.nfs: access denied by server while mounting

### 启动NFS服务

- 配置完成后，您可以在终端提示符后运行以下命令来启动 NFS 服务器：

```
systemctl enable nfs
systemctl start nfs
```

### 检查NFS服务提供是否正常

- 到客户机上执行showmount命令进行检查

```
[root@node05 ~]# showmount -e
Export list for node05:
/ifs/kubernetes 192.168.110.0/24
```

## 在客户端验证

```
[root@k8s-node1 ~]# yum -y install nfs-utils
[root@k8s-node1 ~]# mount -t nfs 192.168.110.239:/ifs/kubernetes /mnt
[root@k8s-node1 ~]# df -h|grep mnt
192.168.110.239:/ifs/kubernetes   37G  4.2G   33G  12% /mnt
[root@k8s-node1 ~]# touch /mnt/a.txt
```



## 安装 nfs-client-provisioner

###在集群每一个节点安装`nfs-utils`

```
yum install -y nfs-utils
```

### 在任意一个master节点执行下面，安装`nfs-client-provisioner`

```
yum -y install git
git clone https://github.com/kubernetes-incubator/external-storage
cd external-storage/nfs-client/deploy/
```

- commit 版本：`201f40d78a9d3fd57d8a441cfc326988d88f35ec`

###安装：

需要修改的地方：

- rbac.yaml ：默认使用 default 的 namespace，如果有需要要自行更改

- class.yaml ：场景 storageclass 存储卷 

- deployment.yaml  ：这个文件是创建 nfs client，供其他 pod 调用客户端，这里除了使用 default 的 namespace 之外，还要修改 nfs 的配置

```
# cat deployment.yaml 
......省略
      containers:
        - name: nfs-client-provisioner
          image: quay.io/external_storage/nfs-client-provisioner:latest  #网络不好要换成国内的镜像地址
          volumeMounts:
            - name: nfs-client-root
              mountPath: /persistentvolumes
          env:
            - name: PROVISIONER_NAME
              value: fuseim.pri/ifs
            - name: NFS_SERVER
              value: 192.168.110.239   ### 这里要换成 nfs 主机的 地址
            - name: NFS_PATH
              value: /ifs/kubernetes   ### 这里要换成 nfs 的存储路径
      volumes:
        - name: nfs-client-root
          nfs:
            server: 192.168.110.239		### 这里要换成 nfs 主机的 地址
            path: /ifs/kubernetes		### 这里要换成 nfs 的存储路径
```

部署：

```
kubectl apply -f rbac.yaml 
kubectl apply -f class.yaml 
kubectl apply -f deployment.yaml 
```

安装结果

```
[root@node01 deploy]# kubectl get pod
NAME                                    READY   STATUS    RESTARTS   AGE
nfs-client-provisioner-cf84d684-cxpmd   1/1     Running   0          6m37s
[root@node01 deploy]# kubectl get sc
NAME                  PROVISIONER      RECLAIMPOLICY   VOLUMEBINDINGMODE   ALLOWVOLUMEEXPANSION   AGE
managed-nfs-storage   fuseim.pri/ifs   Delete          Immediate           false                  6m40s    
[root@node01 deploy]# kubectl get  serviceaccounts 
NAME                     SECRETS   AGE
default                  1         3h48m
nfs-client-provisioner   1         7m25s
[root@node01 deploy]# kubectl logs nfs-client-provisioner-cf84d684-cxpmd 
I0404 05:53:56.745867       1 leaderelection.go:185] attempting to acquire leader lease  default/fuseim.pri-ifs...
E0404 05:53:56.759885       1 event.go:259] Could not construct reference to: '&v1.Endpoints{TypeMeta:v1.TypeMeta{Kind:"", APIVersion:""}, ObjectMeta:v1.ObjectMeta{Name:"fuseim.pri-ifs", GenerateName:"", Namespace:"default", SelfLink:"", UID:"522dc6e9-bb00-49bb-8d11-71a2f85be921", ResourceVersion:"19718", Generation:0, CreationTimestamp:v1.Time{Time:time.Time{wall:0x0, ext:63753112436, loc:(*time.Location)(0x1956800)}}, DeletionTimestamp:(*v1.Time)(nil), DeletionGracePeriodSeconds:(*int64)(nil), Labels:map[string]string(nil), Annotations:map[string]string{"control-plane.alpha.kubernetes.io/leader":"{\"holderIdentity\":\"nfs-client-provisioner-cf84d684-cxpmd_24f93740-950a-11eb-921c-c69636fbe444\",\"leaseDurationSeconds\":15,\"acquireTime\":\"2021-04-04T05:53:56Z\",\"renewTime\":\"2021-04-04T05:53:56Z\",\"leaderTransitions\":0}"}, OwnerReferences:[]v1.OwnerReference(nil), Initializers:(*v1.Initializers)(nil), Finalizers:[]string(nil), ClusterName:""}, Subsets:[]v1.EndpointSubset(nil)}' due to: 'selfLink was empty, can't make reference'. Will not report event: 'Normal' 'LeaderElection' 'nfs-client-provisioner-cf84d684-cxpmd_24f93740-950a-11eb-921c-c69636fbe444 became leader'
I0404 05:53:56.759957       1 leaderelection.go:194] successfully acquired lease default/fuseim.pri-ifs
I0404 05:53:56.759997       1 controller.go:631] Starting provisioner controller fuseim.pri/ifs_nfs-client-provisioner-cf84d684-cxpmd_24f93740-950a-11eb-921c-c69636fbe444!
I0404 05:53:56.860178       1 controller.go:680] Started provisioner controller fuseim.pri/ifs_nfs-client-provisioner-cf84d684-cxpmd_24f93740-950a-11eb-921c-c69636fbe444!
```



## 验证安装

- 该 git 提供了两个测试例子：test-pod.yaml 和 test-claim.yaml ,创建 pvc 和 pod 去验证 nfs

```
kubectl apply -f test-pod.yaml 
kubectl apply -f test-claim.yaml 
```

- 验证是否正常

```
[root@node01 deploy]# kubectl get pvc
NAME         STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS          AGE
test-claim   Bound    pvc-35ef9c39-fa80-4b0e-9a3c-fdb755beddf9   1Mi        RWX            managed-nfs-storage   10s
[root@node01 deploy]# kubectl get pod
NAME                                    READY   STATUS      RESTARTS   AGE
nfs-client-provisioner-cf84d684-brlfk   1/1     Running     0          71s
test-pod                                0/1     Completed   0          23s
```

查看日志，正常创建test-claim

```
[root@node01 deploy]# kubectl logs -f nfs-client-provisioner-cf84d684-brlfk 
......
I0404 06:15:53.548454       1 event.go:221] Event(v1.ObjectReference{Kind:"PersistentVolumeClaim", Namespace:"default", Name:"test-claim", UID:"35ef9c39-fa80-4b0e-9a3c-fdb755beddf9", APIVersion:"v1", ResourceVersion:"22423", FieldPath:""}): type: 'Normal' reason: 'ProvisioningSucceeded' Successfully provisioned volume pvc-35ef9c39-fa80-4b0e-9a3c-fdb755beddf9
```

查看 nfs 存储，也成功创建

```
[root@node05 kubernetes]# ll
total 0
drwxrwxrwx 2 root root 21 Apr  4 14:15 default-test-claim-pvc-35ef9c39-fa80-4b0e-9a3c-fdb755beddf9
[root@node05 kubernetes]# pwd
/ifs/kubernetes
```

pod状态为`Completed`则为正常，若长时间为`ContainerCreating`状态则为不正常，请确认安装操作步骤是否正确。

- 清除测试用例

```
 kubelete delete -f test-claim.yaml  &&  kubelete delete -f test-pod.yaml
```
