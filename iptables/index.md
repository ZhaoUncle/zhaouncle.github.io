# 【iptables】 简单使用


<!--more-->

### 1. 禁止访问外部某个端口

`iptables -A OUTPUT -p tcp --dport 6382 -j DROP`

### 2. 禁止访问某个 ip 的某个端口

`iptables -A OUTPUT -p tcp -d 192.168.1.10 --dport 6382 -j DROP`


