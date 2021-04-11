# 【k8s】Helm 安装 mysql 多版本


<!--more-->



#1. helm 下载 mysql charts 模板

##1.1 查看有哪些 helm 版本

`helm  search repo mysql`

```
[root@node01 helm]# helm  search repo mysql
WARNING: Kubernetes configuration file is group-readable. This is insecure. Location: /root/.kube/config
WARNING: Kubernetes configuration file is world-readable. This is insecure. Location: /root/.kube/config
NAME                                    CHART VERSION   APP VERSION     DESCRIPTION                                       
aliyun/mysql                            0.3.5                           Fast, reliable, scalable, and easy to use open-...
stable/mysql                            1.6.9           5.7.30          DEPRECATED - Fast, reliable, scalable, and easy...
stable/mysqldump                        2.6.2           2.4.1           DEPRECATED! - A Helm chart to help backup MySQL...
stable/prometheus-mysql-exporter        0.7.1           v0.11.0         DEPRECATED A Helm chart for prometheus mysql ex...
aliyun/percona                          0.3.0                           free, fully compatible, enhanced, open source d...
aliyun/percona-xtradb-cluster           0.0.2           5.7.19          free, fully compatible, enhanced, open source d...
stable/percona                          1.2.3           5.7.26          DEPRECATED - free, fully compatible, enhanced, ...
stable/percona-xtradb-cluster           1.0.8           5.7.19          DEPRECATED - free, fully compatible, enhanced, ...
stable/phpmyadmin                       4.3.5           5.0.1           DEPRECATED phpMyAdmin is an mysql administratio...
aliyun/gcloud-sqlproxy                  0.2.3                           Google Cloud SQL Proxy                            
aliyun/mariadb                          2.1.6           10.1.31         Fast, reliable, scalable, and easy to use open-...
stable/gcloud-sqlproxy                  0.6.1           1.11            DEPRECATED Google Cloud SQL Proxy                 
stable/mariadb                          7.3.14          10.3.22         DEPRECATED Fast, reliable, scalable, and easy t...
```

## 1.2 helm 下载：

`helm pull repo/server`，旧版本用`helm fetch repo/server`

```
# 下载后解压为 mysql-5.6 的目录
helm pull stable/mysql  --untar 
mv mysql mysql-5.6

# 下载后不解压
helm pull stable/mysql	#会下载mysql-1.6.9.tgz


# 下载 chart version 为1.6.0 的 stable/mysql
helm  pull stable/mysql --version 1.6.0  #会下载mysql-1.6.0.tgz
```

#2. 更改版本

**Tips：**由 1.1 可知，我们下载的 stable/mysql 是 mysql5.7 的版本，以下方式可以不记住 charts 版本号就可以修改 mysql 版本

## 2.1 修改 Chart.yaml

修改 mysql 版本号`appVersion` 由原来的 5.7.30 改为 5.6.50和 chart 版本号`version`

`vi mysql-5.6/Chart.yaml `

```
apiVersion: v1
appVersion: 5.6.50  #更改为 mysql 你要的版本号
deprecated: true
description: DEPRECATED - Fast, reliable, scalable, and easy to use open-source relational
  database system.
home: https://www.mysql.com/
icon: https://www.mysql.com/common/logos/logo-mysql-170x115.png
keywords:
- mysql
- database
- sql
name: mysql
sources:
- https://github.com/kubernetes/charts
- https://github.com/docker-library/mysql
version: 5.6.50  #这里是 mysql 的 chart 版本号，随意修改不重复即可
```

##2.2 修改mysql 的配置参数

` vi mysql-5.6/values.yaml `，以下是我净化后的配置

```
image: "mysql"
imageTag: "5.6.50"   #这里要修改为 mysql 你需要的版本，与上面修改的版本保持一致
strategy:
  type: Recreate
busybox:
  image: "busybox"
  tag: "1.32"
testFramework:
  enabled: true
  image: "bats/bats"
  tag: "1.2.1"
  imagePullPolicy: IfNotPresent
  securityContext: {}
mysqlRootPassword: mysql123456		#主动设置 root 密码，要大于 10 个字符串
mysqlUser: test				# 设置一个初始账号哦
mysqlPassword: test123456		#👆🏻那个账号的密码，要大于 10 个字符串
imagePullPolicy: IfNotPresent
args: []
extraVolumes: |
extraVolumeMounts: |
extraInitContainers: |
nodeSelector: {}
affinity: {}
tolerations: []
livenessProbe:
  initialDelaySeconds: 30
  periodSeconds: 10
  timeoutSeconds: 5
  successThreshold: 1
  failureThreshold: 3
readinessProbe:
  initialDelaySeconds: 5
  periodSeconds: 10
  timeoutSeconds: 1
  successThreshold: 1
  failureThreshold: 3
persistence:
  enabled: true
  storageClass: "managed-nfs-storage"  #我这里设置为 nfs 的动态 sc，会自动创建存储，可参考 nfs 那章节
  accessMode: ReadWriteOnce
  size: 8Gi
  annotations: {}
securityContext:
  enabled: false
  runAsUser: 999
  fsGroup: 999
resources:
  requests:
    memory: 256Mi
    cpu: 100m
configurationFilesPath: /etc/mysql/conf.d/
configurationFiles: {}
initializationFiles: {}
mysqlx:
  port:
    enabled: false
metrics:
  enabled: false
  image: prom/mysqld-exporter
  imageTag: v0.10.0
  imagePullPolicy: IfNotPresent
  resources: {}
  annotations: {}
  livenessProbe:
    initialDelaySeconds: 15
    timeoutSeconds: 5
  readinessProbe:
    initialDelaySeconds: 5
    timeoutSeconds: 1
  flags: []
  serviceMonitor:
    enabled: false
    additionalLabels: {}
service:
  annotations: {}
  type: NodePort	# 我这里的 service 设置为 nodeport 模式，可以用 master或者 node ip+下面的 nodePort 32005 进行访问	
  nodePort: 32005	#宿主机访问的端口
  port: 3306
serviceAccount:
  create: false
ssl:
  enabled: false
  secret: mysql-ssl-certs
  certificates:
deploymentAnnotations: {}
podAnnotations: {}
podLabels: {}
initContainer:
  resources:
    requests:
      memory: 10Mi
      cpu: 10m
```



# 3. 安装

```
helm package mysql-5.6
helm install mydb mysql-5.6.50.tgz
```



行动结果：

```
[root@node01 helm]# helm list
WARNING: Kubernetes configuration file is group-readable. This is insecure. Location: /root/.kube/config
WARNING: Kubernetes configuration file is world-readable. This is insecure. Location: /root/.kube/config
NAME    NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
mysql56 default         1               2021-04-06 21:26:26.6483062 +0800 CST   deployed        mysql-5.6.50    5.6.50     
[root@node01 helm]# kubectl get pod 
NAME                                    READY   STATUS      RESTARTS   AGE
mysql56-6c856cff5b-xxrz6                1/1     Running     0          94s
nfs-client-provisioner-cf84d684-brlfk   1/1     Running     11         2d7h

```



# 4. 查看默认参数和变量

```
[root@node01 helm]# helm status mysql56
WARNING: Kubernetes configuration file is group-readable. This is insecure. Location: /root/.kube/config
WARNING: Kubernetes configuration file is world-readable. This is insecure. Location: /root/.kube/config
NAME: mysql56
LAST DEPLOYED: Tue Apr  6 21:26:26 2021
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
MySQL can be accessed via port 3306 on the following DNS name from within your cluster:
mysql56.default.svc.cluster.local

To get your root password run:

    MYSQL_ROOT_PASSWORD=$(kubectl get secret --namespace default mysql56 -o jsonpath="{.data.mysql-root-password}" | base64 --decode; echo)

To connect to your database:

1. Run an Ubuntu pod that you can use as a client:

    kubectl run -i --tty ubuntu --image=ubuntu:16.04 --restart=Never -- bash -il

2. Install the mysql client:

    $ apt-get update && apt-get install mysql-client -y

3. Connect using the mysql cli, then provide your password:
    $ mysql -h mysql56 -p

To connect to your database directly from outside the K8s cluster:
    MYSQL_HOST=$(kubectl get nodes --namespace default -o jsonpath='{.items[0].status.addresses[0].address}')
    MYSQL_PORT=$(kubectl get svc --namespace default mysql56 -o jsonpath='{.spec.ports[0].nodePort}')

    mysql -h ${MYSQL_HOST} -P${MYSQL_PORT} -u root -p${MYSQL_ROOT_PASSWORD}
```



# 5. 登录mysql

如果你要在命令登录，而不是用上面例子的 docker 容器远程登录，记得在宿主机安装`mysql-client`哦

```
#查看密码
MYSQL_ROOT_PASSWORD=$(kubectl get secret --namespace default mysql56 -o jsonpath="{.data.mysql-root-password}" | base64 --decode; echo)
#查看 mysql host
MYSQL_HOST=$(kubectl get nodes --namespace default -o jsonpath='{.items[0].status.addresses[0].address}')
#查看 mysql 端口
MYSQL_PORT=$(kubectl get svc --namespace default mysql56 -o jsonpath='{.spec.ports[0].nodePort}')
#命令行登录
mysql -h ${MYSQL_HOST} -P${MYSQL_PORT} -u root -p${MYSQL_ROOT_PASSWORD}
```



行动结果：

```
[root@node01 helm]# #查看密码
[root@node01 helm]# MYSQL_ROOT_PASSWORD=$(kubectl get secret --namespace default mysql56 -o jsonpath="{.data.mysql-root-password}" | base64 --decode; echo)
#查看 mysql host
MYSQL_HOST=$(kubectl get nodes --namespace default -o jsonpath='{.items[0].status.addresses[0].address}')
#查看 mysql 端口
MYSQL_PORT=$(kubectl get svc --namespace default mysql56 -o jsonpath='{.spec.ports[0].nodePort}')
#命令行登录
mysql -h ${MYSQL_HOST} -P${MYSQL_PORT} -u root -p${MYSQL_ROOT_PASSWORD}[root@node01 helm]# #查看 mysql host
[root@node01 helm]# MYSQL_HOST=$(kubectl get nodes --namespace default -o jsonpath='{.items[0].status.addresses[0].address}')
[root@node01 helm]# #查看 mysql 端口
[root@node01 helm]# MYSQL_PORT=$(kubectl get svc --namespace default mysql56 -o jsonpath='{.spec.ports[0].nodePort}')
[root@node01 helm]# #命令行登录
[root@node01 helm]# mysql -h ${MYSQL_HOST} -P${MYSQL_PORT} -u root -p${MYSQL_ROOT_PASSWORD}
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MySQL connection id is 32
Server version: 5.6.50 MySQL Community Server (GPL)

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MySQL [(none)]> 
```



使用 test 账号登录测试：

```
[root@node01 hemysql -utest -h192.168.110.236 -P32005 -ptest123456Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MySQL connection id is 125
Server version: 5.6.50 MySQL Community Server (GPL)

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MySQL [(none)]> 
```


