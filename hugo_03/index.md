# Hugo（三）自定义域名&cloudfare github pages 加速https


<!--more-->

# 一、GitHub Pages + CloudFlare是建立网站的最简单方法之一。

github pages，我这里选择了 hugo+LoveIt 的静态页面 blog 架构，有兴趣自己查看之前的文章。

## 1. 配置 github pages 域名

为了使用自己的域名，而不是 username.github.io，我这里使用了 github pages 的设置，进到“Settings”里面，往下拉可以看到，一个是设置自己的顶级域名，不要 zhaouncle.com，一个是开启 https 的选项。开启他可以用 https://zhaouncle.github.io进行访问，另外 https 做 seo 会优先进行，所以开启是有好处的，好像 2018 年才开始有这个功能。以前只能通过反代或者 cloudfare 实现 https。

![img](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201029153651726.png)

![image-20201029153714603](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201029153714603.png)

## 2.在 username.github.io 仓库下面创建一个 CNAME 的文件并写入自己的域名

![image-20201029155629786](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201029155629786.png)

#  二、利用Cloudflare为基于GitHub Pages的Hugo博客添加HTTPS支持

1、cloudfare 注册之后会给你两个 dns 地址，我这里是在阿里云上面购买的域名，于是去注册一个，我这里那 zhaowp.com举个例子

![image-20201029153728901](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201029153728901.png)

![image-20201029153747126](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201029153747126.png)

这一步先跳过，等会在进行编辑

![image-20201029153811185](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201029153811185.png)

这一步要去我们的域名采购商那里进行 dns 的更改

![image-20201029153825639](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201029153825639.png)

这一步默认就行，无需变动

![image-20201029153841882](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201029153841882.png)

我们可以看到在 Overview 这里，没有进行 dns 服务器更改这里会一直提示，我这里是阿里云的域名购买商，于是去进行更改 dns 服务器地址的操作，因为我之前已经填好了上面的步骤，所以用 zhaowp.com举个例子，别觉得奇怪

周到对应的域名，点击管理

![image-20201029154438966](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201029154438966.png)

![image-20201029154503255](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201029154503255.png)

![image-20201029154100218](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201029154100218.png)

点击确定之后我们回到 cloudfare 进行操作，等了几分钟我们发现这里解析成功了，

![image-20201029154045353](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201029154045353.png)

我们在 dns 这里设置 4 个 dns A 记录和 1 个CNAME 记录

add recore的时候 A 记录这里可以用@表示，这样cloudfare 会自己解析成你要的域名了，也就是现在为啥A 记录是域名的原因了。

![img](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201029154543042.png)

![image-20201029154556897](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201029154556897.png)

然后到 SSL/TLS这里配置证书启用情况，我这里选了FULL，为何要选 FULL 呢，因为我在 github 上面开启了 https，所以需要进行双方的证书认证，否则无法打开网站

![image-20201029154721577](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201029154721577.png)

最后，我们创建一条规则，即最简单的永远使用 ssl 访问这个域名，点击 create 一条规则

![image-20201029154739334](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201029154739334.png)

![image-20201029154753244](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201029154753244.png)

![image-20201029154809732](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201029154809732.png)

我刚举了个 zhaowp 的例子，现在要删了，因为我没这个域名

![image-20201029154824563](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201029154824563.png)
