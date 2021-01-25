# 【ELK】elastalert 日志告警


<!--more-->

# 一、环境

```
系统：centos7
elk 版本：7.6.2
```



## 1.1 ElastAlert 工作原理

周期性的查询Elastsearch并且将数据传递给规则类型，规则类型定义了需要查询哪些数据。

当一个规则匹配触发，就会给到一个或者多个的告警，这些告警具体会根据规则的配置来选择告警途径，就是告警行为，比如邮件、钉钉、tg、slack、企业微信等

[ElastAlert 手册](https://elastalert.readthedocs.io/en/latest/elastalert.html#)



# 二、安装配置 elastalert

**Tips：Elastalert 0.2.0 之后使用 Python 3.6，不再使用 Python 2 版本**

## 2.1 安装 python3.6

```
#安装EPEL和IUS软件源
yum install epel-release -y
yum install https://centos7.iuscommunity.org/ius-release.rpm -y
#安装python3.6
yum install python36u python36u-devel python36u-pip -y
#ln -s /usr/bin/python3.6 /bin/python3
#ln -s /usr/bin/pip3.6 /bin/pip3
```

##2.2 安装 elastalert

```
pip3 install elastalert
```

###2.2.1 配置

```
cd /opt/soft/
git clone https://github.com/Yelp/elastalert.git 
cd elastalert
cp config.yaml.example config.yaml
mkdir rules
```

###2.2.2 样例

https://github.com/Yelp/elastalert/tree/master/example_rules

### 2.2.3 config.yaml 配置文件

```
#规则目录
rules_folder: /opt/soft/elastalert/rules
#多久从ES中查询一次
run_every:
  seconds: 30
#是查询窗口的大小，从每个查询运行的时间向后延伸。对于其中use_count_query或use_terms_query设置为true的规则，此值将被忽略。
buffer_time:
  minutes: 15
#连接elasticsearch配置
es_host: 127.0.0.1
es_port: 9200
es_username: elastic
es_password: xxxxxxxx
#elastalert索引名称
writeback_index: elastalert_status
writeback_alias: elastalert_alerts
#失败重试限制
alert_time_limit:
  days: 2
  
```

**以下配置没使用，只做介绍  **

```
#### 使用 TLS 连接诶 elastsearch
#use_ssl: True
#验证 tls 证书
#verify_certs: True
#带正文的GET请求是Elasticsearch的默认选项。如果因为某些原因失败了，你可以通过'GET'，'POST'或'source',具体可以查看以下(http://elasticsearch-py.readthedocs.io/en/master/connection.html?highlight=send_get_body_as#transport)
#es_send_get_body_as: GET

# 开启 ssl 认证证书
#verify_certs: True
#ca_certs: /path/to/cacert.pem
#client_cert: /path/to/client_cert.pem
#client_key: /path/to/client_key.key

```

###2.2.4 在 elasticsearch 中创建 elastalert 的日志索引

**Tips :  **如果索引已存在，则不会重新创建

```
elastalert-create-index --index elastalert
```

输出结果：

```
ProcessController:  Elastic Version: 7.6.2
Reading Elastic 6 index mappings:
Reading index mapping 'es_mappings/6/silence.json'
Reading index mapping 'es_mappings/6/elastalert_status.json'
Reading index mapping 'es_mappings/6/elastalert.json'
Reading index mapping 'es_mappings/6/past_elastalert.json'
Reading index mapping 'es_mappings/6/elastalert_error.json'
```



##2.3 常用命令

```
# 测试规则文件
elastalert-test-rule rule.yaml

# 启动监控报警
python3 -m elastalert.elastalert --verbose --rule /root/elastalert/example_rules/rule.yaml

```



## 2.4 服务启动方法

**Tips: 启动指定家目录的原因主要是有调用，可以少写路径，比如不需要指定 config.yml，rule 里面的 yml 文件可以少写路径指定==**

### 2.4.1 service 启动管理

```
# 创建elastalert服务文件
vim /etc/systemd/system/elastalert.service
```



```
[Unit]
Description=elastalert
After=elasticsearch.service

[Service]
Type=simple
User=root
Group=root
Restart=on-failure
WorkingDirectory=/opt/soft/elastalert
ExecStart=/usr/bin/python3 -m elastalert.elastalert --verbose --config /opt/soft/elastalert/config.yaml 

[Install]
WantedBy=multi-user.target
```

### 2.4.2 supervisor 启动管理

安装supervisor

```
yum install -y supervisor
systemctl enable supervisord
```

编写elastalert supervisord守护进程

`vim /etc/supervisord.d/elastalert.ini`

```
[program:elastalert]
directory=/opt/soft/elastalert
command=/usr/bin/python3 -m elastalert.elastalert --verbose
autostart=true
startsecs=5
autorestart=true
startretries=10
redirect_stderr=true
stdout_logfile_maxbytes=20MB
stdout_logfile_backups=5
stdout_logfile=/var/log/elastalert/elastalert.log
```

启动

```
systemctl enable supervisord
systemctl restart supervisord
systemctl start supervisord
systemctl stop supervisord
supervisorctl status
```

# 三、告警测试

## 3.1 es 测试索引

###3.1.1 创建并推送数据到 index

`curl -X POST "http://elastic:passwd@127.0.0.1:9200/test-alert/test" -H 'Content-Type: application/json' -d '{"@timestamp": "'$(date --iso-8601=seconds)'", "field": "value"}'`

输出：

`{"_index":"test-alert","_type":"test","_id":"inH1I3cBMJwNYi416aLN","_version":1,"result":"created","_shards":{"total":2,"successful":1,"failed":0},"_seq_no":0,"_primary_term":1}`

###3.1.2 删除 index

`curl -X DELETE http://elastic:passwd@127.0.0.1:9200/test-alert`

输出：

`{"acknowledged":true}`

### 3.1.3 查看有哪些索引

`curl 'http://elastic:passwd@127.0.0.1:9200/_cat/indices?v'`

###3.1.4查看索引内容：

`curl -X GET http://elastic:passwd@127.0.0.1:9200/test-alert/_search`

输出结果：

```
{"took":2,"timed_out":false,"shards":{"total":1,"successful":1,"skipped":0,"failed":0},"hits":{"total":{"value":4,"relation":"eq"},"max_score":1.0,"hits":[{"index":"test-alert","type":"test","id":"inH1I3cBMJwNYi416aLN","score":1.0,"source":{"@timestamp": "2021-01-21T08:01:52+00:00", "field": "value"}},{"index":"test-alert","type":"test","id":"0ocXJHcBMJwNYi41y3rG","score":1.0,"source":{"@timestamp": "2021-01-21T08:37:55+0000", "field": "value"}},{"index":"test-alert","type":"test","id":"aZAiJHcBMJwNYi41QTqW","score":1.0,"source":{"@timestamp": "2021-01-21T08:49:20+0000", "field": "value"}},{"index":"test-alert","type":"test","id":"1ZMmJHcBMJwNYi41h4z8","score":1.0,"source":{"@timestamp": "2021-01-21T08:54:00+0000", "field": "value"}}]}}
```



<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20210121170230728.png" alt="image-20210121170230728" style="zoom:50%;" />

## 3.2 邮件告警

### 3.2.1 创建测试告警规则

`vim /opt/soft/elastalert/rules/emailtt.yml` 

```
name: test-alert
type: any
# es 的监控索引
index: test-ale*

# 发现 1 次就告警
num_events: 1
#1 分钟检查1次，和上面一起配合就是 1 分钟内触发 1 次规则就告警
timeframe:
  minutes: 1
  
#告警规则，查询索引内 field 字段的值 value，可以用正则进行匹配
filter:
- query:
    query_string:
      query: "field: *value*"
      
## smtp 邮件 server 配置
smtp_host: smtp.zoho.com
smtp_port: 465
smtp_ssl: true
from_addr: "test@qq.com"
### 发送邮件的账号密码
smtp_auth_file: /opt/soft/elastalert/elastalert/smtp_auth_file.yml

#告警方式
alert:
- "email"

#设置只需要的告警字段
include: ["_index","uri","remote_addr","http_x_forwarded_for","status"]

#邮件标题
alert_subject: "test-alert 正式环境 告警 {}"
#告警邮件接收人
email:
- "test@qq.com"
- "test1@qq.comk"
```

### 3.2.2 发送邮件的账号密码

`vi /opt/soft/elastalert/elastalert/smtp_auth_file.yml`

```
# 发件箱的qq邮箱地址，也就是用户名
user: test@qq.com
# 不是qq邮箱的登录密码，是授权码
password: passwd
```

### 3.2.3 启动告警规则并发送测试数据

```
cd /opt/soft/elastalert
python3 -m elastalert.elastalert --verbose --rule /opt/soft/elastalert/rules/emailtt.yml
```



##3.3 钉钉告警

###3.3.1 下载插件

```
cd /opt/soft/elastalert
git clone https://github.com/xuyaoqiang/elastalert-dingtalk-plugin
cp elastalert-dingtalk-plugin/elastalert_modules/dingtalk_alert.py elastalert/
```

### 3.3.2 安装依赖

```
#查看依赖文件elastalert-dingtalk-plugin/requirements.txt，在对比 pip list 之后，我发现只需要安装以下内容即可

pip3 install pyOpenSSL==16.2.0
pip3 install requests==2.18.1
pip3 install setuptools>=11.3
```

###3.3.3 创建测试告警规则

`vim /opt/soft/elastalert/rules/dingtt.yml` 

```
name: Example frequency rule
type: any
index: test-alert
num_events: 1
timeframe:
  minutes: 1
filter:
- query:
    query_string:
      query: "field: value"


#告警方式
alert:
- "elastalert.dingtalk_alert.DingTalkAlerter"

#钉钉接口
dingtalk_webhook: "https://oapi.dingtalk.com/robot/send?access_token=钉钉机器人 api token"
dingtalk_msgtype: "text"
```

### 3.3.4 启动告警规则并发送测试数据

```
cd /opt/soft/elastalert
python3 -m elastalert.elastalert --verbose --rule /opt/soft/elastalert/rules/dingtt.yml
```



### 3.3.5 钉钉机器人配置

具体可以查看[钉钉机器人开发文档](https://ding-doc.dingtalk.com/doc#/serverapi2/qf2nxq)

1. 最简单可以使用加 ip 方式

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20210121170019750.png" alt="image-20210121170019750" style="zoom:33%;" />

2. 加关键字，就是发送到钉钉的信息里面有某个关键字

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20210121165909770.png" alt="image-20210121165909770" style="zoom: 33%;" />

###3.3.6 测试结果告警内容如下所示

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20210121165950134.png" alt="image-20210121165950134" style="zoom:33%;" />

## 3.4 微信告警

###3.4.1 参考

https://github.com/anjia0532/elastalert-wechat-plugin

https://anjia0532.github.io/2017/02/16/elastalert-wechat-plugin/

https://github.com/anjia0532/elastalert-docker

## 3.5 告警格式优化

```
# 注意上下字段对应
alert_text: |
    kibana_url: "https://hostname:5601/app/kibana"
    alarm_reason: "1分钟内login.php至少被访问10次"
    alarm_name: {}
    request_uri: {}
    request_ip: {}
    response_status: {}
alert_text_args:
    - name
    - request
    - clientip
    - response
alert_text_type: alert_text_only
```



# 四、kibana 添加 elastalert 插件



##4.1 elastalert-kibana-plugin插件

kibana中elastalert插件，可以实现在kibana界面上编辑elastalert的告警规则配置。

kibana 的 elastalert 插件需要使用新的 [bitsensor/elastalert](https://github.com/bitsensor/elastalert) api 服务

### 4.1.1 演示效果

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/showcase.gif" alt="Showcase" style="zoom: 50%;" />

## 4.2 安装 elastalert-kibana-plugin

官网主推：[bitsensor/elastalert-kibana-plugin](https://github.com/bitsensor/elastalert-kibana-plugin)

但是楼上那个很多小版本都没有，以上没有的话推荐使用以下版本：

[nsano-rururu/elastalert-kibana-plugin](https://github.com/nsano-rururu/elastalert-kibana-plugin)

目前版本支持：

- [Kibana 6.8.1～6.8.12、7.5.1～7.8.1](https://github.com/nsano-rururu/elastalert-kibana-plugin/releases/tag/1.2.0)
- [Kibana 7.9.0～7.9.3](https://github.com/nsano-rururu/elastalert-kibana-plugin/releases/tag/1.3.0)



### 4.2.1 安装命令

`/usr/share/kibana/bin/kibana-plugin install https://github.com/nsano-rururu/elastalert-kibana-plugin/releases/download/1.2.0/elastalert-kibana-plugin-1.2.0-7.6.2.zip --allow-root`

### 4.2.2 配置

> 默认情况下，插件将连接到 localhost:3030，也就是 kibana 和 elastalert 在同一台服务器上面，那么就不需要添加以下配置。如果您的ElastAlert服务器在其他主机或端口上运行，请在config / kibana.yml文件中添加更改以下选项：

```
elastalert-kibana-plugin.serverHost: 192.168.1.1
elastalert-kibana-plugin.serverPort: 9000
```



##4.3 安装 elastalert_docker

查看 nsano 提供的 [elastalert api 服务支持列表](https://github.com/nsano-rururu/elastalert-kibana-plugin/wiki/ElastAlert-Server-Docker-Images)

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20210122151817830.png" alt="image-20210122151817830" style="zoom: 50%;" />

我使用的是 [praecoapp/elastalert-server](https://hub.docker.com/r/praecoapp/elastalert-server) 提供的 docker 镜像对应 [elastalert 0.2.4 版本](https://github.com/Yelp/elastalert/releases/tag/v0.2.4)

[docker-compose sample 示例参考](https://github.com/nsano-rururu/elastalert-kibana-plugin/wiki/docker-compose-sample)

###4.3.1 安装命令

```
cd /opt/soft/
git clone https://github.com/bitsensor/elastalert.git elastalert_docker
cd elastalert_docker
mkdir rules 
```

### 4.3.2 更改配置，api server 端用的配置是config.json

`vi config/config.json`

因为我的 kibana 和 elastalert docker 是同一台机，所以我主要修改 es 的配置就可以

```
{
  "appName": "elastalert-server",
  "port": 3030,
  "wsport": 3333,
  "elastalertPath": "/opt/elastalert",
  "verbose": false,
  "es_debug": false,
  "debug": false,
  "rulesPath": {
    "relative": true,
    "path": "/rules"
  },
  "templatesPath": {
    "relative": true,
    "path": "/rule_templates"
  },
  "es_host": "192.168.3.30",
  "es_port": 9200,
  "writeback_index": "elastalert_status"
}
```

### 4.3.3 更改 elastalert 配置

`vi ./config/elastalert.yaml `

```
es_host: 192.168.3.30
es_port: 9200
rules_folder: rules

run_every:
  seconds: 5

buffer_time:
  minutes: 1

es_username: elastic
es_password: passwd

writeback_index: elastalert_status

alert_time_limit:
  days: 2
```



### 4.3.4 docker-compose.yml 配置文件

`vi docker-compose.yml`

```
version: '3'
services:
  elastalert:
    image: praecoapp/elastalert-server:20210104
    container_name: elastalert
    hostname: elastalert
    restart: always
    network_mode: "host"
    ports:
      - "3030:3030"
      - "3333:3333"
    volumes:
      - ./config/elastalert.yaml:/opt/elastalert/config.yaml
      - ./config/elastalert-test.yaml:/opt/elastalert/config-test.yaml
      - ./config/config.json:/opt/elastalert-server/config/config.json
      - ./rules:/opt/elastalert/rules
      - ./rule_templates:/opt/elastalert/rule_templates
      #- ./elastalert:/opt/elastalert/elastalert，#这一句先注释掉，下面有个操作
```

### 4.3.5 启动

```
docker-compose up -d
##然后把 docker 镜像里面的 elastalert 复制出来
docker cp elastalert:/opt/elastalert/elastalert .
##再把docker-compose.yml 里面的 volumes 的注释项去掉重新启动
docker-compose up -d
```

###4.3.6 配套钉钉插件和规则测试

```
cd /opt/soft/elastalert_docker
git clone https://github.com/xuyaoqiang/elastalert-dingtalk-plugin
cp elastalert-dingtalk-plugin/elastalert_modules/dingtalk_alert.py elastalert/
##安装依赖
docker exec -it elastalert pip install pyOpenSSL==16.2.0
docker exec -it elastalert pip install requests==2.18.1
docker exec -it elastalert pip install setuptools>=11.3
#创建规则
touch rules/dingtt.yml
#授权，否则页面无法编辑，具体可以看下面的报错示例
chown centos.centos rules/dingtt.yml
#重新启动 elastalert 容器
docker restart elastalert
##查看日志
docker logs -f --tail=20 elastalert
```



## 4.4 出现的报错：





<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20210122095020201.png" alt="image-20210122095020201" style="zoom:50%;" />



```
01:50:07.285Z ERROR elastalert-server:
    Routes:  Request for '/rules/:id' failed with error: 
    
     [Error: EACCES: permission denied, open '/opt/elastalert/rules/dingtt.yaml'] {
      errno: -13,
      code: 'EACCES',
      syscall: 'open',
      path: '/opt/elastalert/rules/dingtt.yaml'
    }
```

解决办法：

1. 直接 chmod 777 -R /opt/soft/elastalert_docker/rules/
2. 直接 chown centos.centos /opt/soft/elastalert_docker/rules/*



![image-20210122095445225](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20210122095445225.png)





# 五、告警规则详解

[ElastAlert 规则介绍](https://elastalert.readthedocs.io/en/latest/ruletypes.html)

##5.1 告警类型

```
ElastAlert包含几种具有常见监视范例的规则类型：
any: 匹配任何与给定过滤器匹配的事件,这个是查到了什么便直接报警，属于自定义选项；
frequency: 匹配Y时间内至少有X个事件的地方
spike: 当事件发生率增加或减少时匹配，API 流量陡然上升并马上恢复的时候；
flatline: 在Y时间内少于X个事件时进行匹配，内存或者CPU使用率下降的时候；
blacklist并whitelist输入: 当某个字段与黑名单/白名单匹配时匹配，昨天的那个疑似爬虫的 IP 地址又出现了；
change: 当某个字段在一段时间内具有两个不同的值时进行匹配，应用的状态突然从 UP 转为 DOWN；
new_term: 当字段中出现从未见过的术语时进行匹配，某个枚举类型字段，突然出现了未定义的类型；
cardinality:当字段的唯一值数量大于或小于阈值时匹配，线上的 API 服务器突然挂了一台，它是根据唯一值的数量来判定的；
```

##5.2 报警抑制：减少重复报警

```
# 用来区分报警，跟 realert 配合使用，在这里意味着，
# 5 分钟内如果有重复报警，那么当 name 不同时，会当做不同的报警处理，可以是数组
query_key:
  - name

# 5 分钟内相同的报警不会重复发送
realert:
  minutes: 5

# 指数级扩大 realert 时间，中间如果有报警，
# 则按照 5 -> 10 -> 20 -> 40 -> 60 不断增大报警时间到制定的最大时间，
# 如果之后报警减少，则会慢慢恢复原始 realert 时间
exponential_realert:
  hours: 1
```

##5.3 报警聚合：相同报警，聚合为一条

```
# 根据报警的内，将相同的报警安装 name 来聚合
aggregation_key: name

# 聚合报警的内容，只展示 name 与 message
summary_table_fields:
  - name
  - message
```

##5.4 报警格式化：突出重要信息

在这里，你可以自定义 alert 的内容，它的内部使用 Python 的 format 来实现的。

```
alert_subject: "Error {} @{}"
alert_subject_args:
  - name
  - "@timestamp"

alert_text_type: alert_text_only
alert_text: |
  ### Error frequency exceeds
  > Name: {}
  > Message: {}
  > Host: {} ({})
alert_text_args:
  - name
  - message
  - hostname
  - host
```

#六、告警接收方式

```
Email
Slack
企业微信
钉钉
alerta：https://github.com/alerta/alerta，告警接收平台
JIRA
OpsGenie
Commands
HipChat
MS Teams
Telegram
AWS SNS
VictorOps
PagerDuty
Exotel
Twilio
Gitter
```



# 七、告警需求整理

整理下实施ELK最起码要实现的需求：

- 查询条件（精确匹配）：一级域名、二级域名、客户真实IP、HTTP状态码、HTTP方法、request_time、response_time、代理IP、body_bytes_sent
- 查询条件（模糊匹配）：url（如查找SQL注入关键字）、refere（流量来源）、agent（如查找搜索引擎）
- 近期（1周、1个月）内整体请求量走势情况；
- 如果发现总体走势异常，要很方便找到那个域名走势异常；
- 过去一个周期内（1天、1周、1月）所有请求构成，按不同域名出饼图；
- 实时监控爬虫IP过高的频率访问（如单个IP1分钟请求超过100次报警）；
- 实时监控500状态请求情况（如单个域名1分钟出现30个500就报警）；





# 参考

https://github.com/Yelp/elastalert

https://github.com/bitsensor/elastalert

https://github.com/bitsensor/elastalert-kibana-plugin

https://github.com/xuyaoqiang/elastalert-dingtalk-plugin

https://github.com/anjia0532/elastalert-wechat-plugin

https://segmentfault.com/a/1190000017553282

https://blog.csdn.net/wenwenxiong/article/details/106048313

https://www.cnblogs.com/liuxinyustu/articles/14228934.html




