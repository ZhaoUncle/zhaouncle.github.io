# Centos7 安装 ELK 7.6.2


<!--more-->



# 1. 初始化环境

## 1.0 初始化环境官网参考
<https://www.elastic.co/guide/en/elasticsearch/reference/current/system-config.html>

## 1.1 架构
| 主机 ip | hostname | 安装的服务 |
| ------ | --------- | -------- |
| 192.168.110.245 | node01 | es、kibana |
| 192.168.110.246 | node02 | es、logstash、filebeat、auditbeat |
| 192.168.110.247 | node03 | es、filebeat、auditbeat |

## 1.2 配置 hosts
```shell
cat << EOF >> /etc/hosts
192.168.110.245 node01
192.168.110.246 node02 
192.168.110.247 node03
EOF
```

## 1.3 修改 hostname
```shell
hostnamectl set-hostname node01
hostnamectl set-hostname node02
hostnamectl set-hostname node03
```

## 1.4 时间同步(联网状态)
```shell
yum -y install ntpdate
/bin/echo "* */1 * * * /usr/sbin/ntpdate pool.ntp.org > /dev/null 2>&1" >> /var/spool/cron/root
```
## 1.5 关闭 selinux
```shell
setenforce 0 #临时，马上生效
sed -i 's/enforcing/disabled/' /etc/selinux/config #永久，重启生效
```

## 1.6 关闭防火墙
```shell
iptables -F 
systemctl stop firewalld
systemctl disable firewalld
```

## 1.7 内存解锁和文件限制
```shell
#临时修改，重启失效
ulimit -n 65535
#永久修改，重启生效
cat << EOF >>/etc/security/limits.conf
elasticsearch soft memlock unlimited   
elasticsearch hard memlock unlimited   
* soft nofile 65535        
* hard nofile 65535
EOF
```

## 1.8 关闭 swap 缓存
```shell
swapoff -a #临时
sed -i '/swap/s/^/#/' /etc/fstab #永久 
```

## 1.9 设置 es 用户内存限制大小
```shell
sysctl -w vm.max_map_count=262144
echo 'vm.max_map_count=262144' >> /etc/sysctl.conf
```

## 1.10 不能 root 用户启动 

## 1.11 java版本

> es 7.*之后的版本不需要安装环境 java，默认自带 jdk 环境，但是其他的服务还是需要 jdk 环境的，所以还是要统一安装，另外 jdk 版本要为 8 或者 11，我这里默认 8 就好了，直接 yum 安装

```shell
yum -y install java-1.8.0-openjdk java-1.8.0-openjdk-devel
```

# 2. 安装 es 集群

## 2.1 官网下载
```
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.6.2-x86_64.rpm
wget https://artifacts.elastic.co/downloads/kibana/kibana-7.6.2-x86_64.rpm
wget https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.2-x86_64.rpm
wget https://artifacts.elastic.co/downloads/logstash/logstash-7.6.2.rpm
wget https://artifacts.elastic.co/downloads/beats/auditbeat/auditbeat-7.6.2-x86_64.rpm
```

## 2.2 yum 直接安装则可

```shell
yum -y install elasticsearch
yum -y install kibana
yum -y install filebeat
yum -y install auditbeat
```

## 2.3 es 配置修改，3 台机都要，其中 node.name 配置不同，其他一样
```
cd /etc/elasticsearch/ && cp elasticsearch.yml elasticsearch.yml.orig

cat << EOF > elasticsearch.yml
cluster.name: es-cluster
node.name: node01
path.data: /var/lib/elasticsearch
path.logs: /var/log/elasticsearch
bootstrap.memory_lock: true
network.host: 0.0.0.0
http.port: 9200
discovery.seed_hosts: ["192.168.110.245", "192.168.110.246", "192.168.110.247"]
cluster.initial_master_nodes: ["node01"]
xpack.security.enabled: true
xpack.security.transport.ssl.enabled: true
xpack.security.transport.ssl.verification_mode: certificate
xpack.security.transport.ssl.keystore.path: elastic-certificates.p12
xpack.security.transport.ssl.truststore.path: elastic-certificates.p12
xpack.security.audit.enabled: true
xpack.security.audit.logfile.events.emit_request_body: true
EOF

#node02: sed -i 's#node\.name\:\ node01#node.name: node02#' /etc/elasticsearch/elasticsearch.yml       
#node03: sed -i 's#node\.name\:\ node01#node.name: node03#' /etc/elasticsearch/elasticsearch.yml
```
```
解释说明：
cluster.name 集群名字，同一个集群中使用相同名字，单机就随意
node.name: es-cluster 节点名字，每台机都不一样
bootstrap.memory_lock: true 将进程地址所在 RAM 空间中，以防止 es 内存被换出，交换对性能，节点稳定性非常不利，应不惜一切代价避免交换。它可能导致垃圾收集持续数分钟而不是毫秒，并且可能导致节点响应缓慢甚至断开与群集的连接。在弹性分布式系统中，让操作系统杀死该节点更为有效。
    1、如果你的机器已经全局进制swap，那么不开启这个配置也无所谓。详情查看 1.8 配置
    2、或者配置了/etc/sysctl.conf 的 vm.swappiness = 1 ，所以也不需要配置 true
    3、如果以上两个你都没做，那你还是乖乖把 true 配置上。我这里都做了，也不影响。
network.host: 0.0.0.0 需要外网 ip 可以这么配置，不然配置内网也行
http.port: 9200 端口号，不配置的话默认9200
discovery.seed_hosts: ["192.168.110.245", "192.168.110.246", "192.168.110.247"] 这里可以不指定，不过避免 es 回环查找，直接指定方便
cluster.initial_master_nodes: ["node-1"] 首次启动 es 集群指定 master 机器，之后重启和添加新服务器都无需更改
xpack.security.enabled: true  启用 xpack 安全模块
xpack.security.transport.ssl.enabled: true  TLS 功能，可对通信进行加密
```

## 2.4 如果是 yum 安装的方式需要修改es的启动文件

{{< link "https://www.elastic.co/guide/en/elasticsearch/reference/current/setting-system-settings.html#systemd" "参考地址" >}}

```shell
cat /usr/lib/systemd/system/elasticsearch.service
[Service]
LimitMEMLOCK=infinity

修改命令：
sed -i '/\[Service\]/a\LimitMEMLOCK=infinity' /usr/lib/systemd/system/elasticsearch.service
修改完之后要reload 一下
sudo systemctl daemon-reload
```


## 2.5 如果你要设置 es 的内存大小，有两个地方
```
1、jvm.options
cat /etc/elasticsearch/jvm.options 
-Xms512m
-Xmx512m

2、这里不做介绍，以上一种为常用方法，如果你设置的 java PATH 有问题，可以在这里面指定 java 环境。
/etc/sysconfig/elasticsearch 
```

## 2.6  在 node01 配置证书，我这里设置的空密码，直接回车便可。
```
/usr/share/elasticsearch/bin/elasticsearch-certutil ca
/usr/share/elasticsearch/bin/elasticsearch-certutil cert --ca elastic-stack-ca.p12 
cp /usr/share/elasticsearch/{elastic-stack-ca.p12,elastic-certificates.p12} /etc/elasticsearch/
chmod 640 /etc/elasticsearch/elastic-certificates.p12 

#scp /etc/elasticsearch/elastic-certificates.p12 root@node02:/etc/elasticsearch/
#scp /etc/elasticsearch/elastic-certificates.p12 root@node03:/etc/elasticsearch/
```
```
生成CA证书 bin/elasticsearch-certutil ca，将产生新文件 elastic-stack-ca.p12。该 elasticsearch-certutil 命令还会提示你输入密码以保护文件和密钥，请保留该文件的副本并记住其密码。
为集群中的每个节点生成证书和私钥 bin/elasticsearch-certutil cert --ca elastic-stack-ca.p12，将产生新文件 elastic-certificates.p12。系统还会提示你输入密码，你可以输入证书和密钥的密码，也可以按Enter键将密码留空。默认情况下 elasticsearch-certutil 生成没有主机名信息的证书，这意味着你可以将证书用于集群中的每个节点，另外要关闭主机名验证。

注意：如果你在创建证书时输入了密码，那可以通过下面的方法设置。
/usr/share/elasticsearch/bin/elasticsearch-keystore add xpack.security.transport.ssl.keystore.secure_password

/usr/share/elasticsearch/bin/elasticsearch-keystore add xpack.security.transport.ssl.truststore.secure_password
```

## 2.7 node01启动 es
```
systemctl start elasticsearch
systemctl enable elasticsearch
systemctl status elasticsearch
```

## 2.8 node01，配置账号密码完成tls通信

命令：
```
/usr/share/elasticsearch/bin/elasticsearch-setup-passwords interactive
```
结果：
```
[root@node01 elasticsearch]# /usr/share/elasticsearch/bin/elasticsearch-setup-passwords interactive
Initiating the setup of passwords for reserved users elastic,apm_system,kibana,logstash_system,beats_system,remote_monitoring_user.
You will be prompted to enter passwords as the process progresses.
Please confirm that you would like to continue [y/N]y


Enter password for [elastic]: 
Reenter password for [elastic]: 
Passwords do not match.
Try again.
Enter password for [elastic]: 
Reenter password for [elastic]: 
Enter password for [apm_system]: 
Reenter password for [apm_system]: 
Enter password for [kibana]: 
Reenter password for [kibana]: 
Enter password for [logstash_system]: 
Reenter password for [logstash_system]: 
Enter password for [beats_system]: 
Reenter password for [beats_system]: 
Enter password for [remote_monitoring_user]: 
Reenter password for [remote_monitoring_user]: 
Changed password for user [apm_system]
Changed password for user [kibana]
Changed password for user [logstash_system]
Changed password for user [beats_system]
Changed password for user [remote_monitoring_user]
Changed password for user [elastic]
```


检查：检查 node01是否启动并且密码访问正常
```
[root@node01 elasticsearch]# curl http://192.168.110.245:9200/_cat/nodes?v -u elastic:123456
ip              heap.percent ram.percent cpu load_1m load_5m load_15m node.role master name
192.168.110.245           12          90   3    0.07    0.08     0.08 dilm      *      node01
```

> 注意的问题：

> 1. 证书权限没设置，创建好默认为 600，手动修改为 640
> 
```
启动报错相关：
org.elasticsearch.ElasticsearchSecurityException: failed to load SSL configuration [xpack.security.transport.ssl]
Caused by: org.elasticsearch.ElasticsearchException: failed to initialize SSL TrustManager - not permitted to read truststore file [/etc/elasticsearch/elastic-certificates.p12]
Caused by: java.nio.file.AccessDeniedException: /etc/elasticsearch/elastic-certificates.p12
```
> 2. 权限查看及修改

```
[root@node01 elasticsearch]# ll /etc/elasticsearch/elastic-certificates.p12
-rw------- 1  640 elasticsearch  3451 Apr 16 18:57 elastic-certificates.p12
[root@node01 elasticsearch]# chmod 640 /etc/elasticsearch/elastic-certificates.p12 
```


## 2.9 将 node01 的证书 scp 到 02 和 03，并启动 02 和 03的 es

> 这里有三个证书
>> elasticsearch.keystore：这里存放的是 elastic和 123456 的账号密码，给 es 集群使用，默认读取
>> elastic-stack-ca.p12：这个是 ca 初始证书，保存好就行，生成完 cert 之后用处不大
>> elastic-certificates.p12：这个是 cert 验证证书，es 集群就是通过这个证书完成

```
scp /etc/elasticsearch/{elasticsearch.keystore,elastic-stack-ca.p12,elastic-certificates.p12} root@node02:/etc/elasticsearch/
scp /etc/elasticsearch/{elasticsearch.keystore,elastic-stack-ca.p12,elastic-certificates.p12} root@node03:/etc/elasticsearch/

systemctl start elasticsearch
systemctl enable elasticsearch
systemctl status elasticsearch
```

## 2.10 基于 api 的检查，集群是否搭建成功。

> #查看集群健康状态
```
curl http://192.168.110.245:9200/_cat/health?v -u elastic:123456
```
> #查看集群节点
```
curl http://192.168.110.245:9200/_cat/nodes?v -u elastic:123456
```

> 可以发现 es 集群 ok 了！我们可以发现 node01 的 master 字段值为*，没错，它就是 master 节点了。
```
[root@node01 elasticsearch]# curl http://192.168.110.245:9200/_cat/health?v -u elastic:123456
epoch      timestamp cluster    status node.total node.data shards pri relo init unassign pending_tasks max_task_wait_time active_shards_percent
1591345666 08:27:46  es-cluster green           3         3      2   1    0    0        0             0                  -                100.0%
[root@node01 elasticsearch]# curl http://192.168.110.245:9200/_cat/nodes?v -u elastic:123456
ip              heap.percent ram.percent cpu load_1m load_5m load_15m node.role master name
192.168.110.245           20          91   0    0.00    0.01     0.05 dilm      *      node01
192.168.110.246           21          78  63    0.42    0.14     0.08 dilm      -      node02
192.168.110.247           29          76  13    0.89    0.29     0.13 dilm      -      node03
```

# 3. 安装 es 的 web UI
## 3.0 有哪些 web ui
| | | |
| ---- | --- | --- |
| ElasticHD | 我喜欢用这个，因为临时使用可以直接./ElasticHD 启动，方便快捷。 有 sql 转 json 的小工具，很好用 | https://github.com/360EntSecGroup-Skylar/ElasticHD/ |
| elasticsearch-HQ | 要安装 python3.6 有 es 默认的接口查询，还有 node 节点的监控数据 | https://github.com/ElasticHQ/elasticsearch-HQ/ |
| dejavu | 支持从 json 和 csv 导入导出数据 | https://github.com/appbaseio/dejavu/ |
| cerebro | | https://github.com/lmenezes/cerebro |
| elasticsearch-head | | https://github.com/mobz/elasticsearch-head |
| elasticsearch-browser | 不支持 5.0 之后 | https://github.com/OlegKunitsyn/elasticsearch-browser |
| elasticsearch-kopf | 支持 2.0之前 | https://github.com/lmenezes/elasticsearch-kopf |

## 3.1 如果带账号密码访问的话 url 
```
http://elastic:123456@192.168.110.245:9200
```

## 3.2 安装web UI管理工具ElasticHD:

> 普通安装：

（1）安装supervisord
```
yum -y install supervisor
echo_supervisord_conf > supervisord.conf
systemctl start supervisord
systemctl enable supervisord
```
（2）安装 elasticHD
```
yum -y install xdg-utils
cd /usr/local/src/
wget https://github.com/360EntSecGroup-Skylar/ElasticHD/releases/download/1.4/elasticHD_linux_amd64.zip
unzip elasticHD_linux_amd64.zip 
chmod 0777 ElasticHD
mv ElasticHD /usr/local/bin/
cat <<EOF> /etc/supervisord.d/ElasticHD.ini
[program:ElasticHD]
command=/usr/local/bin/ElasticHD -p 0.0.0.0:9800
autostart=true
autorestart=true
user=elasticsearch
redirect_stderr=true
stdout_logfile=/tmp/ElasticHD.log
EOF

supervisorctl update
supervisorctl reload
supervisorctl status
```

> docker 安装：

```
docker run -p 9800:9800 -d --link elasticsearch:demo containerize/elastichd
```

web端访问结果：http://192.168.110.245:9800/

{{< image src="https://cdn.jsdelivr.net/gh/ZhaoUncle/zhaouncle.github.io/images/elk_01.png" >}}

## 3.3 安装 elasticsearch -HQ


> 普通安装：

(1)安装python3.6
```
yum install python3 python3-pip python3-devel -y
```
(2) 安装 elasticsearch-HQ
```
cd /usr/local/src/
wget -O elasticsearch-HQ.zip  https://github.com/ElasticHQ/elasticsearch-HQ/archive/master.zip
unzip elasticsearch-HQ.zip 
mv elasticsearch-HQ-master /usr/local/elasticsearch-HQ
cd /usr/local/elasticsearch-HQ
pip3 install -r requirements.txt
cat <<EOF> /etc/supervisord.d/ElasticHQ.ini
[program:ElasticHQ]
command=/usr/bin/python3 /usr/local/elasticsearch-HQ/application.py --host 0.0.0.0 --port 5000
autostart=true
autorestart=true
#user=elasticsearch
redirect_stderr=true
stdout_logfile=/tmp/ElasticHQ.log
EOF

supervisorctl update
supervisorctl reload
supervisorctl status
```

> docker 安装
```
docker run -p 5000:5000 elastichq/elasticsearch-hq
```
web端访问结果：http://192.168.110.245:5000/

{{< image src="https://cdn.jsdelivr.net/gh/ZhaoUncle/zhaouncle.github.io/images/elk_02.png" >}}

# 4. 安装 Kibana
## 4.1 引用：

> Kibana是一个针对Elasticsearch的开源分析及可视化平台，用来搜索、查看交互存储在Elasticsearch索引中的数据。使用Kibana，可以通过各种图表进行高级数据分析及展示。

> Kibana让海量数据更容易理解。它操作简单，基于浏览器的用户界面可以快速创建仪表板（dashboard）实时显示Elasticsearch查询动态。

## 4.2 安装：
```
yum -y install kibana-7.6.2-x86_64.rpm 

#创建日志路径
mkdir /var/log/kibana
touch /var/log/kibana/kibana.log
chmod -R 755 /var/log/kibana/
chown -R kibana /var/log/kibana/
```


## 4.3 修改配置文件
```
[root@node01 elk]# cat /etc/kibana/kibana.yml |grep -Ev '^#|^$'
#kibana 端口号
server.port: 5601
#kibana 内网 ip
server.host: "192.168.110.245"
#es 的地址
elasticsearch.hosts: ["http://192.168.110.245:9200"]
#es 的账号密码，在配置 tls 通信的时候那里就设置了的
elasticsearch.username: "kibana"  
elasticsearch.password: "123456"
#记录发送到 es 的查询
elasticsearch.logQueries: true
# 日志路径
logging.dest: /var/log/kibana/kibana.log
# 禁止显示除错误消息以外的所有日志记录输出。
logging.quiet: true
# 记录所有日志事件，包括系统使用情况信息和所有请求
logging.verbose: false
# 开启 xpack
xpack.security.enabled: true
```

## 4.4 启动
```
systemctl restart kibana
systemctl status kibana
```

## 4.5 open kibana url

http://192.168.110.245:5601/ ，这里要用 elastic 账号登录，使用 kibana 登录是无法，会报错 403 没权限



{{< image src="https://cdn.jsdelivr.net/gh/ZhaoUncle/zhaouncle.github.io/images/elk_03.png" >}}

## 4.6 内存限制
参考资料：<https://www.elastic.co/guide/en/kibana/7.2/production.html#load-balancing-es>

默认内存限制 1.4GB，如果过需要可以通过环境变量进行替换，一般不需要更改。

# 5. node02 安装 logstash
## 5.1 安装 logstash
```
yum -y install logstash-7.6.2.rpm
cp /etc/logstash/logstash.yml /etc/logstash/logstash.yml.orig
```
## 5.2 配置文件
```
[root@node02 elk]# cat /etc/logstash/logstash.yml|grep -Ev '#|^$'
path.data: /var/lib/logstash
# 配置自动加载配置，这样就无须在更改配置时手动重启 logstash 引起数据丢失
config.reload.automatic: true
# 自动加载的时间
config.reload.interval: 10s
path.logs: /var/log/logstash
```
## 5.3 修改内存地址
```
vim /etc/logstash/jvm.options
-Xms1g
-Xmx1g
```
## 5.4 手动测试，输入 hello
```
cd /usr/share/logstash
./bin/logstash -e 'input { stdin { } } output { stdout { } }'
hello
```

{{< image src="https://cdn.jsdelivr.net/gh/ZhaoUncle/zhaouncle.github.io/images/elk_04.png" >}}


# 6. node02、03安装 filebeat

## 6.1 安装
```
yum -y install filebeat-7.6.2-x86_64.rpm 
cd /etc/filebeat
cp filebeat.yml filebeat.yml.orig
```


# 7. 日志处理

## 案例一：官方例子
## 参考：
https://www.elastic.co/guide/en/logstash/7.7/plugins-inputs-beats.html
https://www.elastic.co/guide/en/logstash/current/advanced-pipeline.html

## 7.1.1在 node02 调整 logstash 配置
```
cd /etc/logstash/conf.d/
cat <<EOF> /etc/logstash/conf.d/example.conf 
input {
  beats {
    port => 5044
  }
}

output {
  elasticsearch {
    hosts => ["http://192.168.110.245:9200"]
    index => "%{[@metadata][beat]}-%{[@metadata][version]}" 
    user => "elastic"
    password => "123456"
  }
}
EOF
systemctl restart logstash
```

## 7.1.2检查端口是否启动：
```
[root@node02 elk]# netstat -nltp|grep 5044
tcp6       0      0 :::5044                 :::*                    LISTEN      31422/java   
```

## 7.1.3 在 node02 配置 filebeat
```
cd /root/src/elk
wget https://download.elastic.co/demos/logstash/gettingstarted/logstash-tutorial.log.gz
gzip -d logstash-tutorial.log.gz 
cat <<EOF> filebeat.yml
filebeat.inputs:
- type: log
  paths:
    - /root/src/elk/logstash-tutorial.log 
output.logstash:
  hosts: ["192.168.110.246:5044"]
EOF
```

## 7.1.4 启动 filebeat
```
/usr/share/filebeat/bin/filebeat -e -c filebeat.yml  -d "publish"
```

启动结果部分截图：

{{< image src="https://cdn.jsdelivr.net/gh/ZhaoUncle/zhaouncle.github.io/images/elk_05.png" >}}

## 7.1.5 在 kibana 打开“Management”->>“Elasticsearch”->>“IndexManagement”，可以看到 filebeat 的索引

{{< image src="https://cdn.jsdelivr.net/gh/ZhaoUncle/zhaouncle.github.io/images/elk_06.png" >}}

然后我们到 kiabana 那里添加 index 试试。

{{< image src="https://cdn.jsdelivr.net/gh/ZhaoUncle/zhaouncle.github.io/images/elk10.png" >}}
{{< image src="https://cdn.jsdelivr.net/gh/ZhaoUncle/zhaouncle.github.io/images/elk07.png" >}}


创建成功

{{< image src="https://cdn.jsdelivr.net/gh/ZhaoUncle/zhaouncle.github.io/images/elk08.png" >}}

最后我们到 Discover 这里就可以看到相关试图了

{{< image src="https://cdn.jsdelivr.net/gh/ZhaoUncle/zhaouncle.github.io/images/elk09.png" >}}
