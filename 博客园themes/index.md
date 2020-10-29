# 博客园主题


<!--more-->

**记得点“推荐”或者“关注”哦！！！**

# 1. 主题

[Cnblogs-Theme-SimpleMemory 1.3.3 版本](https://github.com/BNDong/Cnblogs-Theme-SimpleMemory/tree/v1.3.3)

# 2. 权限申请

先在博客园申请开通博客，这里就不做介绍了，然后在 **“博客侧边栏公告”** [申请 js 权限](https://i.cnblogs.com/settings)（博客侧边栏公告）

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/20200928084358.png" width="600" hegiht="250" align=center/>



<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/20200928090420-20200928090440540.png" width="600" hegiht="250" align=center/>





# 3. [博客园后台](https://i.cnblogs.com/settings)设置

## 3.1 用户设置

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928101839678.png" width="600" hegiht="250" align=center/>



**参数：**

>1. 标题：主要影响后台的显示；
>
>2. 博客地址名：https://www.cnblog.com/博客地址名/，这个只会修改只能通过邮件申请，我申请过一次，申请完之后用了 js 的话，还会用旧的链接地址访问，需要把 css 和 js 的代码清掉保存，再重新填写再保存，才能快速清除；
>
>3. 登录用户名：这个影响不大，因为下面有个显示名称，就是替换这个的
>
>4. 密码：忽略
>
>5. 显示名称（作者名）：这里会替换掉登录用户名的显示规则，会影响浏览器标签那里的 title，还有显示作者的地方；
>
>6. 邮箱：评论那里可以选择发送邮箱
>
>7. 回复通知邮箱：默认楼上那个
>
>8. 时区：utc+8 即可，北京时间
>
>9. 语言与地区：简体中文



## 3.2 模板设置



<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928102152457.png" width="600" hegiht="250" align=center/>



**参数：**

> 1. 博客皮肤：选择SimpleMemory
>
> 2. 页面定制 CSS 代码：
>
>    2.1 CSS代码位置：[/src/style/base.min.css](https://cdn.jsdelivr.net/gh/BNDong/Cnblogs-Theme-SimpleMemory@v1.3.3/src/style/base.min.css) ，拷贝此文件代码至页面定制CSS代码文本框处即可。
>
>    2.2 /src/style/base.min.css 的修改参考 /src/style/base.css。博客设置请使用压缩版本，直接使用 /src/style/base.css 会字符超限！
>
> 3. 禁用模板默认 CSS：**这里要记得 ![image-20200928112416329](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928112416329.png)**
>
> 4. 博客侧边栏公告：
>
>    4.1 JavaScript 对象是被命名值的容器。值以名称:值对的方式来书写（名称和值由冒号分隔）。

```js
<script type="text/javascript">
    window.cnblogsConfig = {
        GhVersions    : 'v1.3.3',  //这里的版本要与最下面的 scripts 引用的 js 版本一致
        blogUser      : "欲戴王冠，必承其重...易波叶平",  //用户名，主要是首页打开的为孩子
        blogAvatar    : "https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/avatar.png", //用户头像
        blogStartDate : "2017-11-17",  // 入园时间，年-月-日。入园时间查看方法：鼠标停留园龄时间上，会显示入园时间
        webpageTitleOnblur: '(oﾟvﾟ)ノ Hi', //离开页面，title 显示的文字
        webpageTitleFocus: '(*´∇｀*) 欢迎回来！',//点击页面，title 显示的文字
        webpageIcon: "https://cdn.jsdelivr.net/gh/BNDong/Cnblogs-Theme-SimpleMemory@master/img/webp/blog_logo.webp", //网站图标
        fontIconExtend: "//at.alicdn.com/t/font_2104671_57vkche3kz2.css",//字体图标配置，开个小章节单独讲
        menuNavList: [ // 菜单导航 ['导航名称', '链接', 'icon']
             ['Blog', 'https://zhaouncle.com/', 'icon-blog-solid'],
             ['CMS', 'https://link.zhaouncle.com/', 'icon-logo1'],
             ['k8sYaml 生成器', 'https://k8syaml.com/', 'iconkubernetes1'],//这里的iconkubernetes1就是用的我自己的图标库来配置的，默认不自带
        ],
        //homeBannerText: "好好学习，天天向上！",我这里不设置，用的主题的自动获取，默认获取诗词
        //homeBannerTextType: "jinrishici", //jinrishici：每次刷新随机获取一句古诗词;one：每日获取一句话
        homeTopImg: [ //首页轮播图，可以设置多个进行轮播
            "https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/home_top_bg.webp",
            "https://cdn.jsdelivr.net/gh/BNDong/Cnblogs-Theme-SimpleMemory@master/img/webp/home_top_bg.webp"
        ],
        essayTopImg: [//文章页面最上面那个图片，每次刷新随机选择
            "https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/nothome_top_bg.webp",
            "https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/wallhaven-4o5zm9.webp"
        ],
        menuUserInfoBgImg: 'https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/menu_bg.gif',//菜单栏个人信息背景图
        codeLineNumber: true, //代码行号渲染
        essayCodeHighlightingType: "prettify",//代码渲染高亮
        essayCodeHighlighting: "obsidian", // 代码渲染主题
        footerStyle: 2,// 页脚样式，默认有 1、2 两种
        bottomText: { //页脚标语
            left : "长风破浪会有时",
            right: "直挂云帆济沧海",
        },
        reward: { //文章打赏按钮，显示在页面右下角。
            enable: true,
            wechatpay: 'https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/WechatPay.png',//我这里设置的赞赏码
            alipay: 'https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/Alipay.png',//支付宝付款码
        },
        bottomBlogroll: [ // 友情链接，[[链接名,链接]....]
            ["易波叶平", 'https://zhaouncle.com/'],
        ],
        consoleList: [ //浏览器 console 输出
           ['Zhaouncle CNBlogs', 'https://www.cnblogs.com/UncleZhao/'],
        ],
        advertising: true, //博客园广告
    }
</script>
<script src="https://cdn.jsdelivr.net/gh/BNDong/Cnblogs-Theme-SimpleMemory@v1.3.3/src/script/simpleMemory.min.js" defer></script>
```



## 3.3 开启公告控件

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928133928766.png" width="600" hegiht="250" align=center/>



公告这里要选上![image-20200928112416329](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928112416329.png)，否则部分 js 文件会无法生效，其他的就自行参考自己的需要了。

到了这一步，基本上一个完整的博客主题就搭建完成了，保存即可自由玩耍写博客了。



# 4. 功能扩展

## 4.1 菜单栏图标

### 4.1.1 查看菜单栏

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928162846066.png" width="200" hegiht="250" align=center/>



[字体图标库](https://bndong.github.io/Cnblogs-Theme-SimpleMemory/v1.1/#/Docs/Customization/fonticon) 列举了部分内置的图标，但是还有一些需要自定义，比如 “k8sYmal 生成器” 这个图标就没有内置，需要我们自己去 iconfont ，阿里的图标库自定义。

### 4.1.2 登录 [iconfont](https://www.iconfont.cn/)

我一开始选择“Github”登录的方式，但是直接报错了，所以我只能选择“新浪微博”的登录方式，主要是我不玩微博，我觉得很蛋疼

### 4.1.3 创建项目

我们先进到 “资源管理” -- “我发起的项目” ，创建一个 “博客园” 项目，如图所示

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928163537767.png" width="600" hegiht="250" align=center/>



<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928163721514.png" width="600" hegiht="250" align=center/>





### 4.1.4 生成代码

然后生成代码，填入 3.2 的 “博客侧边栏公告” 的 js 对象 **fontIconExtend** 更换成你自己的就行了，这个要添加图标进去后才能生成。

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928163806514.png" alt="image-20200928163806514" style="zoom:67%;" />



### 4.1.5 图标加入购物车

你可以直接搜索 “kubernetes”，也可以直接一个一个搜，不过一般不会这么傻吧？哈哈。重要的是选中图标，把鼠标移到图标上面，然后添加进购物车

![image-20200928164246239](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928164246239.png)



### 4.1.6 从购物车添加至项目

点击购物车图标，然后 “添加至项目” 即可，当然你也可以忽略前面创建项目的步骤，直接在这里的![image-20200928164444244](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928164444244.png)选中添加项目也行，随你自己喜欢了。

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928164401982.png" width="400" hegiht="250" align=center alt="image-20200928164401982"/>

### 4.1.7 添加成功

点击确定，就添加成功了，然后在“生成代码”就好啦，鼠标移动到图标上面，直接复制代码，

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928164637400.png" alt="image-20200928164637400" style="zoom: 25%;" />

### 4.1.8 配置 js 对象

然后直接生成代码，香气十足，再配置到 3.2 的 js 对象 “menuNavList”，完美添加图标了，当然你也可以自己制作图标然后上传使用。

        menuNavList: [ // 菜单导航 ['导航名称', '链接', 'icon']
             ['k8sYaml 生成器', 'https://k8syaml.com/', 'iconkubernetes1'],//这里的iconkubernetes1就是用的我自己的图标库来配置的，默认不自带
        ],
## 4.2 鼠标点击特效

### 4.2.1 鼠标特效

#### 4.2.1.1 烟花特效，我自己在使用的

![image-20200928164637400](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/博客园鼠标特效%201.gif)

#### 4.2.1.2 文字特效

![image-20200928164637400](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/%E5%8D%9A%E5%AE%A2%E5%9B%AD%E9%BC%A0%E6%A0%87%E7%89%B9%E6%95%88%202-20200928182225432.gif)

### 4.2.2 添加鼠标图像

在博客园的设置那里，我们在  “页面定制 CSS 代码” 的“body” 代码块添加如下几句代码，这几句代码主要添加鼠标的展示图，至于点击效应在下面的 js 代码块里面。如下图所示

注意：至于如何快速找到 body，要么你直接浏览器 F12 搜，要么把 css 代码 copy 出来咯，改完再放进去不就好了吗

```css
background-repeat: repeat;background-attachment: fixed;background-size:cover;cursor: url(https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/cursor.ico),auto;
```

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928170447277.png" alt="image-20200928170447277" style="zoom: 33%;" />

### 4.2.3 添加 js 代码

> 注意：特效只需要添加一种即可

#### 4.2.3.1 烟花特效

在 **“页首 HTML 代码” **那里添加以下代码

```js
<!--鼠标点击特效，烟花效应-->
<script src="https://cdn.jsdelivr.net/gh/ZhaoUncle/images/static/mouse-click.js"></script>
<canvas width="1777" height="841" style="position: fixed; left: 0px; top: 0px; z-index: 2147483647; pointer-events: none;"></canvas>
```



<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928170753274.png" alt="image-20200928170753274" style="zoom:50%;" />

#### 4.2.3.2 文字特效

在 **“页首 HTML 代码” **那里添加以下代码

```
<script type="text/javascript">
/* 鼠标特效，文字特效 */
var a_idx = 0;
jQuery(document).ready(function($) {
    $("body").click(function(e) {
        var a = new Array("❤富强❤","❤民主❤","❤文明❤","❤和谐❤","❤自由❤","❤平等❤","❤公正❤","❤法治❤","❤爱国❤","❤敬业❤","❤诚信❤","❤友善❤");
        var $i = $("<span></span>").text(a[a_idx]);
        a_idx = (a_idx + 1) % a.length;
        var x = e.pageX,
        y = e.pageY;
        $i.css({
            "z-index": 999999999999999999999999999999999999999999999999999999999999999999999,
            "top": y - 20,
            "left": x,
            "position": "absolute",
            "font-weight": "bold",
            "color": "rgb("+~~(255*Math.random())+","+~~(255*Math.random())+","+~~(255*Math.random())+")"
        });
        $("body").append($i);
        $i.animate({
            "top": y - 180,
            "opacity": 0
        },
        1500,
        function() {
            $i.remove();
        });
    });
});
</script>
```



### 4.2.4 结束语

如上就能完成鼠标的特效了，当然特效也不止这一种，剩下的等以后有兴趣再试了。

## 4.3 网站统计

> CNZZ 也就是友盟的网站统计功能，主题整合 CNZZ 网站统计，并对样式进行了优化。如果需要本功能，请首先去 CNZZ 配置网站的统计，然后修改下面的代码，添加至页脚 Html 代码中。

![image-20200928174817556](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928174817556.png)

### 4.3.1 配置 CNZZ

网址：[U-Web](https://web.umeng.com/main.php?c=site&a=show)

![image-20200928173853524](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928173853524.png)

### 4.3.2 添加站点

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928174154923.png" alt="image-20200928174154923" style="zoom:33%;" />

### 4.3.3 获取统计代码 ID

![image-20200928174331794](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928174331794.png)

### 4.3.4 开启详细数据

![image-20200928174425996](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928174425996.png)

### 4.3.4 配置页脚 HTML 代码

进入博客园后台设置，配置**页脚 HTML 代码**，将 4.3.3 的统计 ID 替换，然后 copy 进去

```js
<!--友盟访问统计-->
<div id="cnzzProtocol"  style="display: none;">
    <span class="id_cnzz_stat_icon" id='cnzz_stat_icon_你的统计ID'></span>
    <script src='https://v1.cnzz.com/z_stat.php?id=你的统计ID&online=1&show=line' type='text/javascript'></script>
</div>
```

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928174738903.png" alt="image-20200928174738903" style="zoom: 67%;" />



## 4.4 天气预报

### 4.4.1 心知天气

[注册登录](https://www.seniverse.com/signup?callback=%2Findex)，这就我无需多说啦！

### 4.4.2 天气插件

[Weather Widget](https://www.seniverse.com/widget)，我们这里直接点击 “**立即免费试用**” 就可以啦。

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928232605509.png" alt="image-20200928232605509" style="zoom: 33%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/20200928232224.png" style="zoom: 33%;" />



### 4.4.3 天气插件配置

#### 4.4.3.1 自定义配置

> 自动定位：这里不知为何我电脑定位到“福州”，所以我干脆关了算了，可能是免费版导致的。

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928233502331.png" alt="image-20200928233502331" style="zoom: 25%;" />

#### 4.4.3.2 生成代码

将生成的 JS 代码复制到 “**页脚 HTML 代码**”中，放在 4.3 网站统计的下面就好。

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928233706388.png" alt="image-20200928233706388" style="zoom: 67%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928234644921.png" alt="image-20200928234644921" style="zoom:67%;" />

### 4.4.4 效果如图所示

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928235057890.png" alt="image-20200928235057890" style="zoom: 50%;" />

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928235137699.png" alt="image-20200928235137699" style="zoom:50%;" />

## 4.5 背景图片

### 4.5.1 原因 

打开一篇博文，你是否因其空荡荡的背景而觉得索然无味呢？这个时候我没就要利用自己强大的 copy 能力，给自己找张好看的背景丢上去咯

### 4.5.2 配置到 CSS 的 body 代码段

```css
background-image: url("https://img2018.cnblogs.com/blog/1358881/201909/1358881-20190910100015098-837598352.jpg");
```

如图所示：

![image-20200929091203925](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200929091203925.png)

### 4.5.1 效果图如下

![image-20200929092309224](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200929092309224.png)

## 4.6 网抑云音乐

### 4.6.0 效果图

![image-20200929103039509](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200929103039509.png)

### 4.6.1 获取网抑云 ID

打开网页版网抑云，然后选择你想播放的歌单，歌单可以是你自己的也可以是别人的，只要歌曲能播放就行，然后把 uri 后面的 id 给 copy 出来

![image-20200929100851791](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200929100851791.png)

### 4.6.2 代码添加

将一下代码添加至页脚Html代码中，更改 **歌单 ID** 为你自己的即可

```
<!-- require APlayer -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/aplayer/dist/APlayer.min.css">
<script src="https://cdn.jsdelivr.net/npm/aplayer/dist/APlayer.min.js"></script>
<!-- require MetingJS -->
<script src="https://cdn.jsdelivr.net/npm/meting@2/dist/Meting.min.js"></script>
<meting-js
        id="歌单ID"
        lrc-type="0"
        server="netease"
        order="random"
        type="playlist"
        fixed="true"
        list-olded="true">
</meting-js>
```

如图所示，代码存放位置，直接放在之前的代码后面就行

![image-20200929102832380](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200929102832380.png)

## 4.7 去掉反对按钮

### 4.7.1 何谓反对按钮

看到这个按钮，下意识就想删了，大家可不要给我反对哦，否则就不更新了呢，不过我也不给你这个按钮，哈哈！

![image-20200929134759718](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200929134759718.png)

![image-20200929134740622](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200929134740622.png)

### 4.7.2 在配置 CSS

在 **页面定制 CSS 代码 ** 里面直接在最后一行添加一下代码即可，记得保存由

```css
#rightBuryit{display: none !important;}.buryit{display: none;}
```

如图所示：

![image-20200929232353044](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200929232353044.png)

### 4.7.3 效果图

这个时候我们再看就没有啦，我是不是很鸡贼呢？

![image-20200929135145293](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200929135145293.png)

![image-20200929232045242](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200929232045242.png)

## 4.8 复制文字加版权

> 当用户复制文中内容(ctrl+C或鼠标右击复制)时，会自动加上版权文字，csdn的代码复制以及知乎的内容复制都有此功能。

### 4.8.1 添加到**页首 HTML 代码 **

```js
<!--自动复制添加版权-->
<script language="javascript" type="text/javascript">
jQuery(document).on('copy', function(e)
    {
      var selected = window.getSelection();
      var selectedText = selected.toString().replace(/\n/g, '<br>');  // Solve the line breaks conversion issue 处理换行转换的问题
      var copyFooter = '<br>---------------------<br>著作权归作者所有。<br>' 
                            + '商业转载请联系作者获得授权，非商业转载请注明出处。<br>'
                            + '作者：易波叶平<br> 源地址：' + document.location.href
                            + '<br>来源：博客园cnblogs<br>© 版权声明：本文为博主原创文章，转载请附上博文链接！';
      var copyHolder = $('<div>', {id: 'temp', html: selectedText + copyFooter, style: {position: 'absolute', left: '-99999px'}});
        
      $('body').append(copyHolder);
      selected.selectAllChildren( copyHolder[0] );
      window.setTimeout(function() {
          copyHolder.remove();
      },0);
    });
</script>
```

如图所示：直接在鼠标特效后面换行加上就好，然后及的保存

![image-20200929140933762](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200929140933762.png)

## 4.9 看板娘

### 4.9.1 效果图

![image-20200930132532566](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200930132532566.png)

### 4.9.2 所有文件

可以私聊博主要，博主没有藏私，也可以在下面的代码里面自己获取，如果合适，请给个推荐和关注，感谢博主的无私贡献，虽然博主也是从其他地方获取来着。

**[点击推荐和关注才会下载哦，不然会出错的呢 emmmm!!!](https://github.com/ZhaoUncle/images/tree/master/static/kanbanniang)**

### 4.9.3 “博客侧边栏公告 ”设置

```js
<!--看板娘-->
<div class="waifu" id="waifu">
    <div class="waifu-tips" style="opacity: 1;"></div>
    <canvas id="live2d" width="280" height="250" class="live2d"></canvas>
    <div class="waifu-tool">
      <span class="fui-home"></span>
      <span class="fui-chat"></span>
      <span class="fui-eye"></span>
      <span class="fui-user"></span>
      <span class="fui-photo"></span>
      <span class="fui-info-circle"></span>
      <span class="fui-cross"></span>
    </div> 
</div>
<script src="https://cdn.bootcss.com/jquery/3.2.1/jquery.min.js"></script> 
<script src="https://cdn.jsdelivr.net/gh/ZhaoUncle/images/static/kanbanniang/live2d.js"></script>
<script src="https://cdn.jsdelivr.net/gh/ZhaoUncle/images/static/kanbanniang/waifu-tips.js"></script>
<script type="text/javascript">initModel()</script>

```

![image-20200930141616463](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200930141616463.png)

### 4.9.4 “页首 HTML 代码”配置

```
<!--看板娘-->
<link rel="stylesheet" type="text/css" href="https://cdn.jsdelivr.net/gh/ZhaoUncle/images/static/kanbanniang/waifu.css"/>
<link rel="stylesheet" type="text/css" href="https://cdn.jsdelivr.net/gh/ZhaoUncle/images/static/kanbanniang/flat-ui.min.css"/>
```

![image-20200930141533232](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200930141533232.png)



## 4.10 更改置顶图标

### 4.10.1 效果图

原本是 iconfont 的默认图标，我这里用个小火箭给替代了。

![image-20201001153701615](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20201001153701615.png)

![image-20201001153954054](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20201001153954054.png)

### 4.10.2 配置

在**页面定制 CSS 代码**那里最后直接追加即可，记得选择保存哦！

```css
i.iconfont.icon-zhiding {background: url(https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/o_rocket.png) no-repeat center center;}

```

![image-20201001153846665](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20201001153846665.png)





## 4.11 评论区优化

### 4.11.1效果图

![image-20201002203505959](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20201002203505959.png)

### 4.11.2 配置 “页面定制 CSS 代码 ”

```
#tbCommentBody {background: #fff url(https://gitee.com/dbnuo/Cnblogs-Theme-SimpleMemory/raw/master/img/comment_bg.jpg) right -65px;background-size: 250px;background-repeat: no-repeat;}
```

![image-20201002203553522](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20201002203553522.png)



## 4.12 放大图片（low）

### 4.12.1 配置 “页面定制 CSS 代码 ”

```
.post img {cursor: pointer;transition: all 0.5s;}.post img:hover {transform: scale(1.3);}
```

## 4.13 个人书单

![image-20201014175117428](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20201014175117428.png)

### 4.13.1 创建随笔

markdown 格式直接创建一个新“随笔”，然后 markdown 文档模式添加已下数据即可

```
<input id="bookListFlg" type="hidden">
```

### 4.13.2 配置书单数据

博客侧边栏公告设置，因为数据量很大，直接在最后一行新起，就不嵌套在原有的配置里面，不过我觉得后期应该直接加到 github管理引用才方便，期待中。

```
<!--书单-->
<script type="text/javascript">
window.cnblogsConfig.bookList = [
        {
            title: '在读',
            books: [
                {
                    cover: 'https://images.weserv.nl/?url=https://wfqqreader-1252317822.image.myqcloud.com/cover/71/25066071/t6_25066071.jpg&default=ssl:images.weserv.nl/?url=https://img3.doubanio.com/view/subject/l/public/s29962521.jpg',
                    name: '断舍离',
                    formerNname: '',
                    author: '[日]山下英子',
                    translator: '贾耀平',
                    press: '湖南文艺出版社',
                    year: '2019-01|2020-10-14',
                    score: 2,
                }
            ]
        },
        {
            title: '已读',
            books: [
                {
                    cover: 'https://images.weserv.nl/?url=https://wfqqreader-1252317822.image.myqcloud.com/cover/509/413509/t6_413509.jpg&default=ssl:images.weserv.nl/?url=https://img3.doubanio.com/view/subject/l/public/s27311101.jpg',
                    name: '知行合一王阳明',
                    formerNname: '',
                    author: '度阴山',
                    translator: '',
                    press: '北京联合出版公司',
                    year: '2014-07|2020-04',
                    score: 4,
                },
            ]
        },
    ];
</script>
```

如图所在位置所示：

![image-20201014174943992](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20201014174943992.png)





# 5. 参考资料

https://github.com/BNDong/Cnblogs-Theme-SimpleMemory

https://bndong.github.io/Cnblogs-Theme-SimpleMemory/v1.1/#/

https://www.cnblogs.com/wkfvawl/p/9414180.html

https://github.com/metowolf/MetingJS

https://www.cnblogs.com/enjoy233/p/10328361.html#%E5%A4%8D%E5%88%B6%E6%AD%A3%E6%96%87%E6%96%87%E5%AD%97%E6%97%B6%E8%87%AA%E5%8A%A8%E5%8A%A0%E7%89%88%E6%9D%83

https://www.cnblogs.com/yjlaugus/p/8724881.html#/c/subject/p/8724881.html

https://live2d.fghrsh.net/demo/1.4.2/

https://github.com/fghrsh/live2d_demo
