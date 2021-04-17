# 【k8s】部署 jenkins 并部署 tomcat 服务示例


<!--more-->

# 1. 环境

- k8s 集群：v1.20.4 版本

| k8s-master1 | 192.168.110.235 |
| ----------- | --------------- |
| k8s-node1   | 192.168.110.236 |
| k8s-node2   | 192.168.110.237 |
| gitlab      | 192.168.110.238 |
| nfs，harbor | 192.168.110.239 |

# 2. 发布流程

![img](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210412222813.png)

流程：

- 拉取代码 git checkout
- 编译代码 mvn clean
- 打包镜像 并上传镜像仓库
- 使用yaml 模板文件部署用镜像仓库中的镜像，kubectl 命令部署pod
- 开发测试

# 3. harbor 部署

参考：[Harbor安装](https://zhaouncle.com/k8s%E7%8E%AF%E5%A2%83-harbor%E5%AE%89%E8%A3%85/)

# 4. gitlab 部署

参考：[Gitlab安装](https://zhaouncle.com/k8s%E7%8E%AF%E5%A2%83-gitlab%E5%AE%89%E8%A3%85/)

# 5. nfs server 部署

参考：[K8s环境 nfs 动态存储卷部署](https://zhaouncle.com/k8s%E7%8E%AF%E5%A2%83-nfs%E5%8A%A8%E6%80%81%E5%AD%98%E5%82%A8%E5%8D%B7%E9%83%A8%E7%BD%B2/)

```
#在原有的 nfs server 上面添加 jenkins 的 nfs
mkdir -p /data/nfs/jenkins
echo "/data/nfs/jenkins *(rw,sync,no_root_squash,no_subtree_check)" >> /etc/exports
systemctl restart nfs && systemctl restart rpcbind
```



# 6. 在 Kubernetes 中部署 jenkins

![img](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210412223131.png)

## 6.1 创建 jenkins 动态 pvc

```
kubectl create namespace nfs

helm repo add helm-stable https://charts.helm.sh/stable        
helm repo update

cat >  jenkins-client-nfs.yaml << EOF
# NFS 设置
nfs:
  server: 192.168.110.239
  path: /data/nfs/jenkins
storageClass:
  # 此配置用于绑定 PVC 和 PV
  name: jenkins-nfs-client
  
  # 资源回收策略
#主要用于绑定的PVC删除后，资源释放后如何处理该PVC在存储设备上写入的数据。
#Retain：保留，删除PVC后，PV保留数据；
#Recycle：回收空间，删除PVC后，简单的清除文件；（NFS和HostPath存储支持）
#Delete：删除，删除PVC后，与PV相连接的后端存储会删除数据；（AWS EBS、Azure Disk、Cinder volumes、GCE PD支持）
  reclaimPolicy: Delete
# 使用镜像
image:
  repository: kubesphere/nfs-client-provisioner
# 副本数量
replicaCount: 1
EOF

#helm 部署 nfs-client-provisioner
helm install jenkins-nfs-storage -n nfs --values jenkins-client-nfs.yaml helm-stable/nfs-client-provisioner
```

###查看运行状态：

```
[root@node01 helm]# kubectl get pod,sc -n nfs|grep jenkins
pod/jenkins-nfs-storage-nfs-client-provisioner-6d8974459b-wwh88   1/1     Running   0          50s
storageclass.storage.k8s.io/jenkins-nfs-client    cluster.local/jenkins-nfs-storage-nfs-client-provisioner   Delete          Immediate           true                   50s
```



## 6.2 yaml 部署 jenkins

```
mkdir /data/yml/jenkins -p
cd /data/yml/jenkins
kubectl create namespace ops

cat > jenkins.yml << EOF
apiVersion: apps/v1
kind: Deployment
metadata:
  name: jenkins
  namespace: ops
  labels:
    name: jenkins
spec:
  replicas: 1
  selector:
    matchLabels:
      name: jenkins 
  template:
    metadata:
      name: jenkins
      labels:
        name: jenkins
    spec:
      serviceAccountName: jenkins
      containers:
        - name: jenkins
          image: jenkins/jenkins
          ports:
            - containerPort: 8080
            - containerPort: 50000
          resources:
            limits:
              cpu: 2
              memory: 4Gi
            requests:
              cpu: 1
              memory: 1Gi
          env:
            - name: LIMITS_MEMORY
              valueFrom:
                resourceFieldRef:
                  resource: limits.memory
                  divisor: 1Mi
            - name: JAVA_OPTS
              value: -Xmx\$(LIMITS_MEMORY)m -XshowSettings:vm -Dhudson.slaves.NodeProvisioner.initialDelay=0 -Dhudson.slaves.NodeProvisioner.MARGIN=50 -Dhudson.slaves.NodeProvisioner.MARGIN0=0.85
          volumeMounts:
            - name: jenkins-home
              mountPath: /var/jenkins_home
      securityContext:
        fsGroup: 1000
      volumes:
      - name: jenkins-home
        persistentVolumeClaim:
          claimName: jenkins
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: jenkins
  namespace: ops
spec:
  storageClassName: "jenkins-nfs-client"
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 10Gi
---
apiVersion: v1
kind: Service
metadata:
  name: jenkins
  namespace: ops
spec:
  selector:
    name: jenkins
  type: NodePort
  ports:
    - name: http
      port: 80
      targetPort: 8080
      protocol: TCP
      nodePort: 30008
    - name: agent
      port: 50000
      protocol: TCP
---
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: jenkins
  namespace: ops

---
kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: jenkins
  namespace: ops
rules:
- apiGroups: [""]
  resources: ["pods","events"]
  verbs: ["create","delete","get","list","patch","update","watch"]
- apiGroups: [""]
  resources: ["pods/exec"]
  verbs: ["create","delete","get","list","patch","update","watch"]
- apiGroups: [""]
  resources: ["pods/log"]
  verbs: ["get","list","watch"]
- apiGroups: [""]
  resources: ["secrets","events"]
  verbs: ["get"]

---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: jenkins
  namespace: ops
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: jenkins
subjects:
- kind: ServiceAccount
  name: jenkins
EOF


kubectl apply -f jenkins.yml

```



###查看安装结果

```
[root@node01 ~]# kubectl get pod,pvc,svc -n ops -o wide
NAME                          READY   STATUS    RESTARTS   AGE   IP             NODE        NOMINATED NODE   READINESS GATES
pod/jenkins-f656474b8-bjv5s   1/1     Running   0          20s   10.244.36.92   k8s-node1   <none>           <none>

NAME                            STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS         AGE   VOLUMEMODE
persistentvolumeclaim/jenkins   Bound    pvc-037d24b4-16cf-4900-ac06-76699dbc7c24   10Gi       RWX            jenkins-nfs-client   20s   Filesystem

NAME              TYPE       CLUSTER-IP   EXTERNAL-IP   PORT(S)                        AGE   SELECTOR
service/jenkins   NodePort   10.0.0.229   <none>        80:30008/TCP,50000:30619/TCP   20s   name=jenkins
```

## 6.3 控制台访问

访问方式：k8s master/node ip+ NodePort

访问地址：192.168.110.235:30008

查看密码：`kubectl logs -n ops jenkins-f656474b8-bjv5s |grep -A 3 password`



![img](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210412231058.png)

![img](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210412231105.png)

![img](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210412231123.png)

![img](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210412231130.png)

## 6.4 jenkins 安装插件

默认从国外网络下载插件，会比较慢，建议修改成国内源：

只需要到nfs上，修改PVC挂载的内容即可

```
cd /data/nfs/jenkins/ops-jenkins-pvc-037d24b4-16cf-4900-ac06-76699dbc7c24/updates
cp default.json default.json-bak

#修改插件的下载地址为国内的地址
sed -i 's/https:\/\/updates.jenkins.io\/download/https:\/\/mirrors.tuna.tsinghua.edu.cn\/jenkins/g' default.json

#修改jenkins启动时检测的URL网址，改为国内baidu的地址
sed -i 's/http:\/\/www.google.com/https:\/\/www.baidu.com/g' default.json

```

## 6.5 删除 pod 重建

`kubectl delete pod -n ops jenkins-f656474b8-bjv5s`

修改完后，jenkins 会重建，打开浏览器访问: [http://NodeIP:30008](http://nodeport:30008/)

输入账户密码从新登陆 jenkins 控制台

![img](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210412232609.png)

依次点击 管理Jenkins（Manage Jenkins）->系统配置(System Configuration)-->管理插件(Manage Pluglns)-->

![image-20210412233341764](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210412233341.png)



分别搜索 Git/Git Parameter/Pipeline/kubernetes/Config File Provider，选中点击安装。

安装插件可能会失败，多试几次就好了，安装完记得重启Pod

| 插件名称                     | 用途                                                         |
| ---------------------------- | ------------------------------------------------------------ |
| Git                          | 用于拉取代码                                                 |
| Git Parameter                | 用于Git参数化构建                                            |
| Pipeline                     | 用于流水线                                                   |
| kubernetes                   | 用于连接Kubernetes动态创建Slave代理                          |
| Config File Provider         | 用于存储kubectl用于连接k8s集群的kubeconfig配置文件           |
| Gitlab                       | 当提交代码或打开/更新合并请求时，此插件允许GitLab触发Jenkins中的构建。它还可以将构建状态发送回GitLab。 |
| Gitlab Authentication plugin | GitLab身份验证插件提供了一种使用GitLab进行身份验证和授权以保护Jenkins的方法。 |
| credential                   | 账号密码管理插件                                             |

![img](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210412233541.png)



## 6.6 jenkins 在 k8s 中动态创建代理

![img](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210412233619.png)



![img](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210414115808.png)



### 6.6.1 在 jenkins 中添加 kubernetes ☁️

管理Jenkins->Manage Nodes and Clouds->configureClouds->Add

输入Kubernetes 地址： [https://kubernetes.default](https://kubernetes.default/) ，点击连接测试，测试通过的话，会显示k8s的版本信息

输入Jenkins 地址： [http://jenkins.ops](http://jenkins.ops/)

![image-20210412234304246](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210412234304.png)

![image-20210412234328449](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210412234328.png)

![image-20210412234355680](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210412234355.png)

![img](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210412234821.png)

### 6.6.2 构建 jenkins-slave 镜像

基于官方镜像提供的 jnlp-slave 使用

```
docker pull jenkins/jnlp-slave
docker tag jenkins/jnlp-slave:latest 192.168.110.239/library/jnlp-slave:latest
docker login 192.168.110.239
docker push 192.168.110.239/library/jnlp-slave:latest
```



基于官方 alpine 镜像提供的Dockerfile  包使用

```
cd /data
git clone https://github.com/jenkinsci/docker-inbound-agent.git
cd docker-inbound-agent
cp 8/alpine/Dockerfile .
```



修改 Dockerfile 为以下，主要添加 maven 和 kubectl（和 k8s 集群版本对应）

maven 修改为阿里云源：https://github.com/fxkjnj/kubernetes/blob/main/jenkins-for_kubernetes/jenkins-slave/settings.xml

kubectl 下载：curl -LO https://dl.k8s.io/release/v1.20.4/bin/linux/amd64/kubectl

helm 下载：wget https://get.helm.sh/helm-v3.5.3-linux-amd64.tar.gz && tar zxvf helm-v3.5.3-linux-amd64.tar.gz 

```
ARG version=4.7-1-alpine
FROM jenkins/agent:$version

ARG version
LABEL Description="This is a base image, which allows connecting Jenkins agents via JNLP protocols" Vendor="Jenkins project" Version="$version"

ARG user=jenkins

USER root

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.tuna.tsinghua.edu.cn/g' /etc/apk/repositories &&\
    apk add maven git curl &&\
    rm -rf /var/cache/apk/*

COPY jenkins-agent /usr/local/bin/jenkins-agent
COPY settings.xml /etc/maven/settings.xml
COPY kubectl /usr/bin/
COPY helm /usr/bin/
RUN chmod +x /usr/bin/kubectl &&\
    chmod +x /usr/bin/helm &&\
    chmod +x /usr/local/bin/jenkins-agent &&\
    ln -s /usr/local/bin/jenkins-agent /usr/local/bin/jenkins-slave
    
USER ${user}

ENTRYPOINT ["/usr/local/bin/jenkins-agent"]
```

```
#然后 
docker build . -t 192.168.110.239/library/jenkins-slave:4.7.1-alpine
docker push 192.168.110.239/library/jenkins-slave:4.7.1-alpine
```





在jenkins 中创建一个流水线项目，测试jenkins-slave 是否功能

![image-20210414093048480](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210414093048.png)

在pipeline 中 编写脚本，pipeline 脚本分为 声明式 和 脚本式

我这里写 声明式 脚本

需要注意的是，spec 中定义containers的名字一定要写jnlp

```
pipeline {
    agent {
        kubernetes {
            yaml '''
apiVersion: v1          
kind: Pod
metadata:
  name: jenkins-slave
spec:
  containers:
  - name: jnlp
    image: 192.168.110.239/library/jenkins-slave:4.7.1-alpine
'''

        }
    }
    stages {
        stage('测试') {
            steps {
                sh 'hostname'
                sh 'mvn -version'
            }
        }
    }
}
```

点击Build New 按钮，开始构建

构建结束后，点击项目编号，可以查看jenkins 构建的日志

日志中可以看到 输出了主机名

![image-20210414094412922](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210414094413.png)

### 6.6.3 给 Jenkins-Slave pod 添加存储卷，以及挂载docker 命令到Pod 中

因为每次maven 打包会产生依赖的库文件，为了加快每次编译打包的速度，我们可以创建一个pvc 用来存储maven 每次打包产生的依赖文件。以及 我们需要将 k8s 集群 node 主机上的docker 命令挂载到Pod 中，用于镜像的打包 ，推送

直接创建pvc, 这里使用的动态补给的PV（直接使用上面以及创建好一个jenkins-nfs-client storageClass ）

```
cat > jenkins-slave-pvc.yml << EOF
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mavencache
spec:
  storageClassName: "jenkins-nfs-client"
  accessModes:
    - ReadWriteMany     
  resources:
    requests:
      storage: 10Gi
EOF

kubectl apply -f jenkins-slave-pvc.yml -n ops
kubectl get pvc -n ops
```

Jenkins-Slave pod yaml内容

```
apiVersion: v1          
kind: Pod
metadata:
  name: jenkins-slave
spec:
  containers:
  - name: jnlp
    image: docker push 192.168.110.239/library/jenkins-slave:4.7.1-alpine
    imagePullPolicy: Always
    volumeMounts:
      - name: docker-cmd
        mountPath: /usr/bin/docker
      - name: docker-sock
        mountPath: /var/run/docker.sock
      - name: maven-cache
        mountPath: /root/.m2

  volumes:
    - name: docker-cmd
      hostPath:
        path: /usr/bin/docker
    - name: docker-sock
      hostPath:
        path: /var/run/docker.sock
    - name: maven-cache
      persistentVolumeClaim:
        claimName: mavencache       
```

##6.7 jenkins 在 kubernetes 中部署 tomcat 示例

上面的harbor镜像仓库，gitlab 代码仓库，jenkins 发布平台都已经部署完成，现在我们需要使用Jenkins在Kubernetes中持续部署一个无状态的tomcat 应用

具体流程如下：

1. 拉取代码 git checkout，我这里直接将 github 的代码 import 到 gitlab 里面了，具体可以参考：[Gitlab安装](https://zhaouncle.com/k8s%E7%8E%AF%E5%A2%83-gitlab%E5%AE%89%E8%A3%85/)
2. 编译代码 mvn clean
3. 打包镜像 并上传镜像仓库
4. 使用yaml 模板文件部署用镜像仓库中的镜像，kubectl 命令部署pod
5. 开发测试



###6.7.1 生成拉取git 代码的Pipeline 脚本

登陆jenkins 控制器，使用凭据的方式保存 git 账户信息 和 harbor 账户信息

用于jenkins 从gitlab 中拉取代码

Manage Jenkins -> Manage Credentials -> 全局凭据 (unrestricted) -> Add Credentials

![image-20210414105510716](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210414105510.png)

选择Kind 类型 为 username with passwd

输入账户名，密码

添加一个描述信息

![image-20210415133619944](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210415133620.png)

也可以使用 access token 的方式保存 gitlab 访问权限

Manage Jenkins -> Configure System -> Gitlab

![image-20210414111852988](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210414111853.png)

然后点击一下“Test Connection” 测试为“Success” 状态，则成功了。

![image-20210414114758590](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210414114758.png)

gitlab创建 access token，授予 **api** 权限

![image-20210414114703498](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210414114703.png)



### 创建 jenkins job

使用pipeline 生成 git 拉取代码的语法

jenkins 官方提供一个pipeline 语法的生成器

任意创建一个流水线项目

![image-20210414164313915](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210414164444.png)



在pipeline 选项那里点击 ， pipeline Syntax

![image-20210414164412677](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210414164444.png)

点击片段生成器，在sample step 下拉选项框 中 找到 checkout: check out from version control

输入Repository URL(代码仓库地址):   http://192.168.110.238/root/tomcat-java-demo.git

点击Credentials 选择 刚刚创建的git 的用户凭证

最后点击 Generate Pipeline Script 生产pipeline 语法

![image-20210414165854955](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210414165855.png)

也就是 下面这段话,有了这个我们就可以在jenkins 上去拉取仓库里的代码了

```
checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'gitlab-238', url: 'http://192.168.110.238/root/tomcat-java-demo.git']]])
```

###编译代码 mvn clean

在上面构建Jenkins-Slave镜像的时候，我们已经在镜像里安装了maven 编译代码的软件

只需要代码的路径下执行一条命令即可

```
mvn clean package -Dmaven.skip.test=true
```

### 测试案例 tomcat-java-demo gitlim 的 Dockerfile

```
FROM tomcat:8-jdk8
LABEL maintainer www.ctnrs.com
RUN rm -rf /usr/local/tomcat/webapps/*
ADD target/*.war /usr/local/tomcat/webapps/ROOT.war 
```



### 授权创建 kubeconfig 文件，并保存在 jenkins 中

准备好创建K8S 集群的那一套 ca证书

```
[root@proxy k8s]# cd /data/ssl/
[root@proxy ssl]# ls 
ca-config.json  ca.csr  ca-csr.json  ca-key.pem  ca.pem
```

生成请求证书文件

```
cat >admin-csr.json <<EOF
{
  "CN": "admin",
  "hosts": [],
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "CN",
      "L": "BeiJing",
      "ST": "BeiJing",
      "O": "system:masters",
      "OU": "System"
    }
  ]
}
EOF
```

安装cfssl 生成证书工具

```
curl -L https://pkg.cfssl.org/R1.2/cfssl_linux-amd64 -o /usr/local/bin/cfssl
curl -L https://pkg.cfssl.org/R1.2/cfssljson_linux-amd64 -o /usr/local/bin/cfssljson
curl -L https://pkg.cfssl.org/R1.2/cfssl-certinfo_linux-amd64 -o /usr/local/bin/cfssl-certinfo
chmod +x /usr/local/bin/cfssl*
```

生成证书

```
[root@proxy ssl]# cfssl gencert -ca=ca.pem -ca-key=ca-key.pem -config=ca-config.json -profile=kubernetes admin-csr.json | cfssljson -bare admin
2021/04/15 09:07:41 [INFO] generate received request
2021/04/15 09:07:41 [INFO] received CSR
2021/04/15 09:07:41 [INFO] generating key: rsa-2048
2021/04/15 09:07:42 [INFO] encoded CSR
2021/04/15 09:07:42 [INFO] signed certificate with serial number 707063479988721234464365406719482455967000883299
2021/04/15 09:07:42 [WARNING] This certificate lacks a "hosts" field. This makes it unsuitable for
websites. For more information see the Baseline Requirements for the Issuance and Management
of Publicly-Trusted Certificates, v.1.1.6, from the CA/Browser Forum (https://cabforum.org);
specifically, section 10.2.3 ("Information Requirements").
```

查看生成的证书

```
[root@proxy ssl]# ls admin*
admin.csr  admin-csr.json  admin-key.pem  admin.pem
```

生成kubeconfig授权文件：

````
kubectl config set-cluster kubernetes   --certificate-authority=ca.pem   --embed-certs=true   --server=https://192.168.110.235:6443

#设置用户项中cluster-admin用户证书认证字段
kubectl config set-credentials  cluster-admin   --client-key=admin-key.pem   --client-certificate=admin.pem   --embed-certs=true

#设置默认上下文
kubectl config set-context kubernetes   --cluster=kubernetes   --user=cluster-admin

#设置当前环境的default
kubectl config use-context kubernetes
````

查看生成的config文件

```
[root@node01 ssl]# cat /root/.kube/config 
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUR2akNDQXFhZ0F3SUJBZ0lVQ2xzU2RlK1lnRTNDdkRrd1ZhUkZ5c3dZL0Zrd0RRWUpLb1pJaHZjTkFRRUwKQlFBd1pURUxNQWtHQTFVRUJoTUNRMDR4RURBT0JnTlZCQWdUQjBKbGFXcHBibWN4RURBT0JnTlZCQWNUQjBKbAphV3BwYm1jeEREQUtCZ05WQkFvVEEyczRjekVQTUEwR0ExVUVDeE1HVTNsemRHVnRNUk13RVFZRFZRUURFd3ByCmRXSmxjbTVsZEdWek1CNFhEVEl4TURRd05EQXlNRE13TUZvWERUSTJNRFF3TXpBeU1ETXdNRm93WlRFTE1Ba0cKQTFVRUJoTUNRMDR4RURBT0JnTlZCQWdUQjBKbGFXcHBibWN4RURBT0JnTlZCQWNUQjBKbGFXcHBibWN4RERBSwpCZ05WQkFvVEEyczRjekVQTUEwR0ExVUVDeE1HVTNsemRHVnRNUk13RVFZRFZRUURFd3ByZFdKbGNtNWxkR1Z6Ck1JSUJJakFOQmdrcWhraUc5dzBCQVFFRkFBT0NBUThBTUlJQkNnS0NBUUVBMVYzSDVCaUorQkJjRXlwT3lxZjYKbDVvMTJLNFZOM3JvcnNXL1M1cjhnNWFic2RyZFdCczhremlybnp0VVVlZ0RLMXY3T3B2U0xyVWMzQWRVS2xOZApWRkgrTnpNS1Q0QmZ6UHAxR0w0YWZjVFdKMHQyRUpKNU9rT3BYMkNDdURkd1hQYWJUY0FlVWw5NURsWmdzdUNkCmx3bXFkQzM0dlZqUzRZVHJQekxBOVJYN2NPSkJWRlI1Y2VYYloyS0s2L0ZHSDY1TkJGRWU5NVF6bDhHYVBncTMKa05pNDBUYzdPYVBKa1VsQXhmbG9hTkI3eHdVRnMzTk4yS0cxeTdjWC9MdTZWN0Y5YnZCNkNkZVhvblJxTTUvdgp4cGo1MjlEMUthc3JmcHpuNFNEMWR6dE5tVkgvMWN6ZHI2bDVqQURFSFZhQWFVb0xIWVRqRU4vUisyL0VJZlBLCjN3SURBUUFCbzJZd1pEQU9CZ05WSFE4QkFmOEVCQU1DQVFZd0VnWURWUjBUQVFIL0JBZ3dCZ0VCL3dJQkFqQWQKQmdOVkhRNEVGZ1FVTytFWjI0SmNIMnhkTHR5bS9BWFQ5aTg2Sm53d0h3WURWUjBqQkJnd0ZvQVVPK0VaMjRKYwpIMnhkTHR5bS9BWFQ5aTg2Sm53d0RRWUpLb1pJaHZjTkFRRUxCUUFEZ2dFQkFCVTRkOXBkbTNhL1NUeGNBMXJ1CllQazhjNVVmL3VzeUlKWlZmMlRrYnpHa3VQUU96SGdMbXpuajBmampmYjQ2YUZHalBIZGRFbERieWZyRnZ1aGIKSDFwaWVXaUNCVkFUM1FzeDNUMGg4RlR1dXlDbGpsT3hUNVo5VzJWRS96dHRpbGpIb1hqeEkxejJFZHVPSE80TwpSei9EVVZHSENYWndoVDZhR1psRmhhUUtUNjJKQWRqTTVKME9mSVRWNkQyR2J3QlUzQ0RwMEdORlQwaTZqS0hMCkVLb3ladis1Vjd3ckNoRG1oUTg0WkwwRURtNmROTHNXaDZXbDBYaUJGUEd1UFN2YW96cUlYRGlRR0VWM0xrWE4Kb0prVHFkNXhOdnQzRUh6RlB1RytWMVFmdzRMd3J6ZzZZMG4rZ0tXR2xJWkxLN0Q4VmtuY1NNTnFJOUJ4eFU2TApmWG89Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K
    server: https://192.168.110.235:6443
  name: kubernetes
contexts:
- context:
    cluster: kubernetes
    user: admin
  name: default
- context:
    cluster: kubernetes
    user: cluster-admin
  name: kubernetes
current-context: kubernetes
kind: Config
preferences: {}
users:
- name: admin
  user:
    client-certificate: /opt/kubernetes/ssl/admin.pem
    client-key: /opt/kubernetes/ssl/admin-key.pem
- name: cluster-admin
  user:
    client-certificate-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUQzVENDQXNXZ0F3SUJBZ0lVZTluU3IvK3NWWEVMWnBVSVo1ZjFReFYyOEdNd0RRWUpLb1pJaHZjTkFRRUwKQlFBd1pURUxNQWtHQTFVRUJoTUNRMDR4RURBT0JnTlZCQWdUQjBKbGFXcHBibWN4RURBT0JnTlZCQWNUQjBKbAphV3BwYm1jeEREQUtCZ05WQkFvVEEyczRjekVQTUEwR0ExVUVDeE1HVTNsemRHVnRNUk13RVFZRFZRUURFd3ByCmRXSmxjbTVsZEdWek1CNFhEVEl4TURReE5UQXhNRE13TUZvWERUTXhNRFF4TXpBeE1ETXdNRm93YXpFTE1Ba0cKQTFVRUJoTUNRMDR4RURBT0JnTlZCQWdUQjBKbGFVcHBibWN4RURBT0JnTlZCQWNUQjBKbGFVcHBibWN4RnpBVgpCZ05WQkFvVERuTjVjM1JsYlRwdFlYTjBaWEp6TVE4d0RRWURWUVFMRXdaVGVYTjBaVzB4RGpBTUJnTlZCQU1UCkJXRmtiV2x1TUlJQklqQU5CZ2txaGtpRzl3MEJBUUVGQUFPQ0FROEFNSUlCQ2dLQ0FRRUF5K29tTVNXZ2pLdlUKMHY1K3Eza00xckdiZWlFYXN6c09rM1ROakR0Qy8xQlVYQ1VCOEJMWEFQNDNqaTRNQndrcmVlUVBHeEIwSy90YwpDbHVwTzVtL1lSRFlOcng3NVpJVlZKZnlHWVhPRW1WNWtSY2FTRUF4UUs2Q0FSanJxTGY3QkYwOWs4cTRXV3IzCkVMYS83TE5nWUhRdzR3d3ZSYmVIR1FiOGVxL3VhUTRJTFVHT1NZcTlDcmdOa3gvMms0TDV3NkNWSnZnMk8vYnoKbk04YTkwMHdhWTZBTTVvb2pFYXF4MW5BSW9FMlFVVnpXbHpJTDRMSCtXZnMxcVNHM1VicFlzdEI4VUJVZzREWQpxclpEaldoa2lUb0MwN0x4OVlSdXJoUHpyRnJQT2Y4QU53d0ZLL1NWRms2emkzbk4wSStWVFdqYmk1TnVSTUVHCnpjQWFWSXRNWVFJREFRQUJvMzh3ZlRBT0JnTlZIUThCQWY4RUJBTUNCYUF3SFFZRFZSMGxCQll3RkFZSUt3WUIKQlFVSEF3RUdDQ3NHQVFVRkJ3TUNNQXdHQTFVZEV3RUIvd1FDTUFBd0hRWURWUjBPQkJZRUZNNW1nM2VyaHF4UwpCdW9JSXBpakR1alQzOFJqTUI4R0ExVWRJd1FZTUJhQUZEdmhHZHVDWEI5c1hTN2NwdndGMC9Zdk9pWjhNQTBHCkNTcUdTSWIzRFFFQkN3VUFBNElCQVFBdDhlMnVpTDk3TlZ6S21KU1o3eThFYWFmWGJOcWtVVUpNeXR5dW8vYTcKVS82WjEvemJMQTRBbXNvdTM2WFlMNkIycGxMWitwUDdSNStiZTRmMWdMRk5nTENuZWFpRTJhYkFjdzFSYi9GZwpsN2o1amN3Y2huWjRZeHpmdnJ5U0JvS0NGTFEvZi9ob2tUZE1kQjdMekVDZzZnZE85UzJ3NVBXbEtPNnZKWVdrClVheTlBb1N4MjE2clljcDNOUk95TkpTbHphWEFpUVMvRlRDZTdYUmFxSCtmWmF6NS80QmhNNDRjTW8vcjZwYU4KUlhPVlAwYVdscG1kRVAyN3hkT2MyT09zdzlsQXlmSHk0MDUzTC9rMlppZGJRVk1POU52Vkc4KzdpNktaVmViNwpqSUlqRmFjZUlZaHcwZlE3bERtektES09hdDcwM2xsSDlmQW9IVjVNdFpCMAotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==
    client-key-data: LS0tLS1CRUdJTiBSU0EgUFJJVkFURSBLRVktLS0tLQpNSUlFcEFJQkFBS0NBUUVBeStvbU1TV2dqS3ZVMHY1K3Eza00xckdiZWlFYXN6c09rM1ROakR0Qy8xQlVYQ1VCCjhCTFhBUDQzamk0TUJ3a3JlZVFQR3hCMEsvdGNDbHVwTzVtL1lSRFlOcng3NVpJVlZKZnlHWVhPRW1WNWtSY2EKU0VBeFFLNkNBUmpycUxmN0JGMDlrOHE0V1dyM0VMYS83TE5nWUhRdzR3d3ZSYmVIR1FiOGVxL3VhUTRJTFVHTwpTWXE5Q3JnTmt4LzJrNEw1dzZDVkp2ZzJPL2J6bk04YTkwMHdhWTZBTTVvb2pFYXF4MW5BSW9FMlFVVnpXbHpJCkw0TEgrV2ZzMXFTRzNVYnBZc3RCOFVCVWc0RFlxclpEaldoa2lUb0MwN0x4OVlSdXJoUHpyRnJQT2Y4QU53d0YKSy9TVkZrNnppM25OMEkrVlRXamJpNU51Uk1FR3pjQWFWSXRNWVFJREFRQUJBb0lCQUVzUGVkZUZmai9KMEtHSwpSSjVpQkxQUzFPUno0eHd1bFFMNmI5ODZ3VUt5RXh1SUU5cUhWTlZzdm81N3JMbTZTbUtuN0ZlT0N2VlZwVis2CnRlVFdoM2YzZDE5T3FseXJGV2I2TjRmbGxTRjRjUE5UaWIwbXcvcUhRaHVkVE1IaVYyZ1gybDdZdEZ0VjRIUXEKa1NOVU5Cbk5LMkdvVG81ZkxEK3VMcW9Lc0x2Q1lrZFlsZTRudG5tSkhoR25LbnlVTU1OVGVEMVU0bTZMM0JLYwpRb3ZJTy84NVlySCswa2pkVms2OWYwNGMyUks0bVRLNDBSa1VwbXVtWmtVeUc3ZFNad0VMOXQ3ZmkxSENHV0xyCmFuSkF6OEc1NVlvbTIyTWdMcWpWRVZtR3pQRjhabGNhUnBlZmdmTEFNYVE5RlNCcndRaGtjNm5ranMxSFpyYzIKYXk3bnFmRUNnWUVBM3kyT3ZBZDhCNHVVbW8ycVZYbkZvaEU1S2g0S3VjUWpWMUFZWVRaVEhwaFVBeEQ0S3psRwpoUXFLZGR4WUxRczEycXRiRXlWWkVkR1NJS2x6VkpqUEwvSkRlemc0Y05MVHdRZXhYM0hhWklMdDRDSzFubUdPCjdpeHJTRitqaUZJMkFEdndhbzFCU1d6bDdodzllOFl0V0g5a1Y4S0QweDZ2Yi84cHV0dURsNDhDZ1lFQTZlZFkKaGJKRlZRQWo2NkNYYW5rM3Z4cVJPNzF4d1RKR0JWU2wzSnBISFNnUG81bGlab01ZOGdrc3NlOUUyUDdjazFlcgp5Nis3Uk5GblZFUmRTaHVSZ2ZkN1FWSzFBa2ZtcVBjNTVLOG02cVdLMW9wUTJMYmE5QlJ4MkxEWS94VFptVU41Cldpb28vRUxnVEg5V011a2pXNGo3Zys0b1dISEZleVZoVksxV1pROENnWUVBdU5SK0ZYTU5ZU0pVRy91ZlNkTGoKdm1rTk1yaFdvYXF5cWlrSFI5czUwMmpNaXcyT3VoMWhMZVdnN2lOaVUwdXBla1BYMVc5azZNRHJpR2xOdm80ZgphTkgyMlNHaWZLM2QrVXRwdG1ZUVBKWENjNUhMWURQanI1VFI5TXBnSVJJVTd2UzFFMG1RL0VKWDJtOUE3RFVnCmNONXdOODQ4WnFoUEd5M3pYWEZXOWprQ2dZQXc1eU9FNEY3S2hMcjkycGVOdFVaSWVEK0JJL3lTamZaQnl3N2EKYm84THM0S0JpK0ljMksvd2VxYVRsUmpwM1c4MGh4SHVCaEc3TTZUTTQzWTF0a25YeUd4R25nb21MZTQxeVdxYgpPMXVCbDViTmdDR2pEYWY5ZkFESmMwQUxKTXJSMWwrYTBGRzlYQ1lWR0ZKblZvTUQ4a0hUdFlsTjVJdUxVYWRsCjBzL0xWUUtCZ1FETTFwQlk2ME9VT1Zxenc4WG1QTGtuOWV4dlhicm9EQmNrRVd6WTlyczRpRFd2SnFyR21KdEIKVW9KYXlPaC9BcTd6bGFpK0FVYlpjMjNtN3VrSnRMdlFLY2NlVWZFbzVYVkc5NVM3M21QVVkzYXBabXlXZmNtTgpzOUpHK3IyVk9WREFJa0lrcGlWVW1NR0xwb1RXNTdxTHhsYzJQR0sxbk9HcnJReDQ2anhrZEE9PQotLS0tLUVORCBSU0EgUFJJVkFURSBLRVktLS0tLQo=
```



使用--kubeconfig= 指定生成的config文件路径，测试连接K8S集群

```
[root@node01 ssl]# kubectl --kubeconfig=/root/.kube/config get nodes
NAME          STATUS   ROLES    AGE   VERSION
k8s-master1   Ready    <none>   10d   v1.20.4
k8s-node1     Ready    <none>   10d   v1.20.4
k8s-node2     Ready    <none>   10d   v1.20.4
```



### 把生成的kubeconfig配置文件存放在jenkins 中

Manage Jenkins -> Managed files -> Add a new Config -> Custom file（自定义文件）

输入 Name： k8s-kubeconfig

把生成的kubeconfig 文件内容 复制到 Content 文本框中

![image-20210415091854554](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210415091854.png)

![image-20210415091911829](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210415091911.png)

![image-20210415091949467](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210415091949.png)

![image-20210415092052757](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210415092052.png)

将自签名的 harbor 证书的 key/crt/cert 也同样导入到 Managed files

![image-20210417151920204](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210417151920.png)

### 把Managed files 中的配置文件 转换成pipeline 语法

直接打开 http://192.168.110.235:30008/pipeline-syntax/就可以了，不需要创建 job

点击片段生成器，Sample Step 下拉选项框中 选择 configFileProvider: Provide Configuration files

在File 下拉选项框中，选中 刚刚创建的 自定义文件 k8s-kubeconfig

在Target 中 输入 admin.kubeconfig (target 表示把 自定义的文件 挂载到 jenkin-slave 镜像的什么路径下，这边定义了文件名称，就相当于把 admin.kubeconfig 文件 放在 jenkin-slave 镜像 的默认工作路径下/home/jenkins/agent/workspace/XX 下面 )

最后点击 Generate Pipeline Script 生产pipeline 语法

```
configFileProvider([configFile(fileId: 'ac20bd78-327d-41b1-952f-ad15bc07e4ba', targetLocation: 'admin.kubeconfig')]) {
    // some block
}
```



![image-20210415093714811](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210415093714.png)



### 编写标准的deploy.yaml 模板

本项目是 使用Jenkins在Kubernetes中持续部署一个无状态的tomcat pod 应用；涉及到 deployment 控制器 以及采用NodePort 的方式去 访问pod

deployment.yaml 和 service.yaml 我把他合并在一个deploy.yaml 文件中

另外deploy.yaml 文件 必须和 项目的代码 在同一个路径下（否则kubectl 无法指定yaml 文件就 无法创建pod）

```
cat > deploy.yaml << EOF
apiVersion: apps/v1
kind: Deployment
metadata:
  name: tomcat-java-demo
spec:
  replicas: REPLICAS
  selector:
    matchLabels:
      project: www
      app: tomcat-java-demo
  template:
    metadata:
      labels:
        project: www
        app: tomcat-java-demo
    spec:
      imagePullSecrets:
      - name: SECRET_NAME
      containers:
      - image: IMAGE_NAME
        name: tomcat-java-demo
        resources:
          requests:
            cpu: 0.5
            memory: 500Mi
          limits: 
            cpu: 1
            memory: 1Gi
        livenessProbe:
          httpGet:
            path: /
            port: 8080
          initialDelaySeconds: 50
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /
            port: 8080
          initialDelaySeconds: 50
          periodSeconds: 10


---
apiVersion: v1
kind: Service
metadata:
  name: tomcat-java-demo 
spec:
  selector:
    project: www
    app: tomcat-java-demo
  type: NodePort
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
EOF
```

### 在K8S 集群中创建sercret 用于连接Harbor仓库（用于K8S创建POD时 拉取镜像）

备注：

应用启动过程中可能需要一些敏感信息，比如访问数据库的用户名密码或者秘钥。将这些信息直接保存在容器镜像中显然不妥，Kubernetes 提供的解决方案是 Secret。

Secret 会以密文的方式存储数据，避免了直接在配置文件中保存敏感信息。Secret 会以 Volume 的形式被 mount 到 Pod，容器可通过文件的方式使用 Secret 中的敏感数据；此外，容器也可以环境变量的方式使用这些数据。

找一台登陆过的Harbor 的 node 的节点，查看 cat ~/.docker/config.json 问价里的内容

```
[root@node01 ssl]# cat ~/.docker/config.json 
{
        "auths": {
                "192.168.110.239": {
                        "auth": "YWRtaW46SGFyYm9yMTIzNDU="
                }
        },
        "HttpHeaders": {
                "User-Agent": "Docker-Client/19.03.9 (linux)"
        }
}
```

使用 base64 对 ~/.docker/config.json 文件 进行编码

```
[root@node01 ssl]# cat ~/.docker/config.json | base64 |tr -d "\n"
ewoJImF1dGhzIjogewoJCSIxOTIuMTY4LjExMC4yMzkiOiB7CgkJCSJhdXRoIjogIllXUnRhVzQ2U0dGeVltOXlNVEl6TkRVPSIKCQl9Cgl9LAoJIkh0dHBIZWFkZXJzIjogewoJCSJVc2VyLUFnZW50IjogIkRvY2tlci1DbGllbnQvMTkuMDMuOSAobGludXgpIgoJfQp9
```

创建secret yaml文件

**Tips**：注意base64 的结果要写成一行

```
cat > registry-pull-secret.yaml << EOF
apiVersion: v1
kind: Secret
metadata:
  name: secret-dockercfg
type: kubernetes.io/dockerconfigjson
data:
  .dockerconfigjson: |
        ewoJImF1dGhzIjogewoJCSIxOTIuMTY4LjExMC4yMzkiOiB7CgkJCSJhdXRoIjogIllXUnRhVzQ2U0dGeVltOXlNVEl6TkRVPSIKCQl9Cgl9LAoJIkh0dHBIZWFkZXJzIjogewoJCSJVc2VyLUFnZW50IjogIkRvY2tlci1DbGllbnQvMTkuMDMuOSAobGludXgpIgoJfQp9
        
EOF        
```

在 指定的 ns 命名空间下 执行 kubectl apply 创建 Secret：

```
for i in {prod,dev};do kubectl create namespace $i;done
for i in {prod,dev};do kubectl  apply -f registry-pull-secret.yaml -n $i;done

```

查看创建的 secret

```
[root@node01 ssl]# for i in {prod,dev};do kubectl get secret -n $i;done
NAME                  TYPE                                  DATA   AGE
default-token-nhtmq   kubernetes.io/service-account-token   3      51m
secret-dockercfg      kubernetes.io/dockerconfigjson        1      51m
NAME                  TYPE                                  DATA   AGE
default-token-p44zj   kubernetes.io/service-account-token   3      51m
secret-dockercfg      kubernetes.io/dockerconfigjson        1      51m
```



### 定义环境变量，使用参数化构建，修改deploy.yaml文件

需要提前定义好的环境变量：

```
//定义harbor的地址
def registry = "192.168.110.239"

// 项目，BUILD_NUMBER jenkins 内置变量，jenkins 构建编号
def project = "java"
def app_name = "tomcat-java-demo"
def image_name = "${registry}/${project}/${app_name}:${BUILD_NUMBER}"
def git_address = "http://192.168.110.238/root/tomcat-java-demo.git"

// 认证
//k8s 连接harbor 证书
def secret_name = "registry-pull-secret"

//jenkins中定义docker连接harbor的用户密码凭证，全局凭据里面的 ID
def docker_registry_auth = "5079d562-6f41-455a-9f7d-5a7bd9a17a44"

//jenkins中定义git连接gitlab的用户密码凭证，全局凭据里面的 ID
def git_auth = "0ddfe45b-a247-492c-a71b-2ceed7704663"

//jenkins中Config File Provider插件 定义的kubeconfig 文件内容
def k8s_auth = "ac20bd78-327d-41b1-952f-ad15bc07e4ba"
```

![image-20210415134448038](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210415134448.png)



![image-20210415134352169](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210415134352.png)

参数化构建过程中，需要交互的内容：

```
发布分支(prod,dev)
副本数（1,3,5,7）
命名空间（prod,dev）
```

一个标准的deploy.yaml模板文件，需要把修改的内容：

```
#修改deploy.yaml 中镜像名称
sed -i 's#IMAGE_NAME#${image_name}#' deploy.yaml

#修改deploy.yaml 中k8s连接harbor连接的secret
sed -i 's#SECRET_NAME#${secret_name}#' deploy.yaml

#修改deploy.yaml 中副本的数量
sed -i 's#REPLICAS#${ReplicaCount}#' deploy.yaml

#指定pod 创建在哪个命名空间下
kubectl apply -f deploy.yaml -n ${Namespace} --kubeconfig=admin.kubeconfig  
```

pipeline中引用示例：

```
//参数化构建
parameters {    
        gitParameter branch: '', branchFilter: '.*', defaultValue: 'master', description: '选择发布的分支', name: 'Branch', quickFilterEnabled: false, selectedValue: 'NONE', sortMode: 'NONE', tagFilter: '*', type: 'PT_BRANCH'
        choice (choices: ['1', '3', '5', '7'], description: '副本数', name: 'ReplicaCount')
        choice (choices: ['prod','dev'], description: '命名空间', name: 'Namespace')
    }
    

//部署pod需要修改的内容
sh """
sed -i 's#IMAGE_NAME#${image_name}#' deploy.yaml
sed -i 's#SECRET_NAME#${secret_name}#' deploy.yaml
sed -i 's#REPLICAS#${ReplicaCount}#' deploy.yaml
kubectl apply -f deploy.yaml -n ${Namespace} --kubeconfig=admin.kubeconfig  
"""
```

### 

## 完整的pipeline 流水线 脚本

```
// 公共
def registry = "192.168.110.239"
// 项目，BUILD_NUMBER jenkins 内置变量，jenkins 构建编号
def project = "java"
def app_name = "tomcat-java-demo"
def image_name = "${registry}/${project}/${app_name}:${BUILD_NUMBER}"
def git_address = "http://192.168.110.238/root/tomcat-java-demo.git"
// 认证
//k8s 连接harbor 证书
def secret_name = "registry-pull-secret"

//jenkins中定义docker连接harbor的用户密码凭证，全局凭据里面的 ID
def docker_registry_auth = "5079d562-6f41-455a-9f7d-5a7bd9a17a44"

//jenkins中定义git连接gitlab的用户密码凭证，全局凭据里面的 ID
def git_auth = "0ddfe45b-a247-492c-a71b-2ceed7704663"

//jenkins中Config File Provider插件 定义的kubeconfig 文件内容
def k8s_auth = "ac20bd78-327d-41b1-952f-ad15bc07e4ba"

pipeline {
    agent {
        kubernetes {
            yaml '''
apiVersion: v1          
kind: Pod
metadata:
  name: jenkins-slave
spec:
  // 我的 jenkins-slave 不是用 root 用户运行的，查过 root 的 uid 在 docker 容器内部是 0，所以在执行 k8s yml 的时候选择指定 uid=0 的 root 用户执行命令行工具，否则会报权限错误
  securityContext:  
    runAsUser: 0
    runAsGroup: 0
  containers:
  - name: jnlp
    image: ${registry}/library/jenkins-slave:4.7.1-alpine
    imagePullPolicy: Always
    volumeMounts:
      - name: docker-cmd
        mountPath: /usr/bin/docker
      - name: docker-sock
        mountPath: /var/run/docker.sock
      - name: maven-cache
        mountPath: /root/.m2
    securityContext:
      allowPrivilegeEscalation: false
  volumes:
    - name: docker-cmd
      hostPath:
        path: /usr/bin/docker
    - name: docker-sock
      hostPath:
        path: /var/run/docker.sock
    - name: maven-cache
      persistentVolumeClaim:
        claimName: mavencache
'''
           
        }
    }
	 parameters {    
        gitParameter branch: '', branchFilter: '.*', defaultValue: 'master', description: '选择发布的分支', name: 'Branch', quickFilterEnabled: false, selectedValue: 'NONE', sortMode: 'NONE', tagFilter: '*', type: 'PT_BRANCH'
        choice (choices: ['1', '3', '5', '7'], description: '副本数', name: 'ReplicaCount')
        choice (choices: ['prod','dev'], description: '命名空间', name: 'Namespace')
    }

    stages {		
		stage('拉取代码'){
            steps {
                checkout([$class: 'GitSCM', 
                branches: [[name: "${params.Branch}"]], 
                doGenerateSubmoduleConfigurations: false, 
                extensions: [], submoduleCfg: [], 
                userRemoteConfigs: [[credentialsId: "${git_auth}", url: "${git_address}"]]
                ])
            }
        }
		
        stage('代码编译') {
            steps {
                sh 'mvn clean package -Dmaven.skip.test=true'
                sh 'ls -l target/'
                sh 'pwd'
            }
        }
        stage('构建镜像并推送仓库') {
             steps {
                withCredentials([usernamePassword(credentialsId: "${docker_registry_auth}", passwordVariable: 'password', usernameVariable: 'username')]) {
                sh """
                docker build -t ${image_name} .
                docker login -u ${username} -p '${password}' ${registry}
                docker push ${image_name}
                """
                }
           } 
			
        }
        stage('部署到K8S平台'){
             steps {
              configFileProvider([configFile(fileId: "${k8s_auth}", targetLocation: "admin.kubeconfig")]){
                sh """
                  sed -i 's#IMAGE_NAME#${image_name}#' deploy.yaml
                  sed -i 's#SECRET_NAME#${secret_name}#' deploy.yaml
                  sed -i 's#REPLICAS#${ReplicaCount}#' deploy.yaml
                  kubectl apply -f deploy.yaml -n ${Namespace} --kubeconfig=admin.kubeconfig
                  sleep 120
                  kubectl get pods,svc -n ${Namespace} --kubeconfig=admin.kubeconfig
                """
              }
          }
        }
    }
}
```



```
[root@node01 docker-inbound-agent]# kubectl get pod,svc -n prod
NAME                                    READY   STATUS    RESTARTS   AGE
pod/tomcat-java-demo-5dfd566b49-cbdlm   1/1     Running   0          4m35s

NAME                       TYPE       CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
service/tomcat-java-demo   NodePort   10.0.0.222   <none>        80:32088/TCP   21h
```



![image-20210416202537732](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210416202537.png)

部署成功，查看 tomcat 应用

![image-20210416202515414](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210416202515.png)



# 参考

- 摘抄至，略有删改。https://github.com/fxkjnj/kubernetes/tree/main/jenkins-for_kubernetes/jenkins_deploy_kubernetes

- https://blog.51cto.com/u_14143894/2437186
