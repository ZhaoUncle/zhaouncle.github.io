# 【k8s】Helm 部署 skywalking


<!--more-->

# 1. 前言

本实验文档基于单机 es7 作为 skywalking 的后端存储，使用 nfs 动态卷 storageclass，es 没有使用账号密码。

# 2. 环境

- k8s 集群：v1.20.4 版本

| k8s-master1 | 192.168.110.235 |
| ----------- | --------------- |
| k8s-node1   | 192.168.110.236 |
| k8s-node2   | 192.168.110.237 |
| nfs         | 192.168.110.239 |

- elasticsearch：7.12.0
- skywalking：
  - oap-server: 无状态服务后端，主要负责处理核心逻辑，可以简单理解为一个标准 java web 项目。
  - skywalking-ui: UI 前端，通过 graphql 连接 oap-server 提供用户查询等 UI 展示。
  - agent: 各种语言实现的 agent 负责抓取应用运行数据并上报给 oap-server，核心的指标上报来源。
  - DB: 各种数据库，负责存储 Skywalking 的指标数据，生产环境推荐 ES、TiDB、MySQL。



#3. nfs 安装

**参考**：[K8s环境 nfs 动态存储卷部署](https://zhaouncle.com/k8s%E7%8E%AF%E5%A2%83-nfs%E5%8A%A8%E6%80%81%E5%AD%98%E5%82%A8%E5%8D%B7%E9%83%A8%E7%BD%B2/)

# 4. elasticsearch 安装

##  4.1 安装 es 7.12.0

```
cat >  elasticsearch-single.yaml  << EOF
apiVersion: apps/v1
kind: Deployment
metadata:
  name: elasticsearch-single
  namespace: efk
  labels:
    k8s-app: elasticsearch-single
spec:
  replicas: 1
  selector:
    matchLabels:
      k8s-app: elasticsearch-single
  template:
    metadata:
      labels:
        k8s-app: elasticsearch-single
    spec:
      containers:
      - image: elasticsearch:7.12.0
        name: elasticsearch-single
        resources:
          limits:
            cpu: 2
            memory: 3Gi
          requests:
            cpu: 0.5 
            memory: 500Mi
        env:
          - name: "discovery.type"
            value: "single-node"
          - name: ES_JAVA_OPTS
            value: "-Xms512m -Xmx2g" 
        ports:
        - containerPort: 9200
          name: db
          protocol: TCP
        volumeMounts:
        - name: elasticsearch-data
          mountPath: /usr/share/elasticsearch/data
      volumes:
      - name: elasticsearch-data
        persistentVolumeClaim:
          claimName: es-pvc

---

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: es-pvc
  namespace: efk
spec:
#指定动态PV 名称
  storageClassName: "elastic-nfs-client"
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 10Gi

---

apiVersion: v1
kind: Service
metadata:
  name: elasticsearch-single
  namespace: efk
spec:
  ports:
  - port: 9200
    protocol: TCP
    targetPort: 9200
  selector:
    k8s-app: elasticsearch-single
EOF

kubectl apply -f elasticsearch-single.yaml 
```



# 5. 安装 skywalking

## 5.1 安装 helm

```
curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash
```

##5.2 初始化 skywalking 的 charts 配置

```
# clone helm 仓库
git clone https://github.com/apache/skywalking-kubernetes
cd skywalking-kubernetes/chart

# 即使使用外部 ES 也要添加这个 repo，否则会导致依赖错误
helm repo add elastic https://helm.elastic.co
helm dep up skywalking

# 创建 skywalking 的 namespace
kubectl create namespace skywalking
```



##5.3 配置 skywalking 的 vaules 配置参数：

初始化完成后需要自行调整配置文件，配置 oap-server 使用外部 ES，当然你也可以使用 values 自带的 es 的配置示例，这里不做过多介绍

```
cat > skywalking/values-my-es-01.yaml <<EOF
oap:
  image:
    tag: 8.4.0-es7 
  storageType: elasticsearch7

ui:
  image:
    tag: 8.4.0
  service:
    type: NodePort
    externalPort: 80
    internalPort: 8080
    nodePort: 30008
elasticsearch:
  enabled: false
  config: 
    host: elasticsearch-single.efk
    port:
      http: 9200
    #user: ""         # [optional]
    #password: "xxx"     # [optional]
EOF
```

##5.4 helm 安装 skywalking 8.4.0

```
helm install skywalking skywalking -n  skywalking  -f ./skywalking/values-my-es-01.yaml
```

###5.4.1 卸载方式：

```
helm uninstall skywalking -n skywalking
```

###5.4.2 持续查看 pod 安装进度

```
kubectl get pod -n skywalking -w
```

###5.4.3 对外暴露 skywalking 端口，临时，但是本篇我用了 NodePort 的方法开放了端口，生产中也可以使用 ingress 的方式开放

```
export POD_NAME=$(kubectl get pods --namespace skywalking -l "app=skywalking,release=skywalking,component=ui" -o jsonpath="{.items[0].metadata.name}")
kubectl port-forward $POD_NAME 8080:8080 --namespace skywalking
```

###5.4.4 查看 skywalking 的访问：其实就是 k8s master/node ip + nodeport

```
export NODE_PORT=$(kubectl get --namespace skywalking -o jsonpath="{.spec.ports[0].nodePort}" services skywalking-ui)
export NODE_IP=$(kubectl get nodes --namespace skywalking -o jsonpath="{.items[0].status.addresses[0].address}")
echo http://$NODE_IP:$NODE_PORT
```

##5.5 运行状态检查

```
[root@node01 chart]# kubectl get pod,svc -n skywalking
NAME                                  READY   STATUS      RESTARTS   AGE
pod/skywalking-es-init-22g88          0/1     Completed   0          115s
pod/skywalking-oap-687f98bd9b-p5d69   1/1     Running     0          114s
pod/skywalking-oap-687f98bd9b-wxffb   1/1     Running     0          114s
pod/skywalking-ui-6fd5544496-cmmf9    1/1     Running     0          115s

NAME                     TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)               AGE
service/skywalking-oap   ClusterIP   10.0.0.88    <none>        12800/TCP,11800/TCP   115s
service/skywalking-ui    NodePort    10.0.0.87    <none>        80:30553/TCP          115s
```



##5.6 结果：

es 多了一堆 index

![image-20210411182203952](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210411182204.png)

skywalking 访问状态

![image-20210411183850273](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210411183850.png)



# 6. 部署一个 java 微服务示例，并接入 skywalking

参考：[K8s java 微服务部署示例](https://zhaouncle.com/k8s%E7%8E%AF%E5%A2%83-java%E5%BE%AE%E6%9C%8D%E5%8A%A1%E9%83%A8%E7%BD%B2%E7%A4%BA%E4%BE%8B/)

#7. 参考：

- https://mp.weixin.qq.com/s?__biz=MzI3MTI2NzkxMA==&mid=2247495145&idx=1&sn=87c5ff37f836d53c98304a88870d2e6d&chksm=eac6ccc0ddb145d6192df1ec738b885f979a868f96dbd6116eb05953989e64a67713d78f039e&mpshare=1&scene=1&srcid=04098L9gzve10EptJBFeLVHq&sharer_sharetime=1617928300892&sharer_shareid=09464b4a0389b967659ba78076a1ef58#rd

  
