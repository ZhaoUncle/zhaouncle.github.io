# Typora + PicGo 编写 hugo md + 图床


<!--more-->

## 1.下载

PicGo：https://github.com/Molunerfinn/PicGo

Typora：https://www.typora.io/

## 2.软件学习地址

少数派：

Typora：https://sspai.com/search/post/Typora
PicGo：https://sspai.com/search/post/PicGo


## 3.介绍

>Typora 是专注于编写 Markdown 格式的编辑软件，使用方便，支持多种格式导出。

>PicGo 是一款免费的图床管理应用，支持拖拽上传，剪切板上传等方式。你可以用它快捷地将图片上传到图床并获得网络链接。

**注意：请不要把 PicGo 安装到 C 盘 Program Files 下**

## 4.为什么需要图床

Markdown 可以理解为增强版的文本文档，语法简单，支持更多的风格样式，相比 word 更加轻便，文件大小更小，同时可导出为指定格式，目前大多是技术博客论坛已支持 Markdown 格式，基本上可以做到一次编写多处使用。当然 Markdown 也存在缺点，比如图片。

Markdown 文档编写时可使用本地图片，但是无法在网络上使用。图床的作用可以理解为将文档中的图片放到网络上，直接引用网络地址，这样可以做到无论在那个平台都可以使用统一的图片地址。

搭建图床教程较多此处不做讨论，作者使用的是 github 搭建的免费图床。

## 5.设置 GitHub Toekn
参考：https://docs.github.com/cn/github/authenticating-to-github/creating-a-personal-access-token

**Token 权限只要设置 “repo” 即可。**

- token：

```shell
|-seetings 
|-- Developer settings 
|------Personal access tokens 下生成

注意：token 只会显示一次，记得保存如果你不建议重新配置一次的话
```

![image-20200824095351728](https://cdn.jsdelivr.net/gh/ZhaoUncle/images//blog/githubToken.png)
![image-20200824095351728](https://cdn.jsdelivr.net/gh/ZhaoUncle/images//blog/githubToken2.png)
![image-20200824095351728](https://cdn.jsdelivr.net/gh/ZhaoUncle/images//blog/githubToken3.png)
<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/images//blog/githubToken4.png" width="800" hegiht="250" align=center/>



## 6.配置 PicGo (GitHub) 图床
参考：https://picgo.github.io/PicGo-Doc/zh/guide/config.html#github%E5%9B%BE%E5%BA%8A

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/images//blog/20200824095351.png" width="800" hegiht="250" align=center/>



## 7. 使用了 CDN 加速

```shell
https://cdn.jsdelivr.net/gh/用户名/仓库名
```

## 8.Typora 设置

**记得“验证图片上传选项”**

![image-20200824103801757](https://cdn.jsdelivr.net/gh/ZhaoUncle/images//blog/image-20200824103801757.png)



![image-20200824114030892](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200824114030892.png)



## 9. 之后你截图然后直接复制到 Typora，它就会自动转化为 markdown 的图片格式并指向你设置的 github 的 url 地址了。



## 10.MyHugoBlog 设置子模块到 images

这一步是介于之前的 hugo 的配置，其它人可以忽略。

```shel
MyBlogHugo/static/
rm -rf images
git submodule add https://github.com/ZhaoUncle/images.git images

cd MyBlogHugo
#添加 sub pull 到 deploy.sh 脚本
gitsubmoduleImages(){
	cd static/images/
	git submodule foreach git pull
}
```






