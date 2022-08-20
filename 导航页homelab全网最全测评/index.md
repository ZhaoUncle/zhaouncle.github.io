# 史上最全全网最强，导航页测评

<!--more-->

# 一、劝诫篇

⚠️⚠️⚠️有些导航页需要在使用过程中学习一些专业知识，比如 hugo、hexo、wordpress，不是什么开源产品都可以无脑使用的，还是需要学习一些开源产品的专业知识，耗费时间和精力这种就毋庸置疑了，毕竟不是一些 ui 简单的配置，希望大家量力而行，我也没打算全部都详写，只做一些点睛之笔，如果有需要详写的，我会看评论再考虑是否来做一个测评，请见谅！

# 二、何谓导航页？

导航页的关键词就这么几个了，有能力的自己可以去搜一搜，Homepage、homelab、self-hosted、 startpage、single-page。

网址导航（Directindustry Web Guide），也就是导航页，其实是指的一个网页把很多网址整理堆在一块，你拿个 txt 文件集合一堆网址，并按照一定条件进行分类，这也是一个当做导航页，像我们各个浏览器的书签、各种云笔记，甚至☁️网盘放个文件，都可以实现类似的功能。

比如有自己服务器、NAS 机器，折腾了一堆服务，想要记住各种各样的 ip 和端口，导航页就可以整合，方便自己使用，甚至有的可以设置多用户分门别类分享给其他人。有的导航页，甚至有影音功能，集成了影音系统的 api，把诸如 Jellyfin、Plex、Emby 里面的电影、电视剧、音乐等都整合到一个页面，也叫做 HTPC。

# 三、导航页有哪些？

推荐：Organizr、homer、dashy、flame、Notion

github 更新列表，欢迎大家提交pr或者issues来这里，进行更新，喜欢的话，点下 Star 吧：[https://github.com/ZhaoUncle/Awesome-Homepage](https://github.com/ZhaoUncle/Awesome-Homepage)



| 功能类型                                                                                   | 推荐指数             | 静态导航页 | 动态导航页 | 后台管理 | 用户登录 | demo 页面（直接打开体验）                                                                                                                                                                                                                                  |
| ------------------------------------------------------------------------------------------ | -------------------- | ---------- | ---------- | -------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Heimdall](https://github.com/linuxserver/Heimdall)                                           | ⭐️⭐️⭐️⭐️⭐️ | ✅         |            | ✅       | ✅       | https://heimdall.site<br />功能简单够用了，不过我觉得还是差了点意思，每个用户都要重新设置一次，这个确实很坑，而且自从2019年之后，作者再也不更新，也不合并pull requests了，遗留了一堆问题，不过优点是集成了很多应用程序图标，这个很赞                       |
| [Organizr](https://github.com/causefx/Organizr)                                               | ⭐️⭐️⭐️⭐️⭐️ |            |            | ✅       | ✅       | 可以集成影音系统 api并使用 emby、plex、jellyfin 作为用户管理数据                                                                                                                                                                                           |
| [Muximux](https://github.com/mescon/Muximux)                                                  | ⭐️⭐️⭐️⭐️⭐️ | ✅         |            |          |          | 作为导航页使用足够，可以直接页面进行设置，默认 iFrame ，也更轻量                                                                                                                                                                                           |
| [DashMachine](https://github.com/rmountjoy92/DashMachine)                                     | ⭐️⭐️⭐️⭐️     |            | ✅         | ✅       | ✅       | 自 2020 年 5 月后就没更新过了，不过基于 python，我个人用改起来很方便                                                                                                                                                                                       |
| [homer](https://github.com/bastienwirtz/homer)                                                | ⭐️⭐️⭐️⭐️⭐️ | ✅         |            |          |          | https://homer-demo.netlify.app/<br />纯静态页面，编辑需要通过修改代码，支持 netflix 部署（不花钱），适合不需要太多功能，又喜欢自己编辑静态文件的，比如我                                                                                                   |
| [flame](https://github.com/pawelmalak/flame)                                                  | ⭐️⭐️⭐️⭐️⭐️ | ✅         | ✅         |          |          | 静态在线编辑，最简                                                                                                                                                                                                                                         |
| [dashy](https://github.com/Lissy93/dashy)                                                     | ⭐️⭐️⭐️⭐️⭐️ | ✅         |            | ✅       |          | https://demo.dashy.to/<br /> 非常有意识的导航页，整体风格很香 linux 的，富有科技感，支持部署到云端免费试用                                                                                                                                                 |
| [sui](https://github.com/jeroenpardon/sui)                                                    | ⭐️⭐️⭐️⭐️     | ✅         | ✅         |          |          |                                                                                                                                                                                                                                                            |
| [aquar-home](https://github.com/firemakergk/aquar-home)                                       | ⭐️⭐️⭐️         |            |            |          |          | https://post.smzdm.com/p/a6dn36qz/#comments<br />非常欣赏这个项目，除了导航页，就是集成api，把nextcloud、trunas、pve、rsync==这些集成到一个页面，想法是挺好的，就是人力有穷时，而且还得考虑api的更新带来的影响，我觉得可以关注下，不过我个人目前不会使用。 |
| [极客猿导航](https://github.com/geekape/geek-navigation)                                      | ⭐️⭐️⭐️⭐️     |            | ✅         | ✅       |          | https://nav.geekape.net/<br />类似于博客类型的导航页，可惜作者没有做通用的部署方式，不知道是懒还是没有和运维打交道，我尝试了下自己做个 Docker 版本不好用，劝退了                                                                                           |
| [WebStackPage/WebStackPage.github.io](https://github.com/WebStackPage/WebStackPage.github.io) | ⭐️⭐️⭐️⭐️⭐️ | ✅         |            |          |          | http://webstack.cc/cn/index.html<br />可以直接复制源码修改html页面，对于新手不友好                                                                                                                                                                         |
| [shenweiyan/WebStack-Hugo](https://github.com/shenweiyan/WebStack-Hugo)                       | ⭐️⭐️⭐️         | ✅         |            |          |          | https://shenweiyan.github.io/WebStack-Hugo/                                                                                                                                                                                                                |
| [iplaycode/webstack-hugo](https://github.com/iplaycode/webstack-hugo)                         | ⭐️⭐️⭐️         | ✅         |            |          |          | https://iplaycode.github.io/nav/                                                                                                                                                                                                                           |
| [liutongxu/liutongxu.github.io](https://github.com/liutongxu/liutongxu.github.io)             | ⭐️⭐️⭐️         | ✅         |            |          |          | https://liutongxu.github.io/<br />可以直接复制源码修改html页面，对于新手不友好                                                                                                                                                                             |
| [hui-ho/WebStack-Laravel](https://github.com/hui-ho/WebStack-Laravel)                         | ⭐️⭐️⭐️         |            | ✅         | ✅       |          | http://webstack.cc/cn/index.html                                                                                                                                                                                                                           |
| [iTab](https://www.itab.link/)                                                                | ⭐️⭐️⭐️         | ✅         |            |          |          | https://go.itab.link/<br />浏览器上导航页，无需自建服务端                                                                                                                                                                                                  |
| [Hugo Themes Slate](https://github.com/gesquive/slate)                                        | ⭐️⭐️             | ✅         |            |          |          | https://gesquive.github.io/hugo-slate-demo/                                                                                                                                                                                                                |
| [fr0tt/benotes](https://github.com/fr0tt/benotes)                                             | ⭐️⭐️             |            | ✅         |          | ✅       | 无，只是可以做导航页功能                                                                                                                                                                                                                                   |
| [easy-bookmark-manager](https://github.com/devimust/easy-bookmark-manager)                    | ⭐️⭐️⭐️         |            | ✅         |          |          | 无，浏览器有插件导入，更像一个书签管理器，可惜已经不更新了                                                                                                                                                                                                 |
| [poulainv/tottem](https://github.com/poulainv/tottem)                                         | ⭐️⭐️⭐️         |            |            |          | ✅       | [beta.totem.app](https://beta.tottem.app/)<br />不是纯导航，也是类似书签功能，可惜也不更新了                                                                                                                                                                  |
| [go-shiori](https://github.com/go-shiori/shiori)                                              | ⭐️⭐️⭐️⭐️⭐️ |            |            |          |          | 也是一个书签管理器，浏览器有插件导入书签，也可作为导航页（附加）                                                                                                                                                                                           |
| [LinkAce](https://github.com/Kovah/LinkAce)                                                   | ⭐️⭐️⭐️⭐️     |            |            | ✅       | ✅       | https://demo.linkace.org/guest/links<br />也是一个书签管理器，浏览器有插件导入书签，也可作为导航页（附加                                                                                                                                                   |
| [wekan](https://github.com/wekan/wekan)                                                       | ⭐️⭐️⭐️⭐️⭐️ |            |            |          |          | 这是一个看板系统，不过也能作为导航链接使用                                                                                                                                                                                                                 |
| [5iux](https://github.com/5iux/sou)                                                           | ⭐️⭐️             |            |            |          |          | https://sou.5iux.cn/<br />有 php 和静态页面版本，更像是一个浏览器主页，不过可以作为导航页使用而已                                                                                                                                                          |
| [Portal-Lite](https://github.com/Privoce/Portal-Lite/blob/main/README.zh.md)                  | ⭐️⭐️⭐️         |            |            |          |          | http://nicegoodthings.com/<br /> 有浏览器插件，服务端是对方服务器                                                                                                                                                                                          |
| [nightTab](https://github.com/zombieFox/nightTab)                                             | ⭐️⭐️⭐️         |            |            |          |          | https://zombiefox.github.io/nightTab<br />标签类型的导航页                                                                                                                                                                                                 |
| [Bento](https://github.com/migueravila/Bento)                                                 | ⭐️⭐️⭐️         | ✅         |            |          |          | https://migueravila.github.io/Bento/                                                                                                                                                                                                                       |
| [startpage](https://github.com/deepjyoti30/startpage)                                         | ⭐️⭐️             |            |            |          |          | Chrome 和 Firefox 浏览器起始页面，可以作为导航页使用，极简风                                                                                                                                                                                               |
| [tilde](https://github.com/cadejscroggins/tilde)                                              | ⭐️⭐️             | ✅         |            |          |          | https://tilde.cade.me/<br />就一个 html 页面                                                                                                                                                                                                               |
| [homepage](https://github.com/Jaredk3nt/homepage)                                             | ⭐️⭐️             |            |            |          |          | 就一个 html 页面加几个js                                                                                                                                                                                                                                   |
| [Prismatic-Night](https://github.com/3r3bu5x9/Prismatic-Night)                                | ⭐️⭐️⭐️         |            |            |          |          | 浏览器主题页面，很有意思                                                                                                                                                                                                                                   |
| [pomme-page](https://github.com/kikiklang/pomme-page)                                         | ⭐️⭐️             |            |            |          |          |                                                                                                                                                                                                                                                            |
| [fluidity](https://github.com/PrettyCoffee/fluidity)                                          | ⭐️⭐️             |            |            |          |          | 浏览器起始页面                                                                                                                                                                                                                                             |
| [startup-page](https://github.com/timothypholmes/startup-page)                                | ⭐️⭐️             | ✅         |            |          |          |                                                                                                                                                                                                                                                            |
| [startpages](https://github.com/grtcdr/startpages)                                            | ⭐️⭐️             | ✅         |            |          |          |                                                                                                                                                                                                                                                            |
| [the-glorious-startpage](https://github.com/manilarome/the-glorious-startpage/)               | ⭐️⭐️             | ✅         |            |          |          | Firefox 和 Chrome 浏览器起始页                                                                                                                                                                                                                             |
| [smashing](https://github.com/Smashing/smashing)               |              |          |            |          |          | http://dashingdemo.herokuapp.com/sample                                                                                                                                                                                                                             |
| [homelab_proxy](https://github.com/jmztaylor/homelab_proxy)               |              |          |            |          |          |                                                                                                                                                                                                                             |
                                      |
| [simple-dash](https://github.com/kutyla-philipp/simple-dash)               |              |          |            |          |          |    https://kutyla-philipp.github.io/simple-dash/                                                                                                                                                                                                                         |

# 四、安装前准备

## 4.1 安装 git 工具

直接安装插件 [NerdPack GUI](https://forums.unraid.net/topic/35866-unraid-6-nerdpack-cli-tools-iftop-iotop-screen-kbd-etc/) 就可以了

![WX20220108-171308@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220108-171308@2x.png)

## 4.2 安装 mysql

### 4.2.1 手动安装

```
Name: mysql57
Repository: mysql:5.7
Docker Hub URL: https://registry.hub.docker.com/_/mysql/
Icon URL: https://cdn.jsdelivr.net/gh/ZhaoUncle/Unraid@main/templates/images/MySQL.png
Post Arguments: --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci

Host Path 1: /mnt/user/appdata/mysql57/data	### mysql 的数据寸本地
Container Path: /var/lib/mysql

Host Path 2: /mnt/user/appdata/mysql57/conf.d	### mysql 配置文件
Container Path: /etc/mysql/conf.d

Port: 3307	### mysql 映射端口
Container Port: 3306

MYSQL_ROOT_PASSWORD: my-secret-pw	### mysql root 密码
Container Variable: MYSQL_ROOT_PASSWORD

MYSQL_DATABASE: test	### mysql test 库名称，可自行替换，如果这个不用，后面 MYSQL_USER 和 MYSQL_PASSWORD可以一起删了
Container Variable: MYSQL_DATABASE

MYSQL_USER:test_user	### mysql test 库用户，会自动授权 test 库
Container Variable: MYSQL_USER

MYSQL_PASSWORD: test_pw	### mysql test 库用户密码，会自动授权 test 库
Container Variable: MYSQL_PASSWORD

```

![WX20220109-204230@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-204230@2x.png)

![iShot2022-01-09 20.45.39](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/iShot2022-01-09%2020.45.39.png)

### 4.2.2 你也可以在“APPS”里面安装，简单一点

![WX20220109-205228@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-205228@2x.png)

### 4.2.3 创建数据库

1. 点击 mysql57 的头像，然后进入“Console”命令行界面

![WX20220109-232614@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-232614@2x.png)

2. 使用 root 账号登录 mysql，然后创建一个库

```
### 进入 mysql 数据库，-p 后面的是 root 的密码，docker 里面的变量 MYSQL_ROOT_PASSWORD
mysql -uroot -pmy-secret-pw
### 创建 webstack_laravel db 库
CREATE DATABASE IF NOT EXISTS webstack_laravel default charset utf8mb4 COLLATE utf8mb4_general_ci;
### 查看webstack_laravel 是否创建成功
show databases;
```

![WX20220109-232921@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-232921@2x.png)

## 4.3 安装 Filebrowser 导入文件，省去命令行操作

### 4.3.1 安装 荒野无灯大大的 Filebrowser

```
Name: Filebrowser
Repository: 80x86/filebrowser:amd64
Docker Hub URL: https://hub.docker.com/r/80x86/filebrowser
Icon URL: https://cdn.jsdelivr.net/gh/ZhaoUncle/Unraid@main/templates/images/FileBrowser.png
WebUI: http://[IP]:[PORT:8082]
Extra Parameters: --device=/dev/dri/renderD128:/dev/dri/renderD128

Path1: /mnt/user/appdata/filebrowser/config   ### 这个是filebrowser的配置挂载到本地，避免更新镜像丢失数据
Container Path: /config

Path2: /mnt/user   ### 这个是吧 unraid 的本地目录挂载到 filebrowser 进行管理，你也可以单独挂载多个，自己看着办
Container Path: /myfiles

WebUI: 18082   ### web 页面访问端口
Container Port: 8082
```

![WX20220109-174039@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-174039@2x.png)

![iShot2022-01-09 17.39.49](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/iShot2022-01-09%2017.39.49.png)

或者你不想就去 **APPS** 里面安装

![WX20220109-174012@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-174012@2x.png)

### 4.3.2 拿安装 Hugo Themes WebStack 举个简单例子，仅供参考

1. 点击下载安装代码包[WebStack-Hugo.zip](https://github.com.cnpmjs.org/shenweiyan/WebStack-Hugo/archive/refs/heads/main.zip)

![WX20220109-172439@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-172439@2x.png)

2. Filebrowser 在 appdata 创建**Navigation-Hugo-WebStack/themes** 文件夹，不需要一个一个来，会自动按目录嵌套创建多级文件夹，创建完后如果看不到，刷新一下浏览器就好了

![WX20220109-174814@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-174814@2x.png)

![WX20220109-191917@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-191917@2x.png)

3. 上传文件

![WX20220109-192000@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-192000@2x.png)

![WX20220109-192102@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-192102@2x.png)

4. 解压文件，一定要先点击.zip文件，才会显示出解压按钮，然后默认解压，点击“UNARCHIVE”就好，一定要刷新一下浏览器，不然什么都看不到

![WX20220109-192124@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-192124@2x.png)

![WX20220109-192437@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-192437@2x.png)

![WX20220109-192724@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-192724@2x.png)

5. 然后重命名一下解压后的文件夹，hugo 的用法里面一般都是和 github 仓库名称一致

![WX20220109-192831@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-192831@2x.png)

![WX20220109-192849@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-192849@2x.png)

6. 至于 zip 包文件，直接删了也行，留着做备份也行，避免改错文件可以直接解压在覆盖，不过我一般都删了

![WX20220109-193041@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-193041@2x.png)

7. 要将"appdata/Navigation-Hugo-WebStack/themes/WebStack-Hugo/exampleSite/"下面的几个文件全部复制到“appdata/Navigation-Hugo-WebStack/” ，直接全部选中然后点击复制按钮就好了

![WX20220109-193801@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-193801@2x.png)

![WX20220109-193845@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-193845@2x.png)

![WX20220109-193931@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-193931@2x.png)

![WX20220109-193948@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-193948@2x.png)

# 五、Unraid 安装 Organizr

github: [https://github.com/causefx/Organizr]()

Organizr 的初始化过程中也是需要从网上（比如 github）下载一堆文件，网络不好的同学可能会卡在这一步，暂时没打算写个相关脚本去替换外网的资源，请留意！

![53615856-35cc5a80-3b9d-11e9-8428-1f2ae05da2c9](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/53615856-35cc5a80-3b9d-11e9-8428-1f2ae05da2c9.png)
![53615857-35cc5a80-3b9d-11e9-82bf-91987c529e72](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/53615857-35cc5a80-3b9d-11e9-82bf-91987c529e72.png)
![53615858-35cc5a80-3b9d-11e9-8149-01a7fcd9160a](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/53615858-35cc5a80-3b9d-11e9-8149-01a7fcd9160a.png)
![CJ8OziG-1024x552](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/CJ8OziG-1024x552.jpeg)
![organizr](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/organizr.png)
![taco-extra](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/taco-extra.png)
![fin-chat-1024x640](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/fin-chat-1024x640.png)

## 5.1 直接在**APPS**里面安装

![WX20220112-150312](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220112-150312.png)
![WX20220112-154316](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220112-154316.png)
![WX20220112-160640](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220112-160640.png)

## 5.2 手动安装

```
Name: Organizr
Overview: https://github.com/causefx/Organizr
Repository: organizr/organizr
Docker Hub URL: https://hub.docker.com/r/organizr/organizr/
Icon URL: https://raw.githubusercontent.com/causefx/Organizr/v2-master/plugins/images/organizr/logo-no-border.png
WebUI: http://[IP]:[PORT:80]/

Branch: master
Container Variable: branch

PUID: 99
Container Variable: PUID

PGID: 100
Container Variable: PGID

ConfigPath: /mnt/user/appdata/organizr
Container Path: /config

WebUI: 21881
Container Port: 80
```

![iShot2022-01-13 17.10.42](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/iShot2022-01-13%2017.10.42.png)

## 5.3 安装后初始化相关

1. 安装类型：直接选择Personal就好了

| 许可证类型 | 信息                         |
| ---------- | ---------------------------- |
| Personal   | 一切都已解锁 - 没有任何隐藏  |
| Business   | 所有与媒体相关的项目均已隐藏 |

![WX20220113-151911](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220113-151911.png)

2. 管理员信息：

Organiz 同时作为一款影音系统，可以直接拿plex和emby作为后端，匹配这两个网站相同的用户名、电子邮箱、密码的话，使用SSO功能会更方便，也可以后期重新配置，这里我就不做设置了

![WX20220113-152116](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220113-152116.png)

3. 安全：

| 字段                  | 信息                                                                |
| --------------------- | ------------------------------------------------------------------- |
| Hash Key              | 这是用于散列配置文件中所有密码的 salt，简单理解就是用来加密的字符串 |
| Registration Password | 这是任何人都需要为您注册的字段 Organizr                             |
| API Key               | 自动生成，用于访问 Organizr 的数据                                  |

![WX20220113-152146](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220113-152146.png)

4. 数据配置：

Organizr 使用 sqlite 作为其数据库，所以只需要指定数据库名称和存储路径就好了，然后单击“test/create”按钮，查看是否一切正常。

| 路径                          | 信息                                         |
| ----------------------------- | -------------------------------------------- |
| Suggested Directory(推荐目录) | 这是在 Organizr 所在的父目录中命名的新目录db |
| Current Directory(当前目录)   | Organizr 所在的位置                          |
| Parent Directory(父目录)      | 这是 Organizr 所在的父目录                   |

- Database Name：这里填写organizer
- Database Location：这里填写上面的 Suggested Directory(推荐目录) /config/www/db 就好了

![WX20220113-152247](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220113-152247.png)

5. 核验：
   完成前面所有步骤后，将进入概览页面，有需要重新验证一下密码的，就把鼠标悬停在字段对应的Hover to show 上面就好了。如果都没啥完成，点击 Finish 完成，就会进行初始化设置，创建数据库信息了，然后就可以正式进入系统了，密码账号这些都可以进去系统后再改，问题不大，直接过了吧。

![WX20220113-152308](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220113-152308.png)

6. 系统登录界面概览：![WX20220113-180651](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220113-180651.png)

![WX20220113-152429](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220113-152429.png)

## 5.4 使用

### 5.4.1 Tabs 导航链接管理

1. 修改完后要刷新浏览器才会生效
2. 进入"Settings"-->"TAB EDITOR"-->"Tabs"

```
NAME：名称
CATEGORY：类别，Tabs 进行分组使用
GROUP：用户组
TYPE：url 打开类型
	- Organizr：内部打开，目前只有 Settings 和 Homepage 需要，其他用不得
  - iFrame：理解为内嵌页面打开，不需要打开一个新窗口，iFrame 在严格安全模式下的话，是需要设置域名或者 ip 来使用，通常我们会通过 nginx 进行反代来去除安全限制，大部分应用不需要，又遇到再自己研究去，我这里不做举例
  - New Window：新的窗口页面打开
DEFAULT：设置为登录打开默认页面，一般设置 Homepage页面
ACTIVE：打勾就是在线，不打勾就是隐藏
SPLASH：页面闪入，可以忽略
PING：监控检查页面
PRELOAD：定时加载页面
EDIT：修改页面配置
DELETE：删除
```

![WX20220114-235915@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220114-235915@2x.png)

```
Add New Tab
Tab Name：导航链接显示名称
Tab URL： 导航 url，可以是基于 oragnizr 当前访问地址的/uri，也可以是 http:// 或者 https://
Tab Local URL：
Ping URL：ping 功能 url，格式为 host/ip:port
Tab Auto Action：None、Auto close、Auto Reload
Tab Auto Action Minutes：选项卡动作的时间，单位分钟
### 以下三选一就好，正常使用Choose Image 作为 tab 导航图标
Choose Image：因为内部集成了很多nas 相关应用的图标，所以输入对应字符串会正则匹配， 没有你在自动上次
Choose Icon：图标选择，默认就好
Choose Blackberry Theme Icon：主题图标，默认带有黑白色的图标，看你个人风格了
Tab Image：这里可以手动填写地址，也可以上面 3 选 一之后会自动匹配，没有就只能你自己弄图标了

```

![WX20220114-232816@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220114-232816@2x.png)
![WX20220115-072022@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-072022@2x.png)

### 5.4.2 设置类别，导航组

进入"Settings"-->"TAB EDITOR"-->"Categories"

![WX20220115-000043@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-000043@2x.png)
![WX20220115-000121@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-000121@2x.png)

### 5.4.3 设置 Homepage 主页面

1. 进入"Settings"-->"TAB EDITOR"-->"HomePage Items"
2. Homepage 主页面我会习惯性放在第一行，设置放第二行，其他应用按分组放后面
3. Homepage 主页面的设置主要是因为 Organizr 设置了很多 NAS 应用的 api，比如可以把 jellyfin、emby、plex 里面的电影电视==这些放在主页进行展示，可以一个页面就进去不同的影音系统，或者其他系统，而不需要打开新的 url，分别进行查找，整合了很多应用，这是这个系统最大的优势点。
4. 一定要刷新浏览器才会生效哦

![WX20220115-000601@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-000601@2x.png)

### 5.4.4 Homepage 页面顺序排列

自己按照喜好移动👇🏻的方块就好了

![WX20220116-113146@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220116-113146@2x.png)

### 5.4.5 设置 Jellyfin 举例

主要是Jellyfin 生成 api token 然后填写进去启用就好了，要进行 test 检查是否成功，然后刷新浏览器是否生效就好了。如果是要外网访问的话，一定要填写外网访问ip或者域名。

![WX20220115-131116@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-131116@2x.png)
![WX20220115-131136@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-131136@2x.png)
![WX20220115-131158@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-131158@2x.png)
![WX20220115-131222@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-131222@2x.png)
![WX20220115-131232@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-131232@2x.png)
![WX20220115-131301@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-131301@2x.png)
![WX20220115-131347@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-131347@2x.png)
![WX20220115-131402@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-131402@2x.png)
![WX20220115-131841@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-131841@2x.png)

### 5.4.6 关闭提示信息

ORGANIZR NEWS，不关闭会一直在闪，特别烦，鼠标点击 Feature Request Site Migration，然后点击“Click me to ignore”
![WX20220113-152833](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220113-152833.png)

### 5.4.7 更改左上角 Title "ORGANIZE V2" 为 logo 图

1. 去"Settings"-->"CUSTOMIZE"-->"Appearance"-->"Top Bar"
2. 把 “Use Logo instead of Title”打勾就是只显示为“Logo URL”内置的 logo 图片，你可以手动改为你自己的，如果你只改 "Organizr Title" 的话，就是把"ORGANIZE V2" 字样改成你自己的字样
3. 最后右上角“save”保存，然后刷新浏览器才会生效

![WX20220113-230946@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220113-230946@2x.png)
![WX20220113-231052@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220113-231052@2x.png)

### 5.4.8 更改登录页面

📢：更改主题Themes，会更改登录页面整个效果图，而不是小调整，请注意。

1. 去"Settings"-->"CUSTOMIZE"-->"Appearance"-->"Login Page"

```
Login Logo URL: 登录 logo，这里使用默认的 organizr 图标
Use Logo instead of Title on Login Page：这里开启，就会显示👆🏻的图标在登录页面头顶
Login Wallpaper URL: 登录页面背景图，不喜欢原图就自己改掉，https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/meinv4k.jpg
Minimal Login Screen:最小登录屏幕，就是除了登录页面这里，其他都关掉不显示
```

![WX20220114-001528@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220114-001528@2x.png)
![WX20220114-002151@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220114-002151@2x.png)
![WX20220114-002229@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220114-002229@2x.png)

2. 这里发现一个 bug，如果背景图片链接太长，会导致出现显示问题

![WX20220114-001707@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220114-001707@2x.png)

### 5.4.9 更改主题

1. 去"Settings"-->"CUSTOMIZE"-->"Marketplace"-->"Theme Marketplace"-->"Install 下载你需要的主题包"
2. 然后再去"Settings"-->"CUSTOMIZE"-->"Appearance"-->"Customize Appearance"-->"Colors & Themes"，选择你需要的主题或者样式，进行更改就好了
3. 最后选择右上角的“Save”进行保存，才会生效

![WX20220113-211610@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220113-211610@2x.png)
![WX20220113-211829@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220113-211829@2x.png)
![WX20220113-211918@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220113-211918@2x.png)

### 5.4.10 如何关闭左侧默认自带的几个导航图标

去"Settings"-->"CUSTOMIZE"-->"Appearance"-->"Side Menu"，去掉之后要Save 保存，然后刷新浏览器之后才会生效。

```
Allow Side Menu to be Collapsable：允许侧边菜单可折叠
Side Menu Collapsed at Launch：侧边菜单在启动时折叠
Collapse Side Menu after clicking Tab：单击选项卡后折叠侧边菜单
Show GitHub Repo Link：显示 GitHub 存储库链接
Show Organizr Feature Request Link：显示 Organizer 功能请求链接
Show Organizr Support Link：显示 Organizer 支持链接
Show Organizr Docs Link：显示 Organizer Docs 链接
Show Organizr Sign out & in Button on Sidebar：在侧边栏上显示 Organizr 退出和登录按钮
Expand All Categories：展开所有类别
Auto-Collapse Categories：自动折叠类别
Auto-Expand Nav Bar：自动展开导航栏
Unsorted Tab Placement：未排序的标签放置
```

![WX20220113-213051@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220113-213051@2x.png)

### 5.4.11 重置密码

![WX20220113-210458@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220113-210458@2x.png)
![WX20220113-210531@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220113-210531@2x.png)
![WX20220113-210905@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220113-210905@2x.png)
![WX20220113-211032@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220113-211032@2x.png)

### 5.4.12 外部用户注册功能

用户注册功能可以关闭，默认是开启的，用户注册是需要 Registration Password，也就是注册密码

![WX20220114-234639@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220114-234639@2x.png)

![WX20220114-143906](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220114-143906.png)
![WX20220114-143940](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220114-143940.png)

### 5.4.13 查看 api token

1. 去 "Settings"-->"System Settings"-->"Main"-->"API"， “Organizr API”就是我们需要的key了
2. 如果你要重置就点击Generate New API Key的 Generate，然后保存就会生成新的，并且会保存到`www/organizr/api/config/config.php`的 “organizrAPI”中。
3. 如果你需要查看api文档，就直接点击“Organizr Docs”，可以直接在web端进行api调试，非常方便。

![WX20220114-174245](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220114-174245.png)
![WX20220114-173330](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220114-173330.png)
![WX20220114-175018](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220114-175018.png)

### 5.4.14 忘记密码

方法一：

参考：https://docs.organizr.app/help/faq/forgot-password

配置要求：

1. 使用organizr sqlite默认本地的后端数据，没有使用emby和plex作为后端数据服务
2. 如果没有安装 PHPMailer Plugin，已经安装就可以直接邮箱重置了，没有安装的需要使用api的方式进行安装开启smtp邮箱功能，不需要配置邮箱功能，默认会使用organizr提供的邮件服务器，记得要联网。
3. 版本要求 2.1.165 之后

配置方法：

1. 如果有保留之前生成的api token，可以直接使用，如果不知道的话，就在`organizr/www/organizr/api/config/config.php` 查看 organizrAPI 的配置，其实就是开机自动生成的api token了
2. 直接浏览器访问 http://organizrIP:port/api/v2/help/smtp?apikey=organizrAPI ，显示 "result" : "success" 就表示成功了

![WX20220114-190320](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220114-190320.png)

3. 然后重新进入登录页面，发现多了个重置密码的按钮 “Forgot pwd”，填写对应的账号邮箱后，然后会在右下角显示已发送邮件![WX20220114-190544](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220114-190544.png)![WX20220114-190635](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220114-190635.png)![WX20220114-192549](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220114-192549.png)
4. 收到邮件之后，打开就可以看到重置的密码了

![WX20220114-193224](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220114-193224.png)

5. 你要是邮箱也是随便填的咋办？

答： 那只能通过更改 sqlite 数据库修改了，使用 filebrowser 打开 `organizr/www/db/organizr.db`下载到本地，然后修改“users”表，找到对应的用户，进行修改 “email” 字段为你自己的邮箱📮就好了。提示一下，如果被邮箱重置密码的话，会数据库字段“locked”为 1，如截图那里被锁定

![WX20220114-220103@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220114-220103@2x.png)
![WX20220114-194400](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220114-194400.png)

方法二：

直接注册一个新用户，然后去把 `organizr/www/db/organizr.db` 下载到本地之后，把注册的那个用户的 password 字段复制到 admin 那里，然后在重新上传替换就好了，最好你就可以用你刚注册的用户使用的密码登录 admin 账号了。

![](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202201162232745.png)

### 5.4.15 更改登录之后进来哪一个页面

默认进来是 ”System Settings"，修改路径为"Settings"-->"System Settings"-->"Main"-->"Settings Page"

![WX20220114-155227](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220114-155227.png)

### 5.4.16 更改后端验证类型

进到 "Settings"-->"System Settings"-->"Main"-->"Authentication"，我们可以使用诸如、Jellyfin、Plex、Emby、LDAP、FTP 等服务作为验证后端，我比较建议可以同时使用 “Organizr DB + Backend”这种验证方式，比如，我这里同时存在使用 organizr 数据库和 Jellyfin，只要账号密码，两个系统设置一样，就可以使用 Jellyfin 的用户进行登录了，不需要导入。

![iShot2022-01-14 22.46.56](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/iShot2022-01-14%2022.46.56.png)
![WX20220114-224937@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220114-224937@2x.png)

### 5.4.17 导入Jellyfin系统的用户

正常情况下不需要导入，直接用其他系统作为后端就可以，不过有时候因为我们有很多后端系统，统一导入就不需要重新配置，不过这个时候直接使用 LDAP 去统一管理所有，可能会更方便。

本来初始化的时候就可以设置是使用 Plex 还是 Emby 作为验证后端，不过我直接跳过了，这次我们可以重新设置
进到 "Settings"-->"USER MANAGEMENT"-->"Import Users"
更改后端验证类型这里选了 Jellyfin 之后，我们在用户管理这里可以把 Jellyfin 里面的用户直接导入进去
![WX20220114-230312@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220114-230312@2x.png)

### 5.4.18 用户管理

👆🏻导入Jellyfin 系统的用户之后，你要是直接修改 Organizr 的用户的密码的话，是不会同步到 Jellyfin 的，而且当你尝试使用和 Jellyfin 通用户名的密码登录之后，Organizr 的用户密码就无法在登录了。

用户管理这里，我们最主要的是用初始用户来管理用户，修改用户组还有头像。
![WX20220114-230503@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220114-230503@2x.png)

### 5.4.19 用户组管理

用户组管理，主要是可以分组进行管理用户，然后分配不同的导航页

![WX20220114-232246@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220114-232246@2x.png)

### 5.4.20 插件功能，聊天

插件有 CHAT、HEALTHCHECKS、INVITES、SPEEDTEST、BOOKMARK、PHP MAILER，聊天插件使用的是[PUSHER](https://dashboard.pusher.com/)，可以所有用户在线聊天，插件目前的打开方式是在右上角人头像下面

![WX20220116-102640@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220116-102640@2x.png)
![WX20220116-103019@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220116-103019@2x.png)
![WX20220116-103150@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220116-103150@2x.png)
![WX20220116-103237@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220116-103237@2x.png)
![WX20220116-103614@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220116-103614@2x.png)

### 5.4.21 SSO 单点登录

其实指的是登录 Organizr 时，会登陆其他的系统，前提是你的 organizr 登录用户与其他的系统是同个账号密码才可以。

![](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202201162238425.png)

# 六、Unraid 安装 Heimdall

github: [https://github.com/linuxserver/heimdall]()

![68747470733a2f2f692e696d6775722e636f6d2f4d72433451704e2e676966](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/68747470733a2f2f692e696d6775722e636f6d2f4d72433451704e2e676966.gif)

## 6.1 小总结

1. 支持多用户登录
2. 支持分组分页，问题是，每个用户都需要重新设置一次，很烦
3. 页面编辑简单，不需要改代码，直接在线改，支持图表，隐藏，颜色
4. 可以修改背景页，不过无法更改字体
5. 中文输入可以，但是 tag 分组如果设置中文会导致跳转失败
6. 支持内部嵌套搜索引擎，不过没有显示搜索结果，只能页面进行跳转

## 6.2 直接在 **APPS** 里面安装就好啦

1. 从官方统计发现，下载了还真不少

![WX20220109-233907@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-233907@2x.png)

![WX20220109-233535@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-233535@2x.png)

![WX20220109-233627@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-233627@2x.png)

![WX20220109-233702@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-233702@2x.png)

2. 不需要啥更改，直接把 80 和 443 端口改成其他的不冲突的就够了。

![iShot2022-01-09 23.37.54](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/iShot2022-01-09%2023.37.54.png)

![WX20220109-233830@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-233830@2x.png)

![WX20220111-160506](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220111-160506.png)

## 6.3 举一个手动安装的例子，因为就想要手控

⚠️⚠️⚠️ 1. **安装完一定要重启才能正常访问！！！安装完一定要重启才能正常访问！！！安装完一定要重启才能正常访问！！！**

```
Name: heimdall2
Overview: https://github.com/orgs/linuxserver/packages/container/package/heimdall https://github.com/linuxserver/Heimdall
Repository: linuxserver/heimdall:amd64-latest ### 可以直接使用“lscr.io/linuxserver/heimdall”，就不需要去判断是哪个cpu架构
Docker Hub URL: https://hub.docker.com/r/linuxserver/heimdall/
Icon URL: https://cdn.jsdelivr.net/gh/ZhaoUncle/Unraid@main/templates/images/heimdall-logo.png
WebUI: http://[IP]:[PORT:80]

http: 21880 
Container Port: 80

https: 21443
Container Port: 443

PUID: 99
Container Variable: PUID

PGID: 100
Container Variable: PGID

ConfigPath: /mnt/user/appdata/heimdall2 ### 挂载配置存到本地
Container Path: /config

```

![iShot2022-01-11 14.40.43](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/iShot2022-01-11%2014.40.43.png)

![WX20220111-120501@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220111-120501@2x.png)

2. 为何要重启的原因，是因为会导致访问失败，我发现是生产key的时候导致文件权限不对，重启之后会重新授权就正常了，具体自己看截图吧

![WX20220111-120449@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220111-120449@2x.png)

3. 重启前的文件权限为root，并且启动失败，进程没启动![WX20220111-120530@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220111-120530@2x.png)

![WX20220111-120619@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220111-120619@2x.png)

4. 重启后的文件改为uid=99，gid=100的nobody用户了，启动就正常了

![WX20220111-145128](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220111-145128.png)

![WX20220111-120637@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220111-120637@2x.png)

## 6.4 使用

📢：默认创建好打开就是没有密码的，直接就是 admin 用户进来，进来之后，我们设置一下账号密码，就可以限制别人访问打开了。

### 6.4.1 当前页面管理

点击编辑符号就会显示编辑效果，可以直接跳转到导航链接页面或者 tag 管理页面进行编辑，也可以显示或者隐藏标签组或者链接

![WX20220112-000259@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220112-000259@2x.png)
![WX20220112-000312@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220112-000312@2x.png)

![WX20220112-000524@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220112-000524@2x.png)

### 6.4.2 首页

![WX20220111-221946@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220111-221946@2x.png)

### 6.4.3 应用管理，创建导航链接

⚠️⚠️⚠️ 1. 每个用户的链接都是单独设置，不能通用，也就是每个新用户都要手动设置新的导航链接

```
Add application：添加应用

Application Name（必填）: 应用或者服务名称，可自定义，也可输入会正则匹配内部有的
Application Type：应用类型，默认为 None，目前由 212 个集成内置图标配置，比如我输入 plex在 application name，这里会自动正则匹配弹出内置的选项
Colour：颜色，点击会有弹出颜色选择器，选择你喜欢的就行
URL（必填）：一般是服务地址，比如 docker 的访问地址，只能填一个，而且必须带 http://或者 https:// 标志，否则会有一些跳转问题，我在线举个例子
Tags (Optional)：标签🏷，默认是 Home dashboard 即主面板首页
Upload a file：照片或者图标上传，如果是内置的应用，Type 类型选好后就会自动弹出了
PINNED：打勾就是不隐藏，在页面可以看得到，不打勾就是隐藏，在页面无法观看，可以进入当前页面管理器编辑为显示状态。

Preview：预览，👆🏻填完后，下面会弹出预览样式
```

![WX20220111-163845](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220111-163845.png)

2. 如果是已经集成了图片，那么输入关键字“jel”就会弹出对应的图标；如果没有集成，那么就需要手动上传图片，自己改颜色就可以了,

![WX20220111-165629](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220111-165629.png)
![WX20220111-170050](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220111-170050.png)
![WX20220111-210428@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220111-210428@2x.png)

3. 比如我Application Type 不选（默认为 None），即便其他的选了，也会报错“The url field is required.”![WX20220111-205059@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220111-205059@2x.png)

![WX20220111-211921@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220111-211921@2x.png)

4. URL 没有设置 http 或者 https 的话，就会变成 当前 host+uri 跳转的模式，比如下图的“Bitwarden”配置就会跳转成我的 heimdall 地址+url 的跳转，造成访问失败

![WX20220111-232641@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220111-232641@2x.png)

![WX20220111-232723@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220111-232723@2x.png)

### 6.4.4 增强型应用程序

📢：其实说白了，就是集成了一些默认的api，展示一些加载数据，会开发的同学，自己都能魔改需要的

![WX20220112-145611](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220112-145611.png)
![WX20220112-145511](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220112-145511.png)

### 6.4.5 用户管理

📢：admin 密码默认为空

```
Username（必填）：用户名
Email（必填）：邮箱，可以随便填
Avatar：头像
Password：密码
Confirm Password：重复密码
Allow public access to front - Only enforced if a password is set. 这里就是设置那个账户为 public 公开页面，不选，默认为首页公开
Allow logging in from a specific URL. Anyone with the link can login. 用户登录页面，允许有特定 url 访问，用于分享使用比较多，这样对方就不会知道你的其他主页里面有啥子了，
```

![WX20220111-212033@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220111-212033@2x.png)
![WX20220111-214534@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220111-214534@2x.png)

### 6.4.6 标签管理，也可以叫做分组管理

⚠️⚠️⚠️ 1. 每个用户的标签组也是单独设置，不能通用，也就是每个新用户都要手动设置新的标签分组
⚠️⚠️⚠️ 2. 用中文会导致链接跳转失败为 404，比如下面那个“隐藏”

3. 创建一个tag标签分组，名称为隐藏，不勾选“Pinned”，那么改标签就会隐藏，不在首页展示了

![WX20220111-233108@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220111-233108@2x.png)
![WX20220111-233244@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220111-233244@2x.png)
![WX20220111-233302@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220111-233302@2x.png)

4. 想要在首页展示，那么有两种方式，一种是按照最👆🏻开始那里重新打勾“Pinned”，第二种就是使用当前页面管理编辑器直接编辑就是了，如下所示

![WX20220111-234312@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220111-234312@2x.png)

5. tag 中文跳转失败如下

![WX20220111-235040@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220111-235040@2x.png)

![WX20220111-235053@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220111-235053@2x.png)

### 6.4.7 系统设置

![WX20220112-112554](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220112-112554.png)

这里着重讲一下 `Link opens in`的配置

- Open in this tab : 所有跳转链接在当前页面打开
- Open in the same tab : 所有跳转链接都只会在一个新的页面打开，但是每点击一次都会覆盖上个页面，不能多个链接多个页面进行跳转
- Open in a new tab : 所有跳转链接每次都会打开不同的页面，不会覆盖，一般用这个

![iShot2022-01-12.11.26.12](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/iShot2022-01-12.11.26.12.png)

### 6.4.8 设置公开访问页面

1. 无密码：被别人看到也无所谓，可以随意分享，大家都能看
2. 创建一个 public 的用户，然后将其设置为置顶页面，或者称作公开页面，又或者叫主页面，用户一打开就是这个。一定要勾选“Allow public access to front - Only enforced if a password is set.”

![WX20220111-215027@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220111-215027@2x.png)

3. 如果一个分享页你都不想被别人直接打开可以看到，那就在 public 用户设置密码就好了，打开虽然可以看到你设置的导航链图标，但是点击图标的话，还是需要登录密码的

![WX20220111-215944@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220111-215944@2x.png)
![WX20220111-220728@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220111-220728@2x.png)
![WX20220111-220846@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220111-220846@2x.png)

### 6.4.9 中文汉化

1. 进入容器或者使用 filebrowser 挂载代码路径`/var/www/localhost/heimdall/resources/lang/`下来直接进行更改就好了，直接复用一下其他语言的php 脚本，然后拿下来更改就行了，本文有介绍 filebrowser 安装方式，这里就不做过多介绍了

![WX20220112-001817@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220112-001817@2x.png)

2. 我这里使用的是`dengshenkk`提供的的git pull requests 的代码块，https://github.com/linuxserver/Heimdall/pull/617/files
3. 需要创建这几个文件`app.php  auth.php  pagination.php  passwords.php  validation.php`

![WX20220112-001959@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220112-001959@2x.png)
![WX20220112-002402@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220112-002402@2x.png)

![WX20220112-002537@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220112-002537@2x.png)

![WX20220112-092918](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220112-092918.png)

4. 最简单的办法，就是把上面创建的文件，全部都丢到`de`的目录里面，这样就不需要做下面的操作了，直接选择de就是中文界面，好处就是不用修改太多地方（适合小白），坏处也显而易见，看着别扭。

![WX20220112-111701](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220112-111701.png)
![WX20220112-111752](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220112-111752.png)

5. 需要让代码生效有两种方式，一种是直接修改数据库，一种是初始化配置
6. 第一种：将`heimdall2/www/app.sqlite`的代码下载下来，然后修改`settings`表里面的数据，添加`"zh":"Chinese (Chinese)"`数据就好了，然后好像要重启一下heimdall

![WX20220112-110240](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220112-110240.png)
![WX20220112-110124@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220112-110124@2x.png)

7. 第二种：修改`heimdall2/code/database/seeds/SettingsSeeder.php`，把`'zh' => 'Chinese (Chinese)',` 代码块加到`language_options`里面就好了，然后删除`heimdall2/code/database/app.sqlite`，重启就好了，不会删除掉之前的配置，这里只是重新生成一些配置项然后覆盖掉原来的`heimdall2/www/app.sqlite`里面的某些初始配置

📢 8. 如果遇到：没有显示chinese的选项，请再次删除掉 `heimdall2/code/database/app.sqlite`，然后直接去heimdall 的设置里面刷新页面，就会显示了，然后选择chinese中文之后，再去重启heimdall

![WX20220112-110637](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220112-110637.png)
![WX20220112-112010](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220112-112010.png)

### 6.4.10 添加百度搜索🔍

1. 挺无语的，搜索竟然不能手动配置，代码写死了，真是无奈
2. 最简单的办法就如同👆🏻一样，直接把 QWANT 的链接改成百度的就好了，就不需要折腾太多代码的变更，然后重启一下heimdall，修改`heimdall2/code/app/Search.php`；然后去`heimdall2/code/resources/lang/zh/app.php`里面把'options.qwant' => 'Qwant'改成'options.qwant' => '百度'就好了，如果你是用了默认英文的，同理也是进去改`heimdall2/code/resources/lang/en/app.php`

![WX20220112-115014](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220112-115014.png)
![WX20220112-143929](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220112-143929.png)
![WX20220112-144054](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220112-144054.png)
![WX20220112-144006](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220112-144006.png)

3. 想要好看点，就需要改代码了，我推荐`Agurato`提供的的git pull requests 的代码块作为参考使用这里不做介绍了，有能力的同学请参考：https://github.com/linuxserver/Heimdall/pull/617/files

### 6.4.11 修改密码

1. 因为sqlite3里面的密码是加密过的，所以忘记密码只能去数据库密码删掉，在进行设置admin账号的密码，其他账号忘记的话，就直接使用admin账号处理就好了
2. 两种办法，1是直接Console进命令行，安装sqlite3工具，然后进行处理；2是使用比如navicat这种数据库管理工具，用filebrowser下载app.sqlite数据库文件，改好再上传覆盖
3. 方法一：

```
### 安装sqlite3工具
apk add sqlite  
### 进入sqlite数据库
sqlite3 /config/www/app.sqlite 
### 更新admin密码为空，如果你想要重置其他账号，直接把admin改了就好，比如username='要修改的用户名'
UPDATE "users" SET "password" = '' WHERE username = "admin";
### 查看admin密码是否清掉了，|后面啥都没就表示清空密码了
select username,password from users where  username = "admin";
### 退出sqlite编辑终端，也可以直接关闭console
.quit
```

![WX20220112-142207](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220112-142207.png)
![WX20220112-143131](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220112-143131.png)

4. 方法二：找到users表直接双击admin账号后面的password字段，删掉就好了

![WX20220112-143206](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220112-143206.png)
![WX20220112-143229@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220112-143229@2x.png)
![WX20220112-143301@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220112-143301@2x.png)

## 7. 遇到的问题

### 7.1 我发现，官方集成的 APPS 里面的docker，默认把挂在路径和 **PUID**  和 **PGID** 的变量都设置好了，并且在 web 端配置是没有更改的选项，也不需要你做任何的设置

![WX20220109-233830@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202201102326728.png)

1. 我也很好奇为啥会这样？原因如下
2. 该镜像为了 unraid 做了适配，unraid里面默认uid=99和gid=100的用户nobody已经创建好了

![WX20220111-105942@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220111-105942@2x.png)

3. 我试过把 uid 和 gid 改成了 root，uid=0和gid=0 ，发现也是启动不了的，原因是docker内部镜像的系统比如php服务，授权的是abc 用户（uid=99,gid=100），我把php文件收全程root了，启动凉凉

![iShot2022-01-11 11.02.32](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/iShot2022-01-11%2011.02.32.png)

![WX20220111-000811@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220111-000811@2x.png)

4. 懂点运维知识的同学可以从以下图看出问题，👈🏻左边是正常启动的，👉🏻右边是授权为0后启动的，docker里面的php代码授权的是abc用户启动，nginx的服务用户和abc用户其实是同个组gid。

![WX20220111-000731@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220111-000731@2x.png)

### 7.2 有的人网络不好情况下，heimdall 安装后会有一个初始化的时间，因为heimdall会去下载不同application 服务文件下来，具体可以从 `heimdall2/code/app/SupportedApps.php`看到找到下载地址 `https://apps.heimdall.site/list`

没法上网的同学可以参考：https://post.smzdm.com/p/aqn979zk/p2/#comments

![WX20220112-152232](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220112-152232.png)
![WX20220112-152256](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220112-152256.png)

# 八、Unraid 安装 Homer

```
services: 
  - name: "Applications"  ### 标签分组名称
    icon: "fas fa-cloud"  ### 去 font awesome free icons搜你自己想要的 icon，https://fontawesome.com/v5.15/icons?d=gallery&p=2&m=free 或者 http://www.fontawesome.com.cn/icons-ui/
    items:  ### 导航链接配置
      - name: "Awesome app" ### 导航链接名称
        logo: "assets/tools/sample.png" ### 图标存放地址
        subtitle: "Bookmark example"  ### 副标题
        tag: "app"  ### tag 标签
        url: "https://www.reddit.com/r/selfhosted/" ###访问地址
        target: "_blank" # optional html a tag target attribute
      - name: "Another one"
        logo: "assets/tools/sample2.png"
        subtitle: "Another application"
        tag: "app"
        url: "#"
```

1. 配置可以参考官网：https://github.com/bastienwirtz/homer/blob/main/docs/configuration.md
2. 提供了几个自定义服务的配置方式，具体参考：https://github.com/bastienwirtz/homer/blob/main/docs/customservices.md

![WX20220115-145518@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-145518@2x.png)
![WX20220115-145536@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-145536@2x.png)
![WX20220115-145713@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-145713@2x.png)
![WX20220115-145805@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-145805@2x.png)
![WX20220115-145835@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-145835@2x.png)
![WX20220115-150501@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-150501@2x.png)

# 九、Unraid 安装 Muximux

1. 每次刷新浏览器就会进入首页，看到所有导航链接
2. 可以基于 nginx 增加 .htpasswd 设置密码访问
3. 定位为 HTPC 比导航页更准确
4. 静态配置，页面可以直接进行设置，无需更改代码
5. 不常用链接，建议放在右上角设置中
6. 刷熊当前页面，要使用刷新按钮，而不是刷新浏览器
7. 默认提供 2600 多个图标，可以从中挑选并为每个选项卡选择颜色
8. 拖放项目以在菜单中重新排列它们，木有权重设置

![WX20220115-153403@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-153403@2x.png)
![WX20220115-153440@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-153440@2x.png)
![WX20220115-153454@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-153454@2x.png)

![68747470733a2f2f692e696d6775722e636f6d2f4c4c73487a78582e706e67](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/68747470733a2f2f692e696d6775722e636f6d2f4c4c73487a78582e706e67.png)
![68747470733a2f2f692e696d6775722e636f6d2f4d654d667249342e706e67](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/68747470733a2f2f692e696d6775722e636f6d2f4d654d667249342e706e67.png)
![68747470733a2f2f692e696d6775722e636f6d2f4e79556d7a58372e706e67](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/68747470733a2f2f692e696d6775722e636f6d2f4e79556d7a58372e706e67.png)
![68747470733a2f2f692e696d6775722e636f6d2f376d306b3671422e706e67](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/68747470733a2f2f692e696d6775722e636f6d2f376d306b3671422e706e67.png)
![68747470733a2f2f692e696d6775722e636f6d2f713667773435712e706e67](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/68747470733a2f2f692e696d6775722e636f6d2f713667773435712e706e67.png)

# 十、Unraid 安装 DashMachine

1. 账号密码默认 admin
2. 有些应用 iFrame 不生效，大部分都需要你自己使用 nginx 进行反代加入iFrame 头部 ，具体可参考：https://github.com/rmountjoy92/DashMachine/issues/6
3. 背景图支持在线链接，不用上传也可以
4. 集成了部分程序的 api
5. 提供了很多案例，可以快捷配置和使用

![screenshot1](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/screenshot1.png)
![screenshot2](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/screenshot2.png)
![screenshot3](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/screenshot3.png)
![screenshot4](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/screenshot4.png)

![WX20220115-160335@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-160335@2x.png)
![WX20220115-160411@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-160411@2x.png)
![WX20220115-161037@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-161037@2x.png)
![WX20220115-161624@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-161624@2x.png)
![WX20220115-162104@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-162104@2x.png)
![WX20220115-163245@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-163245@2x.png)
![WX20220115-163343@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-163343@2x.png)
![WX20220115-163434@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-163434@2x.png)
![WX20220115-163500@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-163500@2x.png)

# 十一、Unraid 安装 Flame

![apps](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/apps.png)
![bookmarks](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/bookmarks.png)
![settings](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/settings.png)
![themes](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/themes.png)

## 11.1 安装

![WX20220115-181810@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-181810@2x.png)
![WX20220115-181850@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-181850@2x.png)
![WX20220115-182004@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-182004@2x.png)
![WX20220115-182426@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-182426@2x.png)
![WX20220115-182553@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-182553@2x.png)

![WX20220115-183018@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-183018@2x.png)

## 11.2 配置

进去设置页面，ip:port/settings
![WX20220115-182610@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-182610@2x.png)

### 11.2.1 配置主题

![WX20220115-183422@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-183422@2x.png)

### 11.2.2 配置天气

通过[weather api](https://www.weatherapi.com/signup.aspx)获取天气数据，通过[LatLong](https://www.latlong.net/convert-address-to-lat-long.html)获取经纬度，

![WX20220115-184008@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-184008@2x.png)
![WX20220115-183813@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-183813@2x.png)
![WX20220115-183721@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-183721@2x.png)

### 11.2.3 配置搜索

1. 添加一个百度搜索先，虽然我也不会用，但是必须要有，你说是不是

![WX20220115-184113@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-184113@2x.png)
![WX20220115-191113@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-191113@2x.png)
![WX20220115-191131@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-191131@2x.png)

2. 如果你需要使用其他搜索，使用对应的变量，设置是 prefix，比如使用默认 google 搜索"/g hello"就好了，https://github.com/pawelmalak/flame/wiki/Search-bar

![WX20220115-214626@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-214626@2x.png)

### 11.2.4 配置界面

![WX20220115-204757@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-204757@2x.png)
![WX20220115-212306@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-212306@2x.png)

### 11.2.5 远程 docker 主机

可以通过设置 docker 远程启动端口和 label 标签来控制 docker 容器，不过基于 unraid，我这里就没玩这个了

![WX20220115-220720@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-220720@2x.png)

### 11.2.6 配置 css

其实就是可以自定义自己的 style 样式，不过要懂前端开发知识才行

实验功能，暂时没用到：https://github.com/pawelmalak/flame/wiki/Custom-CSS

### 11.2.7 配置应用程序

1. ip+port/applications 访问，需要登录
2. 在 https://materialdesignicons.com/ 寻找 svg 图标，没有就只能自己上传了

![WX20220115-220843@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-220843@2x.png)
![WX20220115-221208@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-221208@2x.png)
![WX20220115-221809@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-221809@2x.png)
![WX20220115-221843@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-221843@2x.png)
![WX20220115-221858@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-221858@2x.png)

### 11.2.8 配置书签

ip+port/bookmarks 访问，需要登录
![WX20220115-220851@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-220851@2x.png)
![WX20220115-222133@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-222133@2x.png)
![WX20220115-222204@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-222204@2x.png)
![WX20220115-222356@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-222356@2x.png)
![WX20220115-222407@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-222407@2x.png)

找不到应用和书签入口了？

![](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220117-181748.png)

### 11.2.9 中文版

soulteary 苏大大提供了中文版本：https://github.com/soulteary/docker-flame，同时修复了一些问题，如有需要请使用此版本，并给大大的 github 点个 star
![WX20220115-214230@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-214230@2x.png)

# 十二、Unraid 安装 Dashy

github：https://github.com/Lissy93/dashy
demo：https://demo.dashy.to/ https://live.dashy.to/  https://dev.dashy.to/

以后，有空在写文了，很有意思的一个 homelab，哥哥我种草了。

![WX20220115-224322@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-224322@2x.png)
![68747470733a2f2f692e6962622e636f2f4c3859624e4e632f64617368792d64656d6f322e676966](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/68747470733a2f2f692e6962622e636f2f4c3859624e4e632f64617368792d64656d6f322e676966.gif)
![139543020-b0576d28-0830-476f-afc8-a815d4de6def](https://user-images.githubusercontent.com/1862727/139543020-b0576d28-0830-476f-afc8-a815d4de6def.gif)
![config-editor-demo](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/config-editor-demo.gif)
![minimal-view-demo](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/minimal-view-demo.gif)
![theme-config-demo](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/theme-config-demo.gif)

![theme-slideshow</code></code></code></code></code></code></code></code>](https://raw.githubusercontent.com/Lissy93/dashy/master/docs/assets/theme-slideshow.gif)![]()

![workspace-demo](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/workspace-demo.gif)
![WX20220116-002914@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220116-002914@2x.png)
![WX20220116-002931@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220116-002931@2x.png)
![WX20220116-002946@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220116-002946@2x.png)

## 12.1 安装配置

![WX20220115-224442@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-224442@2x.png)
![WX20220115-225657@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-225657@2x.png)
![WX20220115-230121@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-230121@2x.png)
![WX20220115-230152@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-230152@2x.png)
![WX20220115-230257@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-230257@2x.png)
![WX20220115-230323@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-230323@2x.png)

![WX20220115-230512@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220115-230512@2x.png)

## 12.2 配置

文档：https://dashy.to/docs/

# 十三、Notion 如何作为导航页使用

notion 的使用方法，其实就是借助他的几种格式，比如 Table、Board、Gallery 来形成。

浏览器使用 "Web Clipper" 来进行剪辑

!![](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220117-173747@2x.png)

![WX20220116-185902@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220116-185902@2x.png)

![WX20220116-185712@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220116-185712@2x.png)
![WX20220116-185757@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220116-185757@2x.png)
![WX20220116-185827@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220116-185827@2x.png)
![WX20220116-185841@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220116-185841@2x.png)

# 十四、导航页 WebStackPage 推荐指南

地址：https://github.com/WebStackPage/WebStackPage.github.io

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-142307@2x.png" alt="WX20220109-142307@2x" style="zoom:25%;" />

## 说明

1. 该项目可以静态托管，也就是直接复制代码，更改 html 导航位置就可以使用，纯静态
2. 也可以使用后管模式，不过好像没有基于多用户访问，正常来说导航栏也用不到对外分享的这种，加大了维护部署难度，其实用处不大，有时候简单才是最好的。
3. 可以基于以下多种方式部署，本篇文章只介绍 方法 4 .hugo （无后台管理）和 方法 1.php laravel（带后台管理）这两种，其他的不多做介绍了

   ```
   方法1. 使用静态托管
   最简单快速上线自己的导航网站，你可以直接下载本项目修改内容既可部署上线。
   
   方法2. 使用基于 Laravel 搭建的后台系统🔥(感谢@hui-ho提供)
   开源地址：https://github.com/hui-ho/WebStack-Laravel
   
   Docker部署版本:https://hub.docker.com/r/arvon2014/webstack-laravel
   
   方法3. Hexo主题
   开源地址： https://github.com/HCLonely/hexo-theme-webstack
   
   方法4. Hugo主题
   开源地址： https://github.com/iplaycode/webstack-hugo 主题演示： https://iplaycode.github.io/nav/
   
   方法5. 基于Java开发的后台系统🔥(感谢@jsnjfz提供)
   开源地址：https://github.com/jsnjfz/WebStack-Guns
   
   方法6. springboot后台 Nikati-WebStack-Guns ❤️ (感谢Nikati (Nikati)提供)
   开源地址：https://github.com/Nikati/WebStack-Guns-NKT
   
   方法7.1 使用 Jekyll 版本的后台🔥(感谢@0xl2oot提供)
   开源地址：https://github.com/0xl2oot/webstack-jekyll
   
   方法7.2 从Chrome书签生成Jekyll版本配置的工具
   体验网址： https://w.hanxi.info/convert.html 开源地址： https://github.com/hanxi/webstack-jekyll
   
   方法8.1 钻芒二开Typecho主题
   开源地址：https://www.zmki.cn/5366.html 比较详细的安装教程：https://www.waoww.com/typecho-theme/zmki-webstack.html 预览地址：https://tool.zmki.cn/
   
   方法8.2 SEOGO二开Typecho主题
   开源地址：https://www.seogo.me/muban/webstack.html
   
   方法9. 静态博客Gridea主题
   开源地址: https://github.com/lmm214/gridea-theme-webstack 在线预览: https://edui.fun/
   
   方法10. VUE版本
   开源地址: https://github.com/Anjaxs/WebStack-vue/tree/master
   
   方法11. flask-blog-platform
   开源地址: https://github.com/shitianfang/flask-blog-platform/tree/master
   
   方法12. 自己写后台系统
   可以按照自己的喜好和框架搭建后台系统，也可以参考我设计好的后台框架自行搭建。本站设计开发过程在我的博客文章有详细讲到《webstack | viggo》。静态源码（半成品）：https://github.com/WebStackPage/webstack-Admin
   
   如果你有更好的解决方案，并且能够开源供大家使用，可以在本项目提Issus，或者直接通过我个人网站中的联系方式联系我。
   
   JUST DOWNLOAD AND DO WHAT THE FUCK YOU WANT TO.
   ```

# 十五、Unraid 安装 Laravel Themes WebStack

地址：https://github.com/hui-ho/WebStack-Laravel 和 https://github.com/Gourds/WebStackLaravel

![WX20220109-210247@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-210247@2x.png)

## 15.1 创建一个数据库给 docker 用

具体参考： “**安装前准备-->安装 mysql-->创建数据库**”

```
### 进入 mysql 数据库，-p 后面的是 root 的密码，docker 里面的变量 MYSQL_ROOT_PASSWORD
mysql -uroot -pmy-secret-pw
### 创建 webstack_laravel db 库
CREATE DATABASE IF NOT EXISTS webstack_laravel default charset utf8mb4 COLLATE utf8mb4_general_ci;
### 查看webstack_laravel 是否创建成功
show databases;
```

![WX20220109-232921@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-232921@2x.png)

## 15.2 docker 安装界面

```
Name: Navigation-Webstack-laravel
Overview: https://github.com/hui-ho/WebStack-Laravel，https://github.com/Gourds/WebStackLaravel
Repository: arvon2014/webstack-laravel:v1.2.2
Docker Hub URL: https://hub.docker.com/r/arvon2014/webstack-laravel
Icon URL: https://cdn.jsdelivr.net/gh/ZhaoUncle/Unraid@main/templates/images/Navigation-Hugo-WebStack.png
WebUI: http://[IP]:[PORT:8000]
Post Arguments: /entrypoint.sh server

INSTALL_DIR: /mnt/user/appdata/Navigation-Webstack-laravel  ### 容器内的部署目录
Container Path: /opt/webstack-laravel

DB_HOST: 192.168.31.130	### mysql 地址，这里其实就是 unraid 的 ip 地址
Container Variable: DB_HOST

DB_PORT: 3307	### mysql 端口，参考“安装前准备-安装 mysql”里面你自己的映射
Container Variable: DB_PORT

DB_DATABASE: webstack_laravel	### mysql 数据库名称
Container Variable: DB_DATABASE

DB_USERNAME: root	### mysql root 账号
Container Variable: DB_USERNAME

DB_PASSWORD: my-secret-pw	### mysql root 密码
Container Variable: DB_PASSWORD

LOGIN_COPTCHA: false	### 是否启动控制台验证码，默认true
Container Variable: LOGIN_COPTCHA

Port: 28887	### web 页面访问端口
Container Port: 8000
```

![WX20220109-203442@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-203442@2x.png)

![iShot2022-01-09 20.59.06](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/iShot2022-01-09%2020.59.06.png)

## 15.3 后台访问地址

访问地址其实就是 webui 的地址后面加上/admin 就好了，默认账号是 admin，默认密码也是 admin

http://192.168.31.130:28887/admin/auth/login

![WX20220109-210458@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-210458@2x.png)

![WX20220109-210315@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-210315@2x.png)

## 15.4 如何更改 admin 的密码

![WX20220109-210709@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-210709@2x.png)

## 15.5 使用体验

不推荐，该项目已关闭，不更新了。

个人习惯问题，我觉得不如几个 hugo 的体验好，好处就是后台，不过不能用户登录访问不同分组，而且左侧的返回页顶的按钮没有跟随页面，直接放在页脚了，而且没有搜索栏，

![WX20220109-211423@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-211423@2x.png)

# 十六、Unraid 安装 Hugo Themes WebStack

地址：https://github.com/shenweiyan/WebStack-Hugo

![WX20220109-113738@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-113738@2x.png)

📢：简单点可以使用命令安装，不简单的就把下载好的 zip 包👉🏻[点击即可下载](https://github.com.cnpmjs.org/shenweiyan/WebStack-Hugo/archive/refs/heads/main.zip)👈🏻，借用 unraid 的 share 共享或者 Filebrowser 放到你的 unraid 本地目录，挂载镜像的时候加进去就好了

## 16.1 命令行方式加载Themes

参考：https://www.yuque.com/shenweiyan/cookbook/webstack-hugo

```
cd /mnt/user/appdata
mkdir -p Navigation-Hugo-WebStack/themes
cd Navigation-Hugo-WebStack
git clone https://github.com.cnpmjs.org/shenweiyan/WebStack-Hugo.git themes/WebStack-Hugo
cp -r themes/WebStack-Hugo/exampleSite/* ./
```

## 16.2 Filebrowser 安装方式

请参考 **安装前准备**——**安装 Filebrowser 导入文件，省去命令行操作**

## 16.3 docker 安装界面

```
Name: Navigation-Hugo-WebStack
Overview: https://github.com/shenweiyan/WebStack-Hugo；https://github.com/klakegg/actions-hugo
Repository: klakegg/hugo:0.83.1-alpine
Docker Hub URL: https://hub.docker.com/r/klakegg/hugo
Icon URL: https://cdn.jsdelivr.net/gh/ZhaoUncle/Unraid@main/templates/images/Navigation-Hugo-Webstack.png
WebUI: http://[IP]:[PORT:1313]
Post Arguments: server

Path: /mnt/user/appdata/Navigation-Hugo-WebStack    ###模板页面挂载本地
Container Path: /src

Port: 28889    ### web 页面访问端口
Container Port: 1313
```

![WX20220109-171535@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-171535@2x.png)

![iShot2022-01-09 17.16.01](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/iShot2022-01-09%2017.16.01.png)

## 16.4 配置介绍

![WX20220109-154314@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-154314@2x.png)

## 16.5 问题

导航页面的"配置"接口是无用的，老老实实去 hugo 的配置文件进行修改吧

![WX20220109-150331@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-150331@2x.png)

## 16.6 类似站对比，直接看 gif 图

体验都差不多

[shenweiyan/webstack-hugo](https://github.com/shenweiyan/webstack-hugo)

![QQ20220109-151503](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/QQ20220109-151503.gif)

[iplaycode/webstack-hugo](https://github.com/iplaycode/webstack-hugo)

![preview](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/preview.gif)

# 十七、Unraid 安装 Hugo Themes Slate

地址：https://themes.gohugo.io/themes/slate/

![WX20220109-113834@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220109-113834@2x.png)

📢：简单点可以使用命令安装，不简单的就把下载好的 zip 包👉🏻[点击即可下载](https://github.com.cnpmjs.org/gesquive/slate/archive/refs/heads/master.zip)👈🏻，借用 unraid 的 share 共享或者 Filebrowser 放到你的 unraid 本地目录，挂载镜像的时候加进去就好了

## 17.1命令行方式加载Themes

```
cd /mnt/user/appdata
mkdir -p Navigation-Hugo-Slate/themes
cd Navigation-Hugo-Slate
git clone https://github.com.cnpmjs.org/gesquive/slate.git themes/slate
cp -r themes/slate/exampleSite/* ./
```

![WX20220108-165617@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220108-165617@2x.png)

## 17.2 Filebrowser 安装方式

请参考 **安装前准备**——**安装 Filebrowser 导入文件，省去命令行操作**

## 17.3 docker 安装界面

```
Name: Navigation-Hugo-Slate
Overview: https://github.com/shenweiyan/WebStack-Hugo，https://github.com/klakegg/actions-hugo
Repository: klakegg/hugo:0.83.1-alpine
Docker Hub URL: https://hub.docker.com/r/klakegg/hugo
WebUI: http://[IP]:[PORT:1313]
Post Arguments: server ### 启动命令，非 nginx 静态模式

Path: /mnt/user/appdata/Navigation-Hugo-Slate	###模板页面挂载本地
Container Path: /src

Port1: 28880	### web 页面访问端口
Container Port: 1313
```

![iShot2022-01-08 21.56.32](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/iShot2022-01-08%2021.56.32.png)

## 17.4 配置文件和目录介绍：

```
Navigation-Hugo-Slate/
├── config.toml		### 页面展示相关的配置文件，主要配置导航目录
├── content
├── data
│   └── links.yml	### 相关页面配置路径，具体地址在这里配置
├── resources
├── static
│   └── logos			### 图标存放路径
└── themes				### hugo 主题目录
    └── slate
```

![WX20220108-165900@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220108-165900@2x.png)

![WX20220108-165233@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220108-165233@2x.png)

## 17.5 配置简介

### 17.5.1 举个例子，页面左侧的导航栏的导航链接🔗简称 nav 🏷是 favorites

![WX20220108-172738@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220108-172738@2x.png)

### 17.5.2 config.toml 的 params.nav 的配置就是👇🏻

```
[[ params.nav ]]		### 最左边的导航链接
name = "favorites"	### 鼠标停放后显示的名称
tag = "favorite"		### ⭐️表示👈🏻导航栏的导航链接的 tags 给 links.yml文件引用
icon = "star"				### ⭐️表示 fontawesome 的 svg 图标名称，手动更改为其他的svg 名称
```

![WX20220108-173142@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220108-173142@2x.png)

### 17.5.3 links.yml 里面你想把那些 url 添加到对应的导航栏，就要在 tags 标签里协商 config.toml 里面对应的配置

![WX20220108-175203@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220108-175203@2x.png)

### 17.5.4 如何更改 config.toml 里面 icon

打开 [fontawesome.com](https://fontawesome.com/v5.15/icons?d=gallery&p=2&q=star&m=free)，选择免费的标签，然后搜索🔍你想要的svg 图标就好了，比如 favorites 用来“star”的 icon，我给改成 “star-and-crescent”；

![WX20220108-175722@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220108-175722@2x.png)

## 17.6 使用过程中发现的问题：

- 背景尺寸不能自动 auto
- 字体会被截断
- config.toml 里面的导航栏图标加载会不正确，比如我把favorites的 icon 图标 “star”改成“star-and-crescent”，他不显示;

# 十八、浏览器端 iTab

地址：https://www.itab.link/

## 18.1 chrome 浏览器直接在应用商店就可以安装了，还支持 Edge、Firefox、360 极速

![WX20220108-220356@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220108-220356@2x.png)

## 18.2 作者自己介绍的优点哟，可以瞅瞅

- 简洁优美无广告
  - 无垃圾，无广告，陪伴你的是简洁优雅，是高效省心，让你从未如此专注
- 自定义卡片式应用
  - 天气，笔记，日历，壁纸变身桌面卡片，让信息所见即所得
- 自定义网站图标
  - 桌面网站图标可多可少，可大可小，任意布局，无拘无束，让桌面从未如此自由
- 内置壁纸应用
  - 可能是全世界最丰富的免费壁纸集合，动静结合，随意切换，让壁纸从未如此好看
- 快捷翻译功能
  - 无需再装翻译软件，搜索栏一键输入，翻译和搜索一石二鸟，让翻译从未如此快捷

![WX20220108-220134@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220108-220134@2x.png)

![WX20220108-222017@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220108-222017@2x.png)

![WX20220108-222030@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220108-222030@2x.png)

![WX20220108-222040@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220108-222040@2x.png)

## 18.3 自我体验的一些问题和建议

- 感觉还有很多bug 需要修复，对于一个不需要分享给别人的个人导航页来说，使用体验算是不错了
- 登录即可支持多端同步，将数据备份到作者的 server 端，大家自己衡量数据安全的得失了
- 唯一的问题就是不能打通各个浏览器的书签栏，如果可以的话，无缝对接，那就很厉害了
- 目前只能手动备份
- 页面还是有明显的卡顿，还无法如丝般顺滑

## 18.4 FAQ 问题反馈

https://support.qq.com/products/362782/#label=show

# 十九、Bento 赏析

github: https://github.com/migueravila/Bento
demo : https://migueravila.github.io/Bento/

![header](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/header.png)
![previewbg](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/previewbg.png)
![WX20220116-000545@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220116-000545@2x.png)

# 二十、startpage 赏析

github：https://github.com/deepjyoti30/startpage
demo：无

![startpage](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/startpage.gif)

# 二十一、homepage 赏析

github：https://github.com/Jaredk3nt/homepage
demo：无

![68747470733a2f2f692e726564642e69742f63626e7a7133367a6a333630312e676966](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/68747470733a2f2f692e726564642e69742f63626e7a7133367a6a333630312e676966.gif)

# 二十二、tilde 赏析

github：https://github.com/cadejscroggins/tilde
demo：https://tilde.cade.me/

![WX20220116-001611@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220116-001611@2x.png)

# 二十三、Prismatic-Night 赏析

github：https://github.com/3r3bu5x9/Prismatic-Night

![all](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/all.png)
![ctrl_L](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/ctrl_L.gif)
![engineToggle](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/engineToggle.gif)
![hover](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/hover.gif)
![zathura](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/zathura.png)

# 二十四、pomme-page 赏析

github：https://github.com/kikiklang/pomme-page
demo：无

![screenshotpomme](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/screenshotpomme.png)
![screenshotpomme2](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/screenshotpomme2.png)

# 二十五、fluidity 赏析

github：https://github.com/PrettyCoffee/fluidity

![DarkSouls-theme](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/DarkSouls-theme.png)
![default-theme](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/default-theme.png)
![pop!os-theme](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/pop!os-theme.png)

# 二十六、startup-page 赏析

github：https://github.com/timothypholmes/startup-page

![preview](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/preview.png)

# 二十七、startpages 赏析

github：https://github.com/grtcdr/startpages

![preview (1)](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/preview111.png)

![preview2](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/preview222.png)

![preview3](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/preview333.png)

![preview4](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/preview444.png)

![preview5](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/preview555.png)

![preview6](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/preview666.png)

![preview](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/preview.webp)

# 二十八、the-glorious-startpage 赏析

github: https://github.com/manilarome/the-glorious-startpage/
demo: https://manilarome.github.io/the-glorious-startpage/

![the-gloriousautosuggestion](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/the-gloriousautosuggestion.png)
![the-gloriousidle](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/the-gloriousidle.png)
![the-glorioussettings](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/the-glorioussettings.png)
![the-gloriousweather](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/the-gloriousweather.png)
![the-gloriouswebmenu](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/the-gloriouswebmenu.png)

# 二十九、flare

github：https://github.com/soulteary/docker-flare

![](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20220119-163954@2x.png)

![](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/ui.png)

![](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/lighthouse.png)

