# openwrt单臂盘路由端口转发


<!--more-->



1\. 使用场景
--------

openwrt 做单臂旁路由，主路由 DHCP 把 gateway 和 dns 都指向了旁路由的 ip。

2\. 方法
------

2.1 首先要在主路由把旁路由的端口映射出去，我这边直接在主路由做了DMZ转发，把旁路由的所有端口都转发出去，然后由旁路由控制外部的端口，当然你也可以在主路由选定端口范围进行转发，具体看自己需求。

![op1](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/op1.png)

2.2 旁路由 op 要开启 nat 转发。

![op2](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/op2.png)

2.3 添加端口转发配置

![op3](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/op3.png)

2.4 因为是单臂旁路由，只有一个网口，所以一般 op 都会设置为 lan 口进行处理 ip 数据。故此👇🏻配置如下

*   源区域选择 wan(lan)；

*   内部区域也选择 lan；

*   外部端口自定义，最好超过 10000，80 和 443 端口就千万别用了，经验之谈，当然听不听就是你自己的事情了；

*   内部 ip 地址和端口就是你要对外访问的内部服务；

*   启用NAT回环；



2.5 然后，你就可以尽情的用外部端口进行访问了，记得要加密，保护自己的隐私安全哟(＾Ｕ＾)ノ~ＹＯ！！！



![](https://res.smzdm.com/pc/pc_shequ/dist/img/the-end.png)

