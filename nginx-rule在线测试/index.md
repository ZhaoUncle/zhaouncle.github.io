# 【Nginx】 正则在线测试


<!--more-->

##起因

nginx 测试基本都是本地起一个 nginx 服务，然后修改 conf 配置文件进行测试，那么，有没有更方便快捷的方法呢？

虽然网上有很多正则表达式的网站，但是因为 nginx 和这些网站的[正则表达式引擎略](https://en.wikipedia.org/wiki/Comparison_of_regular-expression_engines#Libraries)有不同，所以很多在线测试的结果是不准确的。

nginx使用的是 [PCRE](http://www.pcre.org/)，[官方文档介绍](http://nginx.org/en/docs/configure.html)

##方法

nginx 官方提供了一个基于 php 编写的正则测试器 [nginx-regex-tester](https://github.com/nginxinc/NGINX-Demos/tree/master/nginx-regex-tester)，可以用 docker-compose 直接启动



测试成功截图；Result: Match found

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20210105161347338.png" width="600" hegiht="250" align=center/>



测试失败截图：Result：Match not found

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20210105161418649.png" width="600" hegiht="250" align=center/>

###注意问题：

如果你在 `docker-compose up -d` 很慢，主要是因为 Dockerfile 的 nginx 镜像使用的国外源

1. 确认 nginx 是什么系统，在 dockerfile 加入这两句去判断

```
RUN cat /etc/issue
RUN cat /etc/os-release
```

2. 我刚确认用的是 `debian buster` 系统，所以直接更换为清华源就好了，

```
RUN echo "deb https://mirrors.tuna.tsinghua.edu.cn/debian/ buster main contrib non-free \n\
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ buster-updates main contrib non-free \n\
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ buster-backports main contrib non-free \n\
deb https://mirrors.tuna.tsinghua.edu.cn/debian-security buster/updates main contrib non-free" > /etc/apt/sources.list
```

如图所示；

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20210105163028746.png" width="800" hegiht="250" align=center/>



https://www.nginx.com/blog/regular-expression-tester-nginx/


