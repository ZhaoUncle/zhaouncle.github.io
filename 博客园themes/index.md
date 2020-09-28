# 博客园主题


<!--more-->

# 1. 主题

[Cnblogs-Theme-SimpleMemory 1.3.3 版本](https://github.com/BNDong/Cnblogs-Theme-SimpleMemory/tree/v1.3.3)

# 2. 权限申请

先在博客园申请开通博客，这里就不做介绍了，然后[申请 js 权限](https://i.cnblogs.com/settings)（博客侧边栏公告）

![](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/20200928084358.png)

![](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/20200928090420-20200928090440540.png)





# 3. [博客园后台](https://i.cnblogs.com/settings)设置

## 3.1 用户设置

![image-20200928101839678](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928101839678.png)

**参数：**

>标题：主要影响后台的显示；
>
>博客地址名：https://www.cnblog.com/博客地址名/，这个只会修改只能通过邮件申请，我申请过一次，申请完之后用了 js 的话，还会用旧的链接地址访问，需要把 css 和 js 的代码清掉保存，再重新填写再保存，才能快速清除；
>
>登录用户名：这个影响不大，因为下面有个显示名称，就是替换这个的
>
>密码：忽略
>
>显示名称（作者名）：这里会替换掉登录用户名的显示规则，会影响浏览器标签那里的 title，还有显示作者的地方；
>
>邮箱：评论那里可以选择发送邮箱
>
>回复通知邮箱：默认楼上那个
>
>时区：utc+8 即可，北京时间
>
>语言与地区：简体中文



## 3.2 模板设置

![image-20200928102152457](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928102152457.png)

**参数：**

> 博客皮肤：选择SimpleMemory
>
> 页面定制 CSS 代码：
>
> > CSS代码位置：[/src/style/base.min.css](https://cdn.jsdelivr.net/gh/BNDong/Cnblogs-Theme-SimpleMemory@v1.3.3/src/style/base.min.css) ，拷贝此文件代码至页面定制CSS代码文本框处即可。
>
> > /src/style/base.min.css 的修改参考 /src/style/base.css。博客设置请使用压缩版本，直接使用 /src/style/base.css 会字符超限！
>
> 禁用模板默认 CSS：**这里要记得 ![image-20200928112416329](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928112416329.png)**
>
> 博客侧边栏公告：
>
> > JavaScript 对象是被命名值的容器。值以名称:值对的方式来书写（名称和值由冒号分隔）。

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

![image-20200928133928766](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928133928766.png)



公告这里要选上![image-20200928112416329](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928112416329.png)，否则部分 js 文件会无法生效，其他的就自行参考自己的需要了。

到了这一步，基本上一个完整的博客主题就搭建完成了，保存即可自由玩耍写博客了。



# 4. 功能扩展

## 4.1 菜单栏图标

### 4.1.1 查看菜单栏

![image-20200928162846066](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928162846066.png)

[字体图标库](https://bndong.github.io/Cnblogs-Theme-SimpleMemory/v1.1/#/Docs/Customization/fonticon) 列举了部分内置的图标，但是还有一些需要自定义，比如 “k8sYmal 生成器” 这个图标就没有内置，需要我们自己去 iconfont ，阿里的图标库自定义。

### 4.1.2 登录 [iconfont](https://www.iconfont.cn/)

我一开始选择“Github”登录的方式，但是直接报错了，所以我只能选择“新浪微博”的登录方式，主要是我不玩微博，我觉得很蛋疼

### 4.1.3 创建项目

我们先进到 “资源管理” -- “我发起的项目” ，创建一个 “博客园” 项目，如图所示

![image-20200928163537767](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928163537767.png)

![image-20200928163721514](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928163721514.png)



### 4.1.4 生成代码

然后生成代码，填入 3.2 的 “博客侧边栏公告” 的 js 对象 **fontIconExtend** 更换成你自己的就行了，这个要添加图标进去后才能生成。

![image-20200928163806514](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928163806514.png)



### 4.1.5 图标加入购物车

你可以直接搜索 “kubernetes”，也可以直接一个一个搜，不过一般不会这么傻吧？哈哈。重要的是选中图标，把鼠标移到图标上面，然后添加进购物车

![image-20200928164246239](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928164246239.png)



### 4.1.6 从购物车添加至项目

点击购物车图标，然后 “添加至项目” 即可，当然你也可以忽略前面创建项目的步骤，直接在这里的![image-20200928164444244](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928164444244.png)选中添加项目也行，随你自己喜欢了。

![image-20200928164401982](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928164401982.png)

### 4.1.7 添加成功

点击确定，就添加成功了，然后在“生成代码”就好啦，鼠标移动到图标上面，直接复制代码，

![image-20200928164637400](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928164637400.png)

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

![image-20200928170447277](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928170447277.png)

### 4.2.3 添加 js 代码

> 注意：特效只需要添加一种即可

#### 4.2.3.1 烟花特效

在 **“页首 HTML 代码” **那里添加以下代码

```js
<!--鼠标点击特效，烟花效应-->
<script src="https://cdn.jsdelivr.net/gh/ZhaoUncle/images/static/mouse-click.js"></script>
<canvas width="1777" height="841" style="position: fixed; left: 0px; top: 0px; z-index: 2147483647; pointer-events: none;"></canvas>
```



![image-20200928170753274](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928170753274.png)

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

![image-20200928174154923](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928174154923.png)

### 4.3.3 获取统计代码 ID

![image-20200928174331794](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928174331794.png)

### 4.3.4 开启详细数据

![image-20200928174425996](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928174425996.png)

### 4.3.4 配置页脚 HTML 代码

进入博客园后台设置，配置**页脚 HTML 代码**，将 4.3.3 的统计 ID 替换，然后 copy 进去

```js
<div id="cnzzProtocol"  style="display: none;">
    <span class="id_cnzz_stat_icon" id='cnzz_stat_icon_你的统计ID'></span>
    <script src='https://v1.cnzz.com/z_stat.php?id=你的统计ID&online=1&show=line' type='text/javascript'></script>
</div>
```

![image-20200928174738903](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200928174738903.png)



# 5. 参考资料

https://github.com/BNDong/Cnblogs-Theme-SimpleMemory

https://bndong.github.io/Cnblogs-Theme-SimpleMemory/v1.1/#/

https://www.cnblogs.com/wkfvawl/p/9414180.html


