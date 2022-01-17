# freenom免费域名自动续期和ddns

<!--more-->


# 一、DNS 简约

有需要可以看一下参考文章：

- [阮一峰 的 DNS 原理入门](https://www.ruanyifeng.com/blog/2016/06/dns.html)
- [CloudFlare DNS 基础知识](https://www.cloudflare.com/zh-cn/learning/dns/dns-records/dns-ns-record/)
- [Google DNS 基础知识](https://support.google.com/a/answer/48090?hl=zh-Hans)
- [DNS功能介绍](https://www.hillstonenet.com/support/4.0/cn/SA/net_dns_intro.html)


## 1. 什么是 DNS

答：

1. DNS（Domain Name System） 是域名系统，简单理解为根据域名找出ip地址，可以在网上通过域名来转化为你的ipv4或者ipv6地址，找到你的访问地址，而不是记住冗长的ip地址，当然，有些域名耶不好记忆，这个很正常，不抬杠。
2. 常见的资源记录有：SOA（起始授权结构）、A（主机）、NS（名称服务器）、CNAME（别名）和MX（邮件交换器），其中A记录用于映射ipv4或ipv6地址，CNAME记录映射为CDN地址或者其他域名（不能用ip，不过AWS的好像可以用ip表达，其他dns厂商没见过），这两种使用方式是最为常见的。

## 2. 什么是DNS NS记录和域名服务器

答：

1. NS 代表域名服务器，可以简单理解为你要把你的域名放在那里做解析，如果我要把freenom购买的域名放到阿里云做DNS解析，NS就要修改为阿里云的dns ns 域名服务器，否则无法在阿里云上面做 A 解析记录，映射到你的公网ip上；
2. 域名服务器一般是2个以上进行负载均衡，避免一个挂了，导致网站解析异常；
3. 每个dns解析厂商，都有自己的NS服务器，不要混用；
4. 有些dns服务商不支持解析为内网地址；

## 3. 何时应更新或更改 NS 记录？

答：

1. 当你的ip或者cname改变时，就需要修改dns的A记录或者CNMAE记录；
2. NS更新记录一般需要等待几分钟，最长24小时，才能在全网进行更新；
3. 如果更新没成功，建议你先刷新dns、关闭网络在重连（比如拔掉网线，不会弄又急的话就重启电脑咯）、浏览器用无痕模式等去验证解析是否成功；
4. 可以使用**站长工具**在线测试dns解析结果：https://tool.chinaz.com/dns/?type=1&host=&ip=

   <img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220105-180018@2x.png" alt="WX20220105-180018@2x" style="zoom:25%;" />

## 4. 什么是 DDNS ？

答：

DDNS（Dynamic Domain Name Serve） 是指动态域名服务，比如，我们家用宽带的ip是动态的，随着每一次的路由重启，断线重连，或者移动商的定期重连，都会导致你的家用公网ip变化（当然你也可能没有，也可能只有ipv6），甚至是服务器没购买绑定公网ip，停机重启后导致公网ip变化，我们需要把动态ip映射到一个固定的域名解析服务器上（阿里云、dnspod）等，然后由服务器提供DNS服务实现动态域名解析。也就是说DDNS捕获的是用户每次变化的IP，然后与其域名相对应，我们需要记住的只是域名就好，而无需管他后面的ip是啥。

参考：[DDNS是什么？DDNS的工作原理是怎样的?](https://segmentfault.com/a/1190000005772787)

# 二、Freenom 域名

⚠️⚠️⚠️freenom的免费域名千万千万🙅🏻‍♀️不要🙅🏻‍♀️用于生产环境，否则等你流量起来之后，freenom会被莫名其妙的把你网站给封了，然后卖给别人，就等着哭吧。不过只是用于家庭环境使用，也无所谓了，不要就换一个的心态，那就无所谓。要不然还不如去买一下xyz这种便宜的顶级域名，一年6块钱的，就一瓶大可乐的零花钱就够了。

## 1. 申请 freenom 免费域名

1.0 [**Freenom是世界上第一个也是唯一的免费域名提供商。**](http://www.freenom.com/zh/aboutfreenom.html)

1.1 由于该网站没有国内解析，所以比较卡访问慢，没条件的同学可以上阿里云或者腾讯云买一台香港最低配置的临时 ubuntu desktop 或者 windows desktop 云主机，按量付费，基本上15 分钟内都能搞定，花不了两块钱然后可以开始进行注册了。

1.2 点击 👉🏻 [注册地址](https://my.freenom.com/domains.php) 👈🏻 即可跳转登录

1.3 免费域名只支持**.tk**、**.ml**、**.ga**、**.cf**、**.gq**，另外，最多支持 12 个月的免费续订，到期要后续期，否则会被回收。

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220102-231329@2x.png" alt="WX20220102-231329@2x" style="zoom: 33%;" />

1.4 开始进行注册 “**Register a New Domain**”，输入自己想要的域名，然后点击 **Check Availability**

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220102-231638@2x.png" alt="WX20220102-231638@2x" style="zoom: 25%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220102-184531@2x.png" alt="WX20220102-184531@2x" style="zoom: 33%;" />

1.5 如果显示以下信息“**Yes,domain is available!**"，则表示该域名可以注册，然后选择”Select“，点击 **checkout** 进行跳转即可

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220102-184717@2x.png" alt="WX20220102-184717@2x" style="zoom: 25%;" />

1.6 需要手动选择 **Period** 时间为12个月，默认是 **3 Months**

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220102-184753@2x.png" style="zoom:25%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220102-184811@2x.png" alt="WX20220102-184811@2x" style="zoom:25%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220102-184824@2x.png" alt="WX20220102-184824@2x" style="zoom:25%;" />

1.7 最左边”**Enter Your Email Address**“的位置可以直接填写邮箱，然后点击”**Verify My Email Address**“，会发送一封验证邮件到你的邮箱，然后去邮箱打开 freenom 发过来的验证邮件，点击里面的邮件地址进行跳转即可，当然你也可以在最右边登录第三方 gmail 账号进行注册，原理都一样

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220102-190808@2x.png" alt="WX20220102-190808@2x" style="zoom:25%;" />

1.8 我用的是 gmail 第三方登录的方式，点击👆🏻最右边的“**登录**”，如下

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220102-190858@2x.png" alt="WX20220102-190858@2x" style="zoom:25%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220102-191001@2x.png" alt=" " style="zoom:25%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220102-191035@2x.png" alt="WX20220102-191035@2x" style="zoom:25%;" />

1.9 然后填写注意的信息，一个是 **address1** 指的ip，可以不填，默认会获取ip自动写入，一个是** city** 要和ip对应的地区一致，这两个要分别对应，我选取ipinfo.io的地址里面获取到的**city：”Hong Kong“**，然后填进去

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220102-191056@2x.png" alt="WX20220102-191056@2x" style="zoom:25%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220102-191137@2x.png" alt="WX20220102-191137@2x" style="zoom:25%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220102-191213@2x.png" alt="WX20220102-191213@2x" style="zoom: 25%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220102-191312@2x.png" alt="WX20220102-191312@2x" style="zoom:25%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220102-191402@2x.png" alt="WX20220102-191402@2x" style="zoom:25%;" />

1.10 如果出现以下报错，就要考虑ip和city是否填写正确了，改完之后多重新尝试几次

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220102-233002@2x.png" alt="WX20220102-233002@2x" style="zoom: 33%;" />

1.11 免费域名的注册到这里就结束了，因为免费域名是最多时长为 12 个月，默认是 3 个月（要记得自己修改哦），到期前 15 天我们要自动续期，否则就会被取消域名所有权，会被 freenom 回收，另外续期是免费的，不会像奸商一样续期要加钱。

## 2. 如何更改 freenom 的 NameServers

注册成功后，我们去检查我们选择的域名，选择“**Manage Domain**”然后修改 NS 服务器地址，可以指向 aws 的 router53，阿里云的 ddns，腾讯云的 dnspod，cloudfare 免费 cdn dns 解析商==

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220104-173356.png" alt="WX20220104-173356" style="zoom: 50%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220104-173431.png" alt="WX20220104-173431" style="zoom: 50%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220105-145732.png" alt="WX20220105-145732" style="zoom: 50%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220105-145759.png" alt="WX20220105-145759" style="zoom: 33%;" />

## 3. 如何手动续订域名

选择“**Renew Domains**”，然后进入域名类别，点击最右侧的 “**Renew This Domain**”

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220104-173826.png" alt="WX20220104-173826" style="zoom: 50%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220104-173910.png" alt="WX20220104-173910" style="zoom: 50%;" />

## 4. 如果上面的city地址填错，可以手动更改

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220102-233401@2x.png" alt="WX20220102-233401@2x" style="zoom: 25%;" />

## 5. 如何修改freenom 密码

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220102-233050@2x.png" alt="WX20220102-233050@2x" style="zoom: 33%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220102-233240@2x.png" alt="WX20220102-233240@2x" style="zoom:25%;" />

## 6. 忘记密码如何重置？

📢：特别是使用gmail邮箱登录的，都不会有初始密码，需要使用重置功能，直接发送邮件到你的gmail📮进行重置方可。另外要注意的一个点就是，gmail只是第三方登录，freenom和gmail的密码是不通用的，两个不同的系统，互相改密码都不影响对方。

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220102-233639@2x.png" alt="WX20220102-233639@2x" style="zoom: 25%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220102-233539@2x.png" alt="WX20220102-233539@2x" style="zoom: 33%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220102-233555@2x.png" alt="WX20220102-233555@2x" style="zoom: 33%;" />

## 7. 如何删除不用的域名

> 你也可以等到过期自动回收。

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-152357@2x.png" alt="WX20220103-152357@2x" style="zoom:33%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-152419@2x.png" alt="WX20220103-152419@2x" style="zoom: 33%;" />

## 8. 一些官方 Questions

地址：http://www.freenom.com/zh/support.html

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220102-233930@2x.png" alt="WX20220102-233930@2x" style="zoom:50%;" />

# 三、unraid 如何自动续期 freenom？

1. 以下两个docker镜像，推荐使用 luolongfei/freenom ，只拿他做个例子，另一个作为备用。

- [luolongfei/freenom](https://registry.hub.docker.com/r/luolongfei/freenom)
- [rouroux/freenom-automatic-renewal](https://registry.hub.docker.com/r/rouroux/freenom-automatic-renewal)

2. unraid 配置freenom-ddns

```
Name: freenom-ddns
Overview: https://github.com/luolongfei/freenom
Repository: luolongfei/freenom
Docker Hub URL: https://hub.docker.com/r/luolongfei/freenom
Icon URL: https://my.freenom.com/templates/freenom/img/logo.png

PATH1: /mnt/user/appdata/freenom/conf  ###配置文件要导入本地,升级镜像才丢失数据
Container Path: /conf

PATH2: /mnt/user/appdata/freenom/logs  ### 日志，可以不用设置
Container Path: /app/logs

RUN_AT: 0 1 * * 6											 ### 何时执行更新，可以不用设置，我只是举个例子
Container Variable: RUN_AT

FREENOM_USERNAME: freenom登录邮箱			  ### freenom的登录邮箱
Container Variable: FREENOM_USERNAME

FREENOM_PASSWORD: freenom密码					 ### 如果是第三方登录账号，比如gmail，一定要去把密码改了，不是gmail的密码，双方账号密码不通用
Container Variable: FREENOM_PASSWORD

MAIL_ENABLE: 0												### 关闭邮件通知，如果没设置邮箱登录的开启会报错，特别烦
Container Variable: MAIL_ENABLE

```

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220106-111857@2x.png" alt="WX20220106-111857@2x" style="zoom: 25%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/iShot2022-01-06%2011.19.34.png" alt="iShot2022-01-06 11.19.34" style="zoom: 25%;" />
3. 配置文件

  📢：推荐使用 Filebrowser 这个 docker 容器去挂载unraid 的 /mnt/user，这样就可以在线直接修改配置文件了，而不需要去命令行里面修改，很方便

查看默认配置[**.env.example**](https://github.com/luolongfei/freenom/blob/main/.env.example)，除了一开始的freenom账号密码设置为变量外，你也可以去掉，然后在配置文件里面进行更改,
他的通知方式除了看日志，还支持邮件、Telegram Bot、企业微信、Server 酱 微信通知、Bark 送信 ios app端，可惜不支持钉钉。

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220106-142249@2x.png" alt="WX20220106-142249@2x" style="zoom:33%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220106-142359@2x.png" alt="WX20220106-142359@2x" style="zoom:33%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220106-143829@2x.png" alt="WX20220106-143829@2x" style="zoom: 50%;" />

# 四、ddns 服务列表推荐

👂🏻**推荐使用**：80x86/ddns-updater、newfuture/ddns、jeessy/ddns-go、sanjusss/aliyun-ddns

👂🏻**域名购买商**：[NameSilo](https://www.namesilo.com/) 推荐

| 功能                                                         | [qmcgaw/ddns-updater](https://registry.hub.docker.com/r/qmcgaw/ddns-updater) | [80x86/ddns-updater](https://hub.docker.com/r/80x86/ddns-updater) | [newfuture/ddns](https://registry.hub.docker.com/r/newfuture/ddns) | [jeessy/ddns-go](https://registry.hub.docker.com/r/jeessy/ddns-go) | [sanjusss/aliyun-ddns](https://registry.hub.docker.com/r/sanjusss/aliyun-ddns) | [maxisoft/freenom-dns-updater](https://registry.hub.docker.com/r/maxisoft/freenom-dns-updater) | [crazymax/ddns-route53](https://hub.docker.com/r/crazymax/ddns-route53/) |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 支持多域名                                                   | ✅                                                            | ✅                                                            | ✅                                                            | ✅                                                            | ✅                                                            | ✅                                                            | ✅                                                            |
| 支持不同dns解析平台同时登录                                  | ✅                                                            | ✅                                                            |                                                              |                                                              |                                                              |                                                              |                                                              |
| 更新方式                                                     | 只能修改，不能新增                                           | 可新增                                                       | 可新增                                                       | 可新增                                                       | 可新增                                                       |                                                              |                                                              |
| ipv4                                                         | ✅                                                            | ✅                                                            | ✅                                                            | ✅                                                            | ✅                                                            | ✅                                                            | ✅                                                            |
| ipv6                                                         | ✅                                                            | ✅                                                            | ✅                                                            | ✅                                                            | ✅                                                            | ✅                                                            | ✅                                                            |
| [dnspod](https://www.dnspod.cn/)                             | ✅                                                            | ✅                                                            | ✅                                                            | ✅                                                            |                                                              |                                                              |                                                              |
| [dnspod 国际](https://www.dnspod.com/)                       | ✅                                                            | ✅                                                            | ✅                                                            | ✅                                                            |                                                              |                                                              |                                                              |
| [阿里云 dns](https://www.aliyun.com/ntms/wanwang/domain/dns) | ✅                                                            | ✅                                                            | ✅                                                            | ✅                                                            | ✅                                                            |                                                              |                                                              |
| [cloudflare](https://www.cloudflare.com/)                    | ✅                                                            | ✅                                                            | ✅                                                            | ✅                                                            |                                                              |                                                              |                                                              |
| AWS router53                                                 |                                                              |                                                              |                                                              |                                                              |                                                              |                                                              | ✅                                                            |
| Freenom                                                      |                                                              |                                                              |                                                              |                                                              |                                                              | ✅                                                            |                                                              |
| [华为云](https://www.huaweicloud.com/)                       |                                                              |                                                              | ✅                                                            | ✅                                                            |                                                              |                                                              |                                                              |
| [dns.com](https://www.dns.com/)                              |                                                              |                                                              | ✅                                                            |                                                              |                                                              |                                                              |                                                              |
| [HE.net](https://dns.he.net/)                                | ✅                                                            |                                                              | ✅                                                            |                                                              |                                                              |                                                              |                                                              |
| [DD24](https://www.domaindiscount24.com/cn)                  | ✅                                                            |                                                              |                                                              |                                                              |                                                              |                                                              |                                                              |
| [DDNSS.de](https://ddnss.de/)                                | ✅                                                            |                                                              |                                                              |                                                              |                                                              |                                                              |                                                              |
| [DigitalOcean](https://cloud.digitalocean.com/)              | ✅                                                            |                                                              |                                                              |                                                              |                                                              |                                                              |                                                              |
| [DonDominio](https://www.dondominio.com/)                    | ✅                                                            |                                                              |                                                              |                                                              |                                                              |                                                              |                                                              |
| [DremHost](https://www.dreamhost.com/)                       | ✅                                                            | ✅                                                            |                                                              |                                                              |                                                              |                                                              |                                                              |
| [DuckDNS](https://www.duckdns.org/)                          | ✅                                                            | ✅                                                            |                                                              |                                                              |                                                              |                                                              |                                                              |
| [DynDNS](https://account.dyn.com/)                           | ✅                                                            |                                                              |                                                              |                                                              |                                                              |                                                              |                                                              |
| [FreeDNS](https://freedns.afraid.org/zc.php?from=L2RvbWFpbi8=) | ✅                                                            |                                                              |                                                              |                                                              |                                                              |                                                              |                                                              |
| [Gandi](https://www.gandi.net/zh-Hans)                       | ✅                                                            |                                                              |                                                              |                                                              |                                                              |                                                              |                                                              |
| [GoDaddy](https://www.godaddy.com/)                          | ✅                                                            | ✅                                                            |                                                              |                                                              |                                                              |                                                              |                                                              |
| [Google](https://developers.google.com/speed/public-dns)     | ✅                                                            |                                                              |                                                              |                                                              |                                                              |                                                              |                                                              |
| [Infomaniak](https://www.infomaniak.com/fr)                  | ✅                                                            |                                                              |                                                              |                                                              |                                                              |                                                              |                                                              |
| [Linode](https://www.linode.com/)                            | ✅                                                            |                                                              |                                                              |                                                              |                                                              |                                                              |                                                              |
| [LuaDNS](https://www.luadns.com/)                            | ✅                                                            |                                                              |                                                              |                                                              |                                                              |                                                              |                                                              |
| [Namecheap](https://www.namecheap.com/)                      | ✅                                                            | ✅                                                            |                                                              |                                                              |                                                              |                                                              |                                                              |
| [NoIP](https://www.noip.com/)                                | ✅                                                            | ✅                                                            |                                                              |                                                              |                                                              |                                                              |                                                              |
| [Njalla](https://njal.la/)                                   | ✅                                                            |                                                              |                                                              |                                                              |                                                              |                                                              |                                                              |
| [OpenDNS](https://www.opendns.com/)                          | ✅                                                            |                                                              |                                                              |                                                              |                                                              |                                                              |                                                              |
| [OVH](https://www.ovhcloud.com/en-gb/)                       | ✅                                                            |                                                              |                                                              |                                                              |                                                              |                                                              |                                                              |
| [Porkbun](https://porkbun.com/)                              | ✅                                                            |                                                              |                                                              |                                                              |                                                              |                                                              |                                                              |
| [Selfhost.de](https://www.selfhost.de/)                      | ✅                                                            |                                                              |                                                              |                                                              |                                                              |                                                              |                                                              |
| [Servercow.de](https://www.servercow.de/)                    | ✅                                                            |                                                              |                                                              |                                                              |                                                              |                                                              |                                                              |
| [Spdyn](https://spdyn.de/)                                   | ✅                                                            |                                                              |                                                              |                                                              |                                                              |                                                              |                                                              |
| [Strato.de](https://www.strato.de/)                          | ✅                                                            |                                                              |                                                              |                                                              |                                                              |                                                              |                                                              |
| [Variomedia.de](https://www.variomedia.de/)                  | ✅                                                            |                                                              |                                                              |                                                              |                                                              |                                                              |                                                              |

## 0. 公网ip获取渠道有哪些？

### 1. http 测试：
|  ipv4 | ipv6 | 
| :-: | :-: | 
| https://ipv4.lookup.test-ipv6.com/ip/| https://v6.ident.me/|
| https://myip.ipip.net/  |  https://test-ipv6.com/ |
| http://ip-api.com/json/?fields=query  | http://ds.test-ipv6.com/  |
| https://ip.3322.net  |  http://checkipv6.dyndns.com/ |
|  https://www.nsupdate.info/myip |  https://api-ipv6.ip.sb/ip |
| https://ident.me/  | http://v6.ident.me/  |
|  https://v4.ident.me/ | https://api6.ipify.org/  |
| https://icanhazip.com/  | https://ipv6.lookup.test-ipv6.com/ip/  |
|  http://whatismyip.akamai.com/ |   |
| https://myip.dnsomatic.com/  |   |
|  http://ip.cip.cc/ |   |
| http://members.3322.org/dyndns/getip  |   |
|  https://www.pubyun.com/dyndns/getip |   |
|  https://myip.dnsomatic.com/ |   |
| http://checkip.dyndns.com/  |   |
|   |   |

### 2. 命令行测试

```
### ipv4
host -4 myip.opendns.com resolver1.opendns.com
dig -4 +short myip.opendns.com @resolver1.opendns.com

### ipv6
host -6 myip.opendns.com resolver1.opendns.com
dig -6 +short myip.opendns.com @resolver1.opendns.com

```
## 1. unraid安装ddns-go用于动态域名解析

### 1.1 安装 ddns-go 步骤就不讲了，直接上图

```
Name: ddns-go
Overview: https://github.com/jeessy2/ddns-go
Repository: jeessy/ddns-go
Docker Hub URL: https://registry.hub.docker.com/r/jeessy/ddns-go
Icon URL: https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/DDNSIcon.png
WebUI: http://[IP]:[PORT:9876]
Port: 9876
Path: /mnt/user/appdata/ddns-go
```

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220105-165720@2x.png" alt="WX20220105-165720@2x" style="zoom: 25%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/iShot2022-01-05%2016.53.36.png" alt="iShot2022-01-05 16.53.36" style="zoom: 25%;" />

### 1.2 ddns-go配置简介

1. 因为ddns-go不支持多dns解析商的配置，所以你只能配置以下一个，比如我配置了cloudflare，就不能再配置阿里云了，不过家用环境下，其实一个也够用了，要不然你也可以选择开多个ddns-go的docker，配置不同的运营商

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220105-170451@2x.png" alt="WX20220105-170451@2x" style="zoom:25%;" />

2. 一般通过接口获取就足够使用了，不过要是有同学走了代理网络，这里就会有问题，要注意支持哟！另外支持配置多个不同的域名，以下举个例子，只支持自动解析到 A记录（主机）

   ```
   test.com
   ddns-go.test.com
   fucker.com
   dns.fucker.com
   ```

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220105-170514@2x.png" alt="WX20220105-170514@2x" style="zoom: 33%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220105-170528@2x.png" alt="WX20220105-170528@2x" style="zoom:33%;" />

3. 我这里直接禁止公网访问web，打了个勾，然后还设置了账号密码登录访问，已经成为使用习惯了，你自己的话就看着办吧

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220105-170538@2x.png" alt="WX20220105-170538@2x" style="zoom:33%;" />

4. Webhook，看文档说支持[Server酱](https://sc.ftqq.com/SCKEY.send)（强烈推荐），可以使用微信收到回调信息，不管更新成功还是不成功的结果。

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220105-170555@2x.png" alt="WX20220105-170555@2x" style="zoom:33%;" />

## 2. unraid 安装 qmcgaw/ddns-updater 用于动态域名解析

⚠️⚠️⚠️一开始不管是不是代理环境，获取 ip 都不准，因为使用的都是国外的获取工具，现在我都改成国内了，就可以使用了，真的挺不错，不过可以直接用荒野无灯大佬提供的带web编辑的也很香。

### 1. 安装 qmcgaw/ddns-updater

```
Name: ddns-updater-qmcgaw
Overview: https://github.com/qdm12/ddns-updater
Repository: qmcgaw/ddns-updater
Docker Hub URL: https://registry.hub.docker.com/r/qmcgaw/ddns-updater
Icon URL: https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/DDNSIcon.png
WebUI: http://[IP]:[PORT:8000]

Path: /mnt/user/appdata/ddns-updater-qmcgaw  ###保存配置文件，更新镜像也不影响数据
Container Path: /updater/data

Port: 28080
Container Port: 8000

PUBLICIP_FETCHERS: http   ### 改成http获取方式，不用dns
Container Variable: PUBLICIP_FETCHERS 

PUBLICIP_HTTP_PROVIDERS: https://myip.ipip.net,https://ip.3322.net  ### 改成国内的ipv4获取地址即可
Container Variable: PUBLICIP_HTTP_PROVIDERS

PUBLICIPV4_HTTP_PROVIDERS: https://myip.ipip.net,https://ip.3322.net  ### 改成国内的ipv4获取地址即可
Container Variable: PUBLICIPV4_HTTP_PROVIDERS

```
![WX20220107-111325@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220107-111325@2x.png)

![iShot2022-01-07 11.12.27](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/iShot2022-01-07%2011.12.27.png)




### 2. qmcgaw/ddns-updater 的配置文件

```
{
    "settings": [
        {
            "provider": "aliyun",   ### 阿里云 dns 解析
            "domain": "test.ml",    ### 解析的域名
            "host": "@,ddns-go,www,blog",   ### 二级域名前缀
            "access_key_id": "",    ###  阿里云的ck id
            "access_secret": "",    ### 阿里云的ck secret
            "ip_version": "ipv4"    ### 获取 ipv4 地址
        },
        {
            "provider": "cloudflare",  ### cloudflare dns 解析
            "domain": "test.com",   ### 解析的域名
            "host": "ddns-go,www,blog",   ### 二级域名前缀
            "ttl": 600, ### cf 上面的 ttl 值，单位秒
            "zone_identifier": " 每个域名点击进去的 zone id",
            "token": "",   ### cf 的 token
            "ip_version": "ipv4" ### 获取 ipv4 地址
        },
        {
            "provider": "dnspod",  ### dnspod dns 解析
            "domain": "test.com",   ### 解析的域名
            "host": "ddns-go,www,blog",   ### 二级域名前缀
            "token": "id,token",   ### 这里要用dnspod 的“ID,Token”，用逗号分隔开填写进去
            "ip_version": "ipv4" ### 获取 ipv4 地址
        },
        {
            "provider": "dnspod",  ### dnspod dns 解析
            "domain": "test.ml",   ### 解析的域名
            "host": "ddns-go,www,blog",   ### 二级域名前缀
            "token": "id,token",   ### 这里要用dnspod 的“ID,Token”，用逗号分隔开填写进去
            "ip_version": "ipv4" ### 获取 ipv4 地址
        },
    ]
}
```

### 3. 如何知道这些配置怎么来的呢？
答：
   打开对应的md文件，github 地址有提供配置案例：https://github.com/qdm12/ddns-updater/tree/master/docs

### 4. 注意要点：

1. 不能添加，只能更新，就是必须要dns解析厂商那里先手动配置一次A记录，才能使用，否则会有以下报错

![WX20220106-171511@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220106-171511@2x.png)

2. dnspod 要把"id,token"一起填写进去到token配置里面。

3. 如果遇到更新不及时，需要手动吧update.json删掉。

![WX20220107-105824@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220107-105824@2x.png)
## 3. unraid 安装 newfuture/ddns 用于动态域名解析

### 1. 安装 newfuture/ddns 
1.1 先配置 config.json，等会需要挂载到 docker 镜像，我这里用 Filebrowser 管理

```
{
  "$schema": "https://ddns.newfuture.cc/schema/v2.8.json",
  "debug": false,
  "dns": "dnspod",
  "id": "YOUR ID or EMAIL for DNS Provider",
  "index4": "default",
  "index6": "default",
  "ipv4": [
    "newfuture.cc",
    "ddns.newfuture.cc"
  ],
  "ipv6": [
    "newfuture.cc",
    "ipv6.ddns.newfuture.cc"
  ],
  "proxy": null,
  "token": "YOUR TOKEN or KEY for DNS Provider",
  "ttl": null
}
```
![WX20220106-230941@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220106-230941@2x.png)

1.2 unraid 配置

```
Name: ddns-newfuture
Overview: https://github.com/NewFuture/DDNS
Repository: newfuture/ddns
Docker Hub URL: https://registry.hub.docker.com/r/newfuture/ddns
Icon URL: https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/DDNSIcon.png

Path: /mnt/user/appdata/ddns-newfuture/config.json  # 我先手动创建了config.json，要不然会变成挂载目录，导致出问题
Container Path: /config.json
```

![iShot2022-01-06 23.11.18](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/iShot2022-01-06%2023.11.18.png)

1.3 根据你自身的网络环境，修改相应配置，ipv4 我直接用的公网解析 “**public**”，因为我是 unraid，不是在路由器上面，如果是路由器比如 openwrt 直接安装 docker 可以指定网卡。

![WX20220106-2339012@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220106-2339012@2x.png)

官网配置参考：https://github.com/NewFuture/DDNS

![WX20220106-233814@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220106-233814@2x.png)

## 4. unraid 安装 sanjusss/aliyun-ddns 用于动态域名解析

### 1. 安装 sanjusss/aliyun-ddns

```
Name: ddns-aliyun
Overview: https://github.com/sanjusss/aliyun-ddns
Repository: sanjusss/aliyun-ddns
Docker Hub URL: https://registry.hub.docker.com/r/sanjusss/aliyun-ddns
Icon URL: https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/DDNSIcon.png

阿里云的 AccessKey ID: 你自己的阿里云AccessKey ID
Container Variable: AKID

阿里云的 AccessKey Secret: 你自己的 AccessKey Secret
Container Variable: AKSCT

域名: unraid.test.top,www.test.top,test.top,ddns-go.test2.com  ### 支持多域名
Container Variable: DOMAIN

更新间隔: 300   ### 5分钟自动更新一次ip上传
Container Variable: REDO

是否检查本地网卡IP: false   ### 如果是路由器，可以使用检查本地网卡ip
Container Variable: CHECKLOCAL

检查IPv4地址时，仅使用中国服务器: true    ### 国内，不管走不走代理，都用
Container Variable: CNIPV4

需要更改的记录类型: A    ### 只支持A或者AAAA记录     
Container Variable: TYPE

服务器缓存解析记录的时长: 600   
Container Variable: TTL

```
![WX20220107-093835@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220107-093835@2x.png)

![iShot2022-01-07 09.40.02](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/iShot2022-01-07%2009.40.02.png)


![WX20220107-094349@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220107-094349@2x.png)

变量推荐

![WX20220107-095236@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220107-095236@2x.png)

## 5. unraid 安装 80x86/ddns-updater 用于动态域名解析

### 1. 安装80x86/ddns-updater

```
Name: ddns-updater
Overview: 使用荒野无灯大神的ddns-updater镜像，具体使用教程可访问https://hub.docker.com/r/80x86/ddns-updater查看
Repository: 80x86/ddns-updater:amd64
Docker Hub URL: https://hub.docker.com/r/80x86/ddns-updater
Icon URL: https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/DDNSIcon.png
WebUI: http://[IP]:[PORT:8020]
Extra Parameters: --mount type=tmpfs,destination=/tmp

Host Path 1: /mnt/user/appdata/ddns-updater ###将数据挂载到unraid本地，避免更新镜像丢失数据
Container Path: /app/data

Key 1: 8020 ### 内部服务启动时的端口，要和上面的 WebUI 端口匹配
Container Variable: LISTENINGPORT

Key 2: admin  ### web 页面登录账号
Container Variable: HTTP_USERNAME

Key 3: admin  ### web 页面登录密码
Container Variable: HTTP_PASSWORD

Key 4: 114.114.114.114:53,8.8.8.8:53,208.67.222.222:443   ### 
Container Variable: GO_DNS_SERVERS

Key 5:
Container Variable: SERVERCHAN_KEY  ### server酱 微信接受通知
 
```


![iShot2022-01-07 15.24.34](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/iShot2022-01-07%2015.24.34.png)

![WX20220107-151649@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220107-151649@2x.png)

进入web页面进行编辑

![WX20220107-153536@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220107-153536@2x.png)

![WX20220107-114437@2x11](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220107-114437@2x11.png)

将配置进行整理复制进去就好了，这个配置一个settings只支持一个域名和一个host，多个就得复制多份进行配置

![WX20220107-154146@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220107-154146@2x.png)

配置举例：https://hub.docker.com/r/80x86/ddns-updater

```
# use [[settings]] to start a new domain config
# setup for example.com, DNS only
[[settings]]
provider = "cloudflare"
domain = "example.com"
host = "@"
proxied = false
ip_method = "dnspod"
delay = 300
token = "YOUR-CF-API-TOKEN-HERE"

# setup for foo.example.com, Proxied
[[settings]]
provider = "cloudflare"
domain = "example.com"
host = "foo"
proxied = true
# for proxied record, remember to set no_dns_lookup = true
no_dns_lookup = true
ip_method = "dnspod"
delay = 300
token = "YOUR-CF-API-TOKEN-HERE"

# setup for example.com
[[settings]]
provider = "alidns"
domain = "example.com"
host = "@"
ip_method = "dnspod"
delay = 300
key = "AccessKey ID"
secret = "AccessKey Secret"

# setup for foo.example.com
[[settings]]
provider = "alidns"
domain = "example.com"
host = "foo"
ip_method = "dnspod"
delay = 300
key = "AccessKey ID"
secret = "AccessKey Secret"

# use [[settings]] to start a new domain config
# setup for example.com
[[settings]]
provider = "dnspod"
domain = "example.com"
host = "@"
ip_method = "dnspod"
delay = 300
token = "id,token"

# setup for foo.example.com
[[settings]]
provider = "dnspod"
domain = "example.com"
host = "foo"
ip_method = "dnspod"
delay = 600
token = "id,token"
```

配置完拉倒最下面”Save Config“，然后”Back to Home“，查看结果，不需要重启，回自行加载，比 qmcgaw/ddns-updater 好用很多

![WX20220107-154013@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220107-154013@2x.png)

如果需要手动更新，也不需要重启，直接点击”Manual Update“

![WX20220107-114437@2xsdfa](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220107-114437@2xsdfa.png)
![WX20220107-155659@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220107-155659@2x.png)

### 2. 注意，cloudflare需要添加zone read权限

![WX20220107-153339](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220107-153339.png)
# 五、阿里云 dns 解析

## 1. 阿里云申请 AccessKey 用于api调用

1. 点击 👉🏻[阿里云 dns 登录地址](https://dns.console.aliyun.com/)👈🏻 即可跳转登录
2. 登录控制台之后，鼠标移动到右上角的人头像，不需要点击人头像，然后选择**AccessKey 管理**

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-212926@2x.png" alt="WX20220103-212926@2x" style="zoom:33%;" />

3. 创建 AccessKey 有两种方式，第一种是admin 权限的 Accesskey，如果泄露，那么会被用于此账号下面所有的 api 控制权限，非常不安全，推荐使用第二种**子用户 Accesskey** 模式

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-212959@2x.png" alt="WX20220103-212959@2x" style="zoom: 33%;" />

4. 第一种 admin 权限的 Accesskey

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-213246@2x.png" alt="WX20220103-213246@2x" style="zoom:33%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-213316@2x.png" alt="WX20220103-213316@2x" style="zoom:33%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-213340@2x.png" alt="WX20220103-213340@2x" style="zoom:25%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-213418@2x.png" alt="WX20220103-213418@2x" style="zoom: 50%;" />

5. 第二种**子用户 Accesskey** 模式

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-214547@2x.png" alt="WX20220103-214547@2x" style="zoom: 50%;" />

5.1 只需要选择OpenAPi 调用访问就好，控制台访问指的是这个账号可以用网页访问阿里云，没必要，用不到。

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-214706@2x.png" alt="WX20220103-214706@2x" style="zoom: 33%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-214923@2x.png" alt="WX20220103-214923@2x" style="zoom: 25%;" />

5.2 创建完成后，需要给用户进行授权AliyunDNSFullAccess的云解析dns权限

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-215245@2x.png" alt="WX20220103-215245@2x" style="zoom:25%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-215340@2x.png" alt="WX20220103-215340@2x" style="zoom:25%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-215412@2x.png" alt="WX20220103-215412@2x" style="zoom: 25%;" />

## 2. 在 云解析DNS 添加 freenom 注册的免费域名

1. 添加域名解析

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-153429@2x.png" alt="WX20220103-153429@2x" style="zoom: 33%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-153441@2x.png" alt="WX20220103-153441@2x" style="zoom: 50%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-153513@2x.png" alt="WX20220103-153513@2x" style="zoom: 25%;" />


1. 把 freenom 的NS域名服务器改为阿里云的

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-153600@2x.png" alt="WX20220103-153600@2x" style="zoom: 33%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-153244@2x.png" alt="WX20220103-153244@2x" style="zoom:33%;" />

3. 手动添加解析记录做测试

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-153701@2x.png" alt="WX20220103-153701@2x" style="zoom: 25%;" />

## 3.  使用 ddns-go 动态解析域名：

1. 把步骤1申请到的CK填入ddns-go 的DNS服务商；
2. 然后添加需要解析的域名到IPV4，选择右上角的保存就好了；
3. 然后在最左边查看解析日志就好了，对应的域名显示解析成功就表示正常了。

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/iShot2022-01-03%20212435.png" alt="WX20220103-152952@2x" style="zoom:33%;" />

# 七、腾讯云 dnspod 解析

## 1. 在腾讯云 dnspod 添加 freenom 注册的域名

1. 你可以点击 👉🏻[腾讯云 登录入口](https://console.cloud.tencent.com/cns)👈🏻 即可跳转登录，也可以点击 👉🏻[dnspod 登录入口](https://dnspod.cn)👈🏻 即可跳转登录，两个都一样可以用；
2. 添加域名

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-152952@2x.png" alt="WX20220103-152952@2x" style="zoom:33%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-153011@2x.png" alt="WX20220103-153011@2x" style="zoom:33%;" />

3. 查看dnspod的DNS服务器地址

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-153051@2x.png" alt="WX20220103-153051@2x" style="zoom:33%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-153111@2x.png" alt="WX20220103-153111@2x" style="zoom:33%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-153354@2x.png" alt="WX20220103-153354@2x" style="zoom: 33%;" />

4. 修改freenom的NS地址为腾讯云的dnspod，最迟24小时内生效。

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-153222@2x.png" alt="WX20220103-153222@2x" style="zoom: 25%;" />

5. 点击刷新后，解析状态显示“正常解析”就代表正常了

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-154350@2x.png" alt="WX20220103-154350@2x" style="zoom: 33%;" />

## 2. 申请api秘钥

📢**腾讯云 API 密钥** 不支持 dnspod 解析，请使用**DNSPod Token**

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220105-183856@2x.png" alt="WX20220105-183856@2x" style="zoom: 25%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220105-184107@2x.png" alt="WX20220105-184107@2x" style="zoom:25%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220105-184128@2x.png" alt="WX20220105-184128@2x" style="zoom: 33%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220105-184147@2x.png" alt="WX20220105-184147@2x" style="zoom:33%;" />

## 3. 使用 ddns-go 动态解析域名

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220106-083148@2x.png" alt="WX20220106-083148@2x" style="zoom: 33%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220106-083204@2x.png" alt="WX20220106-083204@2x" style="zoom:33%;" />

不管解析成功还是失败，这里都会显示日志信息。

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220106-083254@2x.png" alt="WX20220106-083254@2x" style="zoom: 25%;" />

# 八、cloudflare 动态 ddns 解析

📢：自 2020 年 4 月起，CloudFlare 不在支持 Freenom 的免费域名（**.tk**、**.ml**、**.ga**、**.cf**、**.gq**）调用 api token 的权限，只支持付费域名，所以现在想要用api自动更新ip的，可以弃用了，不过如果是dashboard 的话，还可以支持手动更新和免费证书。

Tips：我测试了.com的付费域名和.tk的免费域名，api调用只支持.com，但是dashboard 面板还可以继续使用.tk免费，没有api功能。

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220105-162451.png" alt="WX20220105-162451" style="zoom: 50%;" />

## 1. cloudflare 添加 freenom 注册的域名

1.1 点击 👉🏻[CloudFlare登录地址](https://www.cloudflare.com/)👈🏻 即可跳转登录

1.2 添加域名

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-153743@2x.png" alt="WX20220103-153743@2x" style="zoom: 33%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-153820@2x.png" alt="WX20220103-153820@2x" style="zoom:33%;" />

1.3 选择免费的功能就可以了

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-153911@2x.png" alt="WX20220103-153911@2x" style="zoom: 25%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-153938@2x.png" alt="WX20220103-153938@2x" style="zoom:25%;" />

1.4 默认会自动扫描该域名的已有的解析地址，等就是了

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-154012@2x.png" alt="WX20220103-154012@2x" style="zoom:33%;" />

<img src="/Users/aomine/Movies/obs/ddns/WX20220103-154150@2x.png?lastModify=1641373044" alt="WX20220103-154150@2x" style="zoom:25%;" />

1.5 重点来了，这里会显示你需要修改的freenom的NS地址，比如我之前是修改为阿里云，这次就修改成cloudflare的就好

```
buck.ns.cloudflare.com 
treasure.ns.cloudflare.com
```

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-154213@2x-20220105165923329.png" alt="WX20220103-154213@2x" style="zoom:25%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-154121@2x.png" alt="WX20220103-154121@2x" style="zoom:25%;" />

1.6 更改 Freenom 的 NS 地址到 cloudflare，具体可查看2.2步骤

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-154309@2x.png" alt="WX20220103-154309@2x" style="zoom:25%;" />

1.7 默认配置建议都开启，不过因为个人用，可能不会有HTTPS，这个可以关闭；

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-154412@2x-20220105165923520.png" alt="img" style="zoom:25%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220103-154504@2x.png" alt="WX20220103-154504@2x" style="zoom:25%;" />

## 2. CloudFlare申请CK用于api调用

点击 👉🏻[CloudFlare api token 获取地址]()👈🏻 即可跳转登录

鼠标点击右上角获取，进入“**我的个人资料**”，然后选择“**API 令牌**”，在创建“**令牌**”即可

<img src="/Users/aomine/Movies/obs/ddns/WX20220105-154549.png" alt="WX20220105-154549" style="zoom: 50%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220105-154834-20220105165923617.png" alt="img" style="zoom: 50%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220105-155030.png" alt="WX20220105-155030" style="zoom: 33%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220105-155440.png" alt="WX20220105-155440" style="zoom: 50%;" />

### 默认选择上面的dns模板就好了，主要是区域资源这里有三个选项，这里做一下解释

- 所有区域：意味着是该账号下面的域名都可以，即便你不是域名所有者，因为cloudflare是有邀请用户进行管理域名的权限控制功能；
- 账户的所有区域：表示该账号的域名所有者，被别人邀请管理的域名不归管控；
- 特定区域：就是指定域名进行管理了；

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220106-091306.png" alt="WX20220106-091306" style="zoom:33%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220106-091047.png" alt="WX20220106-091047" style="zoom:33%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220105-155948.png" alt="WX20220105-155948" style="zoom:50%;" />

### 注意：有些程序会使用 zone id 作为配置使用

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220106-214711@2x.png" alt="WX20220106-214711@2x" style="zoom: 50%;" />


## 3. ddns-go 解析 cloudflare

📢免费域名确实无法api解析，报错如下图，com的域名解析正常。

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220106-093430@2x.png" alt="WX20220106-093430@2x" style="zoom: 25%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220106-093506@2x.png" alt="WX20220106-093506@2x" style="zoom: 50%;" />

参考：

https://zhuanlan.zhihu.com/p/115535965

https://post.smzdm.com/p/ad29zgep/

