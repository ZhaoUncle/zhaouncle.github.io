# 【iptables】 简单使用


<!--more-->

# iptables 管理

iptables 端口映射配置步骤：

1. 保存现有配置，执行如下命令：iptables-save > iptables.config
2. 编辑iptables.config 映射对应端口
3. 更新配置，执行如下命令：iptables-restore < iptables.config

# ip 白名单

```
# sample configuration for iptables service
# you can edit this manually or use system-config-firewall
# please do not ask us to add additional ports/services to this default configuration
*filter
:INPUT ACCEPT [0:0]
:FORWARD ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]


##设置 ip 白名单
-N whitelist
##添加IP到白名单
-A whitelist -s xx.xx.xx.xx/32 -j ACCEPT　　　　

##不配置 witelist 列表，允许内网 ip 访问某端口
-A INPUT -s 192.168.105.0/24 -p tcp -m tcp --dport 22 -j ACCEPT
-A INPUT -s 10.25.10.0/24 -p tcp -m tcp --dport 22 -j ACCEPT

##禁止访问 27017
-A INPUT -p tcp -m tcp --dport 27017 -j DROP

-A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
-A INPUT -p icmp -j ACCEPT
-A INPUT -i lo -j ACCEPT

##whitelist调用白名单IP列表，22端口只允许白名单IP使用
-A INPUT -p tcp -m state --state NEW -m tcp --dport 22 -j whitelist　

##whitelist调用白名单IP列表，3306端口只允许白名单IP使用
-A INPUT -p tcp -m state --state NEW -m tcp --dport 3306 -j whitelist　　

##无调用白名单，外网可访问
-A INPUT -p tcp -m state --state NEW -m tcp --dport 80 -j ACCEPT　　　　　　
-A INPUT -p tcp -m state --state NEW -m tcp --dport 443 -j ACCEPT

-A INPUT -j REJECT --reject-with icmp-host-prohibited
-A FORWARD -j REJECT --reject-with icmp-host-prohibited
COMMIT
```



### 1. 禁止访问外部某个端口

`iptables -A OUTPUT -p tcp --dport 6382 -j DROP`

### 2. 禁止访问某个 ip 的某个端口

`iptables -A OUTPUT -p tcp -d 192.168.1.10 --dport 6382 -j DROP`


