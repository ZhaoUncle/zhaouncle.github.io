# 【k8s】pv 处在 Terminating 状态


<!--more-->

# kubectl delete pv 之后，pv 一直处在 `Terminating` 状态

问题现状

```
[root@node01 efk]# kubectl get pv -n efk|grep elastic
pvc-10cfbb1c-19fd-4b93-bddc-416c5ad1c977   50Gi       RWO            Retain           Terminating   efk/elasticsearch-data-elasticsearch-data-1       elastic-nfs-client             14h
pvc-7164e842-5e6c-4a94-89fe-0e649713a6b7   50Gi       RWO            Retain           Terminating   efk/elasticsearch-data-elasticsearch-data-0       elastic-nfs-client             14h
pvc-8ea92bf8-945c-46ed-8073-a5f2971f52fe   5Gi        RWO            Retain           Terminating   efk/elasticsearch-master-elasticsearch-master-1   elastic-nfs-client             14h
pvc-c9527589-9282-40b3-b0a6-a168412528df   5Gi        RWO            Retain           Terminating   efk/elasticsearch-master-elasticsearch-master-0   elastic-nfs-client             14h
```

解决办法：

办法一：直接到etcd中删除

1. 将所有的etcd中的key值取到一个keys.yam里面，便于查询

   ```
   ETCDCTL_API=3 /opt/etcd/bin/etcdctl --cacert=/opt/etcd/ssl/ca.pem --cert=/opt/etcd/ssl/server.pem --key=/opt/etcd/ssl/server-key.pem --endpoints=https://192.168.110.235:2379,https://192.168.110.236:2379,https://192.168.110.237:2379 get "" --from-key > keys.yaml
   ```

   

2. `vim keys.yaml` 找到`pvc-10cfbb1c-19fd-4b93-bddc-416c5ad1c977`，目标 pv 找到以 **/registry/** 开头 `pvc-10cfbb1c-19fd-4b93-bddc-416c5ad1c977`结尾的一串记录

   ```
   cat keys.yaml |grep pvc-10cfbb1c-19fd-4b93-bddc-416c5ad1c977|grep -a registry
   ```

3. 删除记录

```
ETCDCTL_API=3 /opt/etcd/bin/etcdctl --cacert=/opt/etcd/ssl/ca.pem --cert=/opt/etcd/ssl/server.pem --key=/opt/etcd/ssl/server-key.pem --endpoints=https://192.168.110.235:2379,https://192.168.110.236:2379,https://192.168.110.237:2379 del /registry/persistentvolumes/pvc-10cfbb1c-19fd-4b93-bddc-416c5ad1c977
```



办法二：使用 `kubectl patch`

```
kubectl patch pv pvc-7164e842-5e6c-4a94-89fe-0e649713a6b7 -p '{"metadata":{"finalizers":null}}'
kubectl patch pv pvc-8ea92bf8-945c-46ed-8073-a5f2971f52fe -p '{"metadata":{"finalizers":null}}'
kubectl patch pv pvc-c9527589-9282-40b3-b0a6-a168412528df -p '{"metadata":{"finalizers":null}}'
```




