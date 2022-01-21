# Markdown-here工具将Markdown语法转为富文本格式


<!--more-->



# 一、场景

1. 最近经常用 markdown 写一些上万文字+几百图片的文章，typora因为是实时加载的，所以gpu能力不足的话，会非常卡顿，后面就直接换到 vscode去进行编辑了。
2. 然后发现这种类型的文章markdown编辑好之后，即便转成了html或者富文本，在复制到smzdm编辑器也会导致样式丢失和图片上传失败，手动复制也不行，必须要要使用编辑器的插入图片功能，这就相当于要重新编辑一次，我的天，本来就写的要死要活，我还得重新编辑整理，不是说了markdown是通用文本了吗，富文本都不支持复制，哎，我也是无语的要死。折腾了两天，我发现了个好东西，可以解决这个问题，再也🙅🏻‍♀️用😭了。

3. 这是我的 github 源码，存了一些网上找到的 markdown here css，每个sytle 文件夹都写明了从哪里复制过来的，请留意

【**ZhaoUncle/Markdown-Here-CSS**】：https://github.com/ZhaoUncle/Markdown-Here-CSS

# 二、使用 markdown-here 编辑文件

1. github：https://github.com/adam-p/markdown-here/

2. 使用场景：只能在不能使用 markdown 格式的富文本编辑器中使用，如果是 markdown 编辑器就不行。

3. 富文本编辑器，Multi-function Text Editor, 简称 MTE, 是一种可内嵌于浏览器，所见即所得的文本编辑器。可以简单理解为基于html代码转换成你需要的格式，一般在线web编辑器不支持markdown语法的，大部分都是富文本编辑器，你可以按照需要调整格式，比如smzdm的文章编辑器，就是富文本编辑器。

## 介绍

Markdown Here 是 Google Chrome, Firefox, Safari, Opera, and Thunderbird 浏览器插件，起初是用在 email 邮件里面讲 markdown 语法转化成 html 富文本格式，现在只要是在 web 端，都可以利用插件直接转换了，也不需要说特定在email页面里面了。

支持语法高亮，需要设置CSS等来进行富文本编辑

## 安装教程

chrome 安装用：https://chrome.google.com/webstore/detail/markdown-here/elifhakcjgalahccnjkneoccemfahfoa

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20220119172234281.png" alt="image-20220119172234281" style="zoom:50%;" />

安装后打开显示在页面，其实可以不显示，平时用右键就好了

![image-20220119175058624](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20220119175058624.png)



网络不好的同学，就需要先下载下来，或者你直接用 Microsoft Edge 浏览器吧，chrome不适合你

下载地址：https://chrome.zzzmh.cn/info?token=elifhakcjgalahccnjkneoccemfahfoa

详细安装地址：https://chrome.zzzmh.cn/help?token=setup

![image-20220119174916449](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20220119174916449.png)

打开开发者模式，然后直接把crx插件拖到这个页面就好了

![image-20220119175552547](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20220119175552547.png)

![image-20220119175354760](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20220119175354760.png)

![image-20220119175510379](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20220119175510379.png)



然后右键点击图标，弹出“选项”框，点击使用

按照你自己喜欢的格式修改你需要的CSS代码就好了，一般改“基本渲染 CSS”，不然就默认也够用了，不改也没问题

![image-20220119180506454](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20220119180506454.png)







![image-20220119202813903](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202201192028037.png)



# 三、张大妈家使用 markdown-here 进行测试

通过观察 [纵笔浮生](https://zhiyou.smzdm.com/member/2279839416/) 和 [阿文菌](https://zhiyou.smzdm.com/member/6902738986/) 的文章发现，在 app 端，h1 渲染过的，目录还会存在，但是在 web 端，目录就直接消失了。各有利弊，诸位要自己选择了。

如果既想要有web端目录，还想要有格式，那么只能关闭h1渲染，然后还不能全选

就像我下面这样

1. 先把 markdown 语法复制进去，然后找到对应的 h1 一级目录，手动 先转换成 smzdm 自带的编辑器 h1；
2. 不要全选，然后按照段落分别选择对应的 h1 目录内容，在进行转换，也不麻烦；
3. 测试发现，圆角形状都会被转化成直角，我放弃了。。。。
4. 看来 css 优化还真需要费时间， 真辛苦那些前端开发的同学了，我真是受不了，要是有个产品在我旁边指指点点，就像我自己一样在否定之前的设计，真想来一下；

5. 图片能成功转换并且上传，即便是我几百张图，都能成；

markdown-here 正常转换：

![image-20220120224932691](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202201202249724.png)

张大妈会转换成

![image-20220120224907436](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202201202249516.png)



# 四、微信公众号使用 markdown-here 进行转换测试

1. 右键是没有 **Markdown 转换** 字样，只能点击右上角的图标进行转换；
2. 图片一开始能转换成功，但是上传失败，所以我放弃使用了。

![image-20220120230352521](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202201202303561.png)



# 五、知乎文章编辑测试

不用看了，知乎的转换也有问题，不确定是 css 语法问题，还是编辑其本身导致。



# 六、推荐阅读

https://post.smzdm.com/p/awx48pxp/

https://post.smzdm.com/p/aqn9pk7k/

https://post.smzdm.com/p/ammr4n9p/




