# Unraid安装群晖DS918+7测试硬解和人脸识别




<!--more-->



# 一、环境：  

--------

以下环境只在于体验群辉系统带来的体验感，在保持口袋经济稳定的情况下，我们还是应该购买正规的群晖NAS[服务器](https://www.smzdm.com/fenlei/fuwuqi/)，体验和学习可以用黑群晖系统安装虚拟机来作为体验，有能力还是要买台白裙做冷备。

*   unraid：6.9.2

*   群辉系统：DS918+\_7.0.1-42218

*   cpu：i9-10900t es 测试不显版 QTB0  

*   主板：B460M Steel Legend 钢铁[传奇](https://pinpai.smzdm.com/117439/)

二、安装群辉虚拟系统  

-------------

### 2.1 创建 VMS 虚拟机

![WX20211220-222436@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211220-222436@2x.png)

![WX20211220-222512@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211220-222512@2x.png)



### 2.2 配置 DSM 群辉虚拟机的选项


![WX20211220-223514@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211220-223514@2x.png)

### 2.3 因为网卡驱动集成了virtio，所以现在不需要改成 e1000 都可以正常使用了

![WX20211220-223547@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211220-223547@2x.png)

![WX20211220-223724@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211220-223724@2x.png)



### 2.4 硬盘直通

> 其实就是把在“SHARES”里面，我们没有把硬盘给 UNRAID 组阵列的名称复制出来，加上“/dev/disk/by-id/ata-”前缀，整个路径分配给群辉虚拟机就好了，是不是很简单捏，不过前提是你要安装“Unassigned Devices”这个插件，才会显示未被Unraid使用和格式化的硬盘、usb、虚拟硬盘==；  

![WX20211220-222728@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211220-222728@2x.png)

![WX20211220-222849@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211220-222849@2x.png)



### 2.5 然后就是一步一步的创建了，开机启动，很多步骤都卡在了 55%，原因会有很多，这里略过。  

![WX20211221-100258](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211221-100258.png)

![WX20211220-223803@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211220-223803@2x.png)



### 2.6 待到安装完成，基[本上](https://pinpai.smzdm.com/285004/)按照正常的群辉开启动配置一步一步下去就好了，不过需要注意下几个略过的地方

![WX20211220-231433@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211220-231433@2x.png)




![WX20211220-231454@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211220-231454@2x.png)

![WX20211220-231511@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211220-231511@2x.png)



![WX20211220-231525@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211220-231525@2x.png)

![WX20211220-231537@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211220-231537@2x.png)



![WX20211220-231610@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211220-231610@2x.png)



![WX20211220-231619@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211220-231619@2x.png)



![WX20211220-231631@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211220-231631@2x.png)



![WX20211220-231644@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211220-231644@2x.png)



### 2.7 安装 Synology Photos

> 这个时候我们上传图片并开启“人物相册”，也就是人脸识别空间，会发现，一直开在等待上传中，这里就表示，人脸识别失败了，需要用一些非常规手段去实现“人脸AI”功能。

![](https://qnam.smzdm.com/202112/21/61c12079476a4228.png_e1080.jpg)

![WX20211220-232043@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211220-232043@2x.png)

![WX20211220-232056@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211220-232056@2x.png)](https://post.smzdm.com/p/a5dl2808/pic_22/)



### 2.8 安装 Video Station

> 这里硬解测试是失败的，无法进行硬解转码，需要半洗白，同时把核显驱动补上。  

![WX20211221-091152](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211221-091152.png)

![WX20211221-092118](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211221-092118.png)



![WX20211221-092219](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211221-092219.png)


### 2.9 群辉开启ssh登录，并设置root账号密码

> #这里输入的是你安装群辉系统的第一个创建的用户的密码，我创建的是unclezhao，所以输入他的密码回车就行了
>
> sudo su -  
>
> synouser --setpw root qwe@1234

![WX20211220-231813@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211220-231813@2x.png)

![WX20211221-091608](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211221-091608.png)



![WX20211221-091732](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211221-091732.png)



### 2.10 修改sshd 配置，然后重启sshd服务，无需重启群辉虚拟机

![WX20211221-091636](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211221-091636.png)

![WX20211221-091645](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211221-091645.png)](https://post.smzdm.com/p/a5dl2808/pic_30/)



三、Unraid 开启核显虚拟化给群辉
-------------------

### 3.0 主板开启 gpu 共享内存为 1024M，其他测试发现都有问题，无法开启核显虚拟化直通；  

![IMG_20211215_005049](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/IMG_20211215_005049.jpeg)


### 3.1 安装 Intel GVT-g 插件，然后重启 Unraid；

![WX20211220-213424@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211220-213424@2x.png)



![WX20211220-213630@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211220-213630@2x.png)


### 3.2 将虚拟 GPU 核显导入群辉虚拟系统使用；

![WX20211220-214526@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211220-214526@2x.png)

![WX20211220-232244@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211220-232244@2x.png)](https://post.smzdm.com/p/a5dl2808/pic_35/)



### 3.3 因为 gpu 的位置问题，需要修改他的总线位置改成红框内的样子“bus=‘0x00’ slot=‘0x02’”，否则会导致核显识别有问题；

![WX20211220-232416@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211220-232416@2x.png)

![WX20211220-232629@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211220-232629@2x.png)](https://post.smzdm.com/p/a5dl2808/pic_37/)



### 3.4 然后删除位置重叠的这段代码（红框内），也可以单独删除“bus="0x00 slot= 0x02”那4行，以下二选一删除即可

![WX20211221-095854](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211221-095854.png)



![WX20211221-090857](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211221-090857.png)



### 3.5 用 winscp 导入核显驱动，重启群辉虚拟机

![WX20211221-095555](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211221-095555.png)



### 3.6 ssh 进入查看一下总线位置和核显是否启动  

![WX20211221-092402](/Users/aomine/Movies/obs/nodone/WX20211221-092402.png)

![WX20211221-093614](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211221-093614.png)



### 3.7 然后再打开Viedo Station ，测试是否能够转码即可  

![WX20211221-175148](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211221-175148.png)



四、群晖 DS918+7 硬刚 CPU 开启 Photos 人脸识别
----------------------------------

> 群辉选择 DS918+7.0.1 硬解人脸识别，使用 gpu 直通或者 Intel VTG-g 核显虚拟化直通后，还是无法人脸识别的，还是老老实实走 CPU 人脸识别吧，起码还是能用，硬解（其实在这里是个伪命题，因为 unraid 可以通过 docker 安装 video 服务来完成硬解， DSM 虚拟机可以不用到），不过不能不折腾。
>
> 经过研究，Synology Photos应该使用了OpenCv的DNN神经网络来识别人脸。这个OpenCv库可根据群晖的型号调用不同的神经网络模型，并调用显卡的GPU来加速计算。如果硬件和库代码不匹配，那就无法人脸识别了。所以人脸识别主要是显卡的GPU调用的问题，跟洗白没关系。技术上可以打个补丁解决。最低要求是 intel 六代或者以上的 CPU。黑群晖 DS918-7.0.1 的 Synology Photos 套件人脸识别，与你的系统是否已经洗白并无直接关系。

### 4.1 先确认一下 Synology Photos 的版本必须是 1.1.0-0224

### 4.2 先停用 Synology Photos

![WX20211220-232127@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211220-232127@2x.png)

](https://post.smzdm.com/p/a5dl2808/pic_45/)

### 4.3 ssh 登录群辉虚拟机，然后执行以下命令  

> mv /var/packages/SynologyPhotos/target/usr/lib/libsynophoto-plugin-model.so.1.0 /var/packages/SynologyPhotos/target/usr/lib/libsynophoto-plugin-model.so.1.0.bak
>
> wget https://cdn.jsdelivr.net/gh/jinlife/Synology\_Photos\_Face\_Patch@main/libsynophoto-plugin-model.so -O /var/packages/SynologyPhotos/target/usr/lib/libsynophoto-plugin-model.so.1.0
>
> chmod 755 /var/packages/SynologyPhotos/target/usr/lib/libsynophoto-plugin-model.so.1.0
>
> chown SynologyPhotos:SynologyPhotos /var/packages/SynologyPhotos/target/usr/lib/libsynophoto-plugin-model.so.1.0

### 4.4 重新启动 Synology Photos，并打开人脸识别，就正常了。

五、总结
----

### 5.1 群辉 DS918+\_7.0.1 目前人脸识别不正常的话，那只能通过CPU进行人脸AI识别功能，如果想要GPU的话，还是老实用6.2.3的版本吧，另外DS3615-7.0 因为用的是cpu进行人脸识别，所以如果没有硬解视频的需求，直接用DS3615就好了，也就省的折腾（时间成本也是白花花的大银子，不说了都是泪）

### 5.2 DS918+\_7.0.1 CPU 驱动，是jinlife大佬提出来的，具体可以看一下链接

[https://github.com/jinlife/Synology\_Photos\_Face\_Patch](https://github.com/jinlife/Synology_Photos_Face_Patch/)

[https://blog.jinlife.com/index.php/archives/49/](https://blog.jinlife.com/index.php/archives/49/)

https://xpenology.com/forum/topic/45795-redpill-the-new-loader-for-624-discussion/page/123/?tab=comments#comment-226764

### 5.3 DS918+7.0以上成功的案例具体可以查看👇🏻两个网站可以的统计信息和网友的评论。

[https://www.openos.org/threads/redpill7-0cpu.3647/](https://www.openos.org/threads/redpill7-0cpu.3647/)

[https://wp.gxnas.com/11458.html](https://wp.gxnas.com/11458.html/)

### 5.3.1 核显直通所需要的驱动包和 cpu 是否支持硬解的说明，可以参考人生观大佬的文章；

https://wp.gxnas.com/7952.html

### 5.4 以上步骤都尝试之后，如果还不能人脸识别，可能有以下几个尝试，不一定有用，也可能是我瞎尝试，乱讲，且看且忘记

*   上传 photos 的图片太少，可以把同一张图片在复制一次，上传测试看看，我试过好几次，这样就能识别出照片了，都是临时测试网[上下](https://pinpai.smzdm.com/31199/)载的；  

*   上传的图片名称有特殊符号，比如括号，我试过图片名称为 image(1)、images(2)的图片都无法识别，后面改了名称重新上传，就有了；

*   群辉虚拟机分配 4 vcpu，内存超 4 g；

### 5.5 Unraid安装群辉虚拟系统名称不能用特殊符号，否则会导致 Intel GVT-g 插件导入虚拟化核显失败，不会显示那一段核显直通的代码

![WX20211221-090806](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211221-090806.png)



### 5.6 如果你之前手动在 Unraid 开启过核显驱动，并且在启动文件里面写好了，需要删掉或注释掉

![WX20211220-213825@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211220-213825@2x.png)



### 5.7 Intel-GVT-g 插件支持的 CPU 型号

![WX20211220-214024@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211220-214024@2x.png)



### 5.8 为什么GPU核显要在 0000.00.02.0 的位置么，因为虚拟机的核显ID位置和物理机的核显ID位置不[一致](https://pinpai.smzdm.com/102375/)，其实最简单的做法就是用 lscpi 看看白裙的核显位置就是了，另外一个就是Intel GVT-g 插件的位置也有显示。


![WX20211221-092402](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211221-092402.png)
lscpi显示pci设备

![WX20211220-232323@2x](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211220-232323@2x.png)
Intel GVT-g 显示 Address 位置

### 5.9 Intel-GVT-g 使用过程中因为多次导入gpu到虚拟机，然后因为使用过程中没有正常停机从插件删除核显配置，就把DSM虚拟机删了，后面在重新到插件里面导入gpu，就会报错gpu不够用了，因为被分配到之前的机器上没有释放掉，解决办法也简单，先删除掉插件之后再重启Unraid，重新安装配置就好了

![WX20211221-091017](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211221-091017.png)



![WX20211221-093955](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211221-093955.png)

](https://post.smzdm.com/p/a5dl2808/pic_52/)Unraid系统日志报错

![WX20211221-093801](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211221-093801.png)



### 5.10 正常的步骤如下，先停止DSM群辉虚拟机，然后从插件删除


![WX20211221-180839](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211221-180839.png)

### 5.11 为何要把 cpu 人脸识别驱动导入到某个位置，因为另外两个都是软连接到 libsynophoto-plugin-model.so.1.0，就像是win的快捷方式一样，都需要修改最终的boss才能生效。

![WX20211221-091814](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/WX20211221-091814.png)



### 5.12 命令行重建索引方式

synoindex -R /var/services/homes/你的用户名/Photos  

### 5.13 如果有些驱动没集成，你也可以自己编译，参考如下

h[ttps://github.com/RedPill-TTG/redpill-load](https://github.com/RedPill-TTG/redpill-load/)

[https://github.com/RedPill-TTG/redpill-lkm](https://github.com/RedPill-TTG/redpill-lkm/)

作者声明本文无利益相关，欢迎值友理性交流，和谐讨论～

![](https://res.smzdm.com/pc/pc_shequ/dist/img/the-end.png)

