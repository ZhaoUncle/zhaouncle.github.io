# 【k8s】java 微服务部署示例


<!--more-->

# 1. 环境

- k8s：1.20

  | k8s-master1 | 192.168.110.235 |
  | ----------- | --------------- |
  | 8s-node1    | 192.168.110.236 |
  | k8s-node2   | 192.168.110.237 |
  | harbor,nfs  | 192.168.110.239 |

- java：1.8

# 2. java 微服务部署

##2.1 安装 java jdk1.8

```
yum -y install java-1.8.0-openjdk java-1.8.0-openjdk-devel maven
```

##2.2 下载代码示例：

###版本介绍：

- dev3 分支会 mysql 连接错误，暂时无法解决请抛弃
- dev4 分支是加了 pinpoint APM 服务
- dev4-skywalking 分支如所见

```
git clone git@github.com:ZhaoUncle/simple-microservice.git
#切换dev4-skywalking 分支
cd simple-microservice
git checkout remotes/origin/dev4-skywalking
```

> ## 部署须知

1. 导入db目录下数据库文件到自己的MySQL服务器
   - 默认的 db 文件，没有 use DATABASES；所以会导致报错，参考 2.4 做法。
2. k8s/docker_build.sh 这个是部署脚本，可以参考2.6来使用，用来推送 build 好的 images，然后启动 deployment。
3. 修改配置环境（xxx-service/src/main/resources/application.yml，active值决定启用环境配置文件）
   - 默认使用 fat 文件调用 mysql 和 eureka 链接，无需更改。
4. 修改连接数据库配置（xxx-service/src/main/resources/application-fat.yml）
   - 参考 2.5 做法
5. 修改前端页面连接网关地址（portal-service/src/main/resources/static/js/productList.js和orderList.js）
   - 默认无需修改，不过当你访问 portal，配置的是 ingress 暴露的域名，具体可以看 k8s/目录下的 yaml 文件。
6. 服务启动顺序：eureka -> mysql -> product,stock,order -> gateway -> portal
7. 访问办法：
   - `kubectl get pod,svc,ing -n ms`可以看出是由 nginx-ingress-controller暴露的域名访问，
   - 绑定 hosts进行访问，192.168.110.235是我的 master IP，你也可以绑定为 node IP：`192.168.110.235 eureka.ctnrs.com gateway.ctnrs.com portal.ctnrs.com `

### 构建 java 命令介绍

```
mvn clean package -D maven.test.skip=true
```

>mvn clean package：清除目录中生成的结果，做一个清除，重新打新的包。
>
>-D maven.test.skip: 跳过单元测试，写的测试用例，如果写的有问题，是编译不过去的



##2.3 helm 安装 mysql 介绍

```
mkdir -p /root/helm/mysql
cd /root/helm/mysql

# vi config.yaml 
persistence: #如果你只是简单测试，可以把 persistence 这里的所有配置都去掉，只保留 service 的配置即可。
  enabled: true
  storageClass: "managed-nfs-storage"
  accessMode: ReadWriteOnce
  size: 8Gi

service:
  type: NodePort #暴露 nodeport，可以用宿主机 ip+port 访问
  nodePort: 32000

#helm 创建 db 
helm install db -f config.yaml  stable/mysql

#查看发布状态，这里会显示获取密码、host、port 的命令，下面那条获取 root 密码的命令就是在这里面看到的
helm status db 

#查看mysql 的 root 密码：
kubectl get secret --namespace default db-mysql -o jsonpath="{.data.mysql-root-password}" | base64 --decode; echo
```

一步登录 mysql：这里的命令和下面的一步登录 mysql 步骤不能与你通用，具体要由上面那条`helm status`得出，因为 pod name 不一定一样，如果你创建的不是这个名字的话。

```
MYSQL_ROOT_PASSWORD=$(kubectl get secret --namespace default db-mysql -o jsonpath="{.data.mysql-root-password}" | base64 --decode; echo)
MYSQL_HOST=$(kubectl get nodes --namespace default -o jsonpath='{.items[0].status.addresses[0].address}')
MYSQL_PORT=$(kubectl get svc --namespace default db-mysql -o jsonpath='{.spec.ports[0].nodePort}')
mysql -h ${MYSQL_HOST} -P${MYSQL_PORT} -u root -p${MYSQL_ROOT_PASSWORD}
```

创建结果：

```
[root@node01 mysql]# kubectl get secret --namespace default db-mysql -o jsonpath="{.data.mysql-root-password}" | base64 --decode; echo
W5hcn4PvTI
[root@node01 mysql]# kubectl get pod,svc |grep db-mysql
pod/db-mysql-7f897f4d95-bc7js               1/1     Running     0          80s
service/db-mysql     NodePort    10.0.0.121   <none>        3306:32000/TCP   80s

[root@node01 mysql]# helm list|grep db
db      default         1               2021-04-06 22:04:19.545636699 +0800 CST deployed        mysql-1.6.9     5.7.30     
```

删除你所创建的 db 数据库

```
helm delete db #kubectl get pod 就会就看不见了
```



## 2.4 mysql 登录然后创建 db 和 tables 数据

```
MySQL [(none)]> CREATE DATABASE /*!32312 IF NOT EXISTS*/`tb_order` /*!40100 DEFAULT CHARACTER SET utf8mb4 */;
MySQL [(none)]> use tb_order;
MySQL [(none)]> source /data/simple-microservice/db/order.sql

MySQL [(none)]> CREATE DATABASE /*!32312 IF NOT EXISTS*/`tb_product` /*!40100 DEFAULT CHARACTER SET utf8mb4 */;
MySQL [(none)]> use tb_product;
MySQL [(none)]> source /data/simple-microservice/db/product.sql


MySQL [(none)]> CREATE DATABASE /*!32312 IF NOT EXISTS*/`tb_stock` /*!40100 DEFAULT CHARACTER SET utf8mb4 */;
MySQL [(none)]> use tb_stock;
MySQL [(none)]> source /data/simple-microservice/db/stock.sql
```

虽然 sql 文件介绍说使用的mysql5.6 的版本，但是经过测试 5.7 这里是正常使用，所以你想用 helm 安装 mysql5.6，可以参考另一篇文章[K8s环境 Helm 安装 mysql 多版本]()



## 2.5 修改配置文件密码和 mysql ip 端口，创建 namesapce

语法介绍：`sed -i 's#abc#def#g' filename`，将 filename 文件里面所有的 abc 替换成 def。

```
cd /data/simple-microservice

### 修改 docker_build.sh 里面的 register 为我们创建的 harbor 仓库地址
sed -i 's#docker_registry=192.168.31.90#docker_registry=192.168.110.239#g' k8s/docker_build.sh 

### 创建 namespace ms
kubectl create namespace ms

### 修改 jdbc 的 mysql 连接
for i in `find ./ -name "application-fat.yml"`;do sed -i 's#mysql:3306#192.168.110.235:32000#g' $i;done


###修改 root 密码，2.3有告诉我们 root 密码是多少哦，当然，你也可以进 mysql 或者修改 helm 的 config.yaml 定义自己的初始密码，这里就不多做介绍了
for i in `find ./ -name "application-fat.yml"`;do  sed -i 's#password: 123456#password: W5hcn4PvTI#g' $i;done

### 我们还要修改 skywalking 的 ip+端口，skywalking 用的是 nodeport 的部署方式，skywalking 尽管在 java 启动命令中被调用，但是就算不存在，java 服务也不会退出。
for i in `find ./ -name "Dockerfile"`;do  sed -i 's#192.168.31.90:11800#192.168.110.235:11800#g' $i;done


```





## 2.6  docker_build.sh 介绍

`vi /data/simple-microservice/k8s/docker_build.sh `

```
#!/bin/bash

###这里是 harbor 地址
docker_registry=192.168.110.239  

###创建 harbor 登录secret，以后就不需要每次都手动登录了
kubectl create secret docker-registry registry-pull-secret --docker-server=$docker_registry --docker-username=admin --docker-p
assword=Harbor12345 --docker-email=admin@ctnrs.com -n ms

### 下面是构建 mvn jar 包然后上传到 harbor，在部署 k8s yaml
service_list="eureka-service gateway-service order-service product-service stock-service portal-service"
service_list=${1:-${service_list}}
work_dir=$(dirname $PWD)
current_dir=$PWD

cd $work_dir
mvn clean package -Dmaven.test.skip=true

for service in $service_list; do
   cd $work_dir/$service
   if ls |grep biz &>/dev/null; then
      cd ${service}-biz
   fi
   service=${service%-*}
   image_name=$docker_registry/microservice/${service}:$(date +%F-%H-%M-%S)
   docker build -t ${image_name} .
   docker push ${image_name}
   sed -i -r "s#(image: )(.*)#\1$image_name#" ${current_dir}/${service}.yaml
   kubectl apply -f ${current_dir}/${service}.yaml
done

```

## 2.7 部署 ms java 服务

部署：

```
cd /data/simple-microservice/k8s
sh docker_build.sh
```

删除：

```
cd /data/simple-microservice/k8s
kubectl delete -f *.yaml
```

部署结果：

```
[root@node01 k8s]# kubectl get pod,svc,ing -n ms
NAME                           READY   STATUS    RESTARTS   AGE
pod/eureka-0                   0/1     Running   0          3s
pod/eureka-1                   1/1     Running   0          3m2s
pod/eureka-2                   1/1     Running   0          6m53s
pod/gateway-7b47c8d997-znkfx   1/1     Running   0          7m49s
pod/order-59f6d5d786-htxtg     1/1     Running   0          7m36s
pod/portal-6c79b876c9-h8vsd    1/1     Running   0          6m27s
pod/product-8c88fbf8-hm7k6     1/1     Running   0          6m51s
pod/stock-8944b6585-ccp8w      1/1     Running   0          6m51s

NAME              TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)    AGE
service/eureka    ClusterIP   None         <none>        8888/TCP   6h17m
service/gateway   ClusterIP   10.0.0.113   <none>        9999/TCP   6h17m
service/portal    ClusterIP   10.0.0.104   <none>        8080/TCP   6h15m

NAME                                CLASS    HOSTS               ADDRESS   PORTS   AGE
ingress.networking.k8s.io/eureka    <none>   eureka.ctnrs.com              80      6h17m
ingress.networking.k8s.io/gateway   <none>   gateway.ctnrs.com             80      6h17m
ingress.networking.k8s.io/portal    <none>   portal.ctnrs.com              80      6h15m
```

由 ing 那里得知，需要绑定的 hosts 如下；

`192.168.110.235 eureka.ctnrs.com gateway.ctnrs.com portal.ctnrs.com `

![image-20210406234746629](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210406234746.png)



![image-20210406234940735](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210406234940.png)

![image-20210406235002238](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210406235002.png)

![image-20210406235023413](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210406235023.png)





# 3. 接入 skywalking

## 3.1 Skywalking server 版部署，请参考

[K8s环境 Helm部署skywalking](https://zhaouncle.com/k8s%E7%8E%AF%E5%A2%83-helm%E9%83%A8%E7%BD%B2skywalking/)

## 3.2 下载 skywalking 8.4.0版本

>我们主要使用 agen 端，一定要与 server 端版本对应

````
cd /data
wget https://mirror-hk.koddos.net/apache/skywalking/8.4.0/apache-skywalking-apm-es7-8.4.0.tar.gz
tar xf apache-skywalking-apm-es7-8.4.0.tar.gz 

### 这条是删除命令，你们可别乱来。。。。
for i in `find  /data/simple-microservice/ -name skywalking`;do rm -rf $i && cp -rp /data/apache-skywalking-apm-bin-es7/agent/ $i;done
````



## 3.3 修改 Dockerfile 中 java 的启动命令

之前我们改成了 ip+nodeport 的形式，现在改回去直接 k8s 内部 serviceName+namespace 的访问形式。

```
for i in `find ./ -name "Dockerfile"`;do  sed -i 's#192.168.31.90:11800#skywalking-oap.skywalking:11800#g' $i;done
```

skywalking 的 svc 状态如下，只要 oap 的接口

```
[root@node01 k8s]# kubectl get svc -n skywalking
NAME             TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)               AGE
skywalking-oap   ClusterIP   10.0.0.3     <none>        12800/TCP,11800/TCP   5h22m
skywalking-ui    NodePort    10.0.0.232   <none>        80:30008/TCP          5h22m
```

## 3.4 查看 skywalking 的 java 微服务

![image-20210412000307837](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210412000307.png)

# 4. 参考

- https://github.com/lizhenliang/simple-microservice
