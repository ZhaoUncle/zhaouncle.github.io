# K8s环境 允许 master 节点运行 pod


<!--more-->

### 查看 所有 node 节点的调度

```
[root@node01 efk]# kubectl describe node|grep -E "Name:|Taints:"
Name:               k8s-master1
Taints:             node-role.kubernetes.io/master:NoSchedule
Name:               k8s-node1
Taints:             <none>
Name:               k8s-node2
Taints:             <none>
```



### 去除 k8s-master1 节点不允许配置的 label

```
kubectl taint node k8s-master1 node-role.kubernetes.io/master-
```



### 所有节点不允许调度的 label 去除

```
kubectl taint nodes --all node-role.kubernetes.io/master-
```



###重新设置 master 节点不允许调度 pod

```
kubectl taint node k8s-master1 node-role.kubernetes.io/master=:NoSchedule
```

污点可选参数

- NoSchedule: 一定不能被调度
- PreferNoSchedule: 尽量不要调度
- NoExecute: 不仅不会调度, 还会驱逐Node上已有的Pod
