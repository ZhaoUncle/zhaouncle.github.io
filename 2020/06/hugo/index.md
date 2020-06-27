# Hugo（一）Github Pages 搭建个人博客


<!--more-->

<html><head><meta charset="utf-8"></head>

# 序言
Hugo 号称构建网站最快的框架，看过其他相关资料，hexo 在文章数量大的前提下，建站成静态文件的速度很慢，另外从主题（themes）这个角度去考虑，简单也是我的首选，于是从 hugo 下手自己的第一个博客网站。

# 安装

1. mac 安装 hugo
  
```shell
brew install hugo
```

2. 确认安装成功，可用命令行检查版本号进行测试
  
```shell
hugo version
```

3. 其他平台可用直接下载二进制包进行使用，无需编译
  
   <https://github.com/gohugoio/hugo/releases>

# 构建 hugo 项目

1. 新建博客站点
```shell
hugo new site MyBlogHugo
```

2. 创建完站点后的文件结构
```shell
MyBlogHugo
├── archetypes
│   └── default.md
├── config.toml
├── content
├── data
├── layouts
├── static
└── themes
```

# 添加 LoveIt 主题

1. Why choose it?

   在 {{< link "https://themes.gohugo.io//" "Hugo 主题市场" >}} 挑了半天，还是决定用 {{< link "https://github.com/dillonzq/LoveIt" "LoveIt" >}} 这个主题。

   这个 themes 简单好看，功能齐全，作者也一直在努力更新，还支持多语言版本，包括 seo、搜索等功能都有，可以参考 {{< link "https://hugoloveit.com/zh-cn/" "demo 地址" >}}。

2. 将主题作为子模块

   ```shell
   cd MyBlogHugo
   git init
   git submodule add https://github.com/dillonzq/LoveIt.git themes/LoveIt
   ```

# 启动 hugo server

1. 启动 hugo 作为本地调试使用，在浏览器打开 http://localhost:1313/ 即可查看效果。

   ```shell
   hugo server --disableFastRender
   ```

   参数含义：

   disableFastRender：实时将文章的内容更新到站点，不需要重启也能边修改边观看效果。

2. 生成静态文件，会在MyBlogHugo 下面生成 public 的静态文件目录

   ```shell
   hugo
   ```

3. 我是使用 LoveIt 主题自带的 exampleSite 的 config.toml 来进行修改和配置的。

   ```shell
   cd MyBlogHugo     
   cp themes/LoveIt/exampleSite/config.toml .                
   ```

4. 创建博客文章之前，我会使用example 的默认创建初始化详情来进行配置和修改.

   ```shell
   cp themes/LoveIt/archetypes/default.md archetypes/
   # 这样，你每次创建文章的时候，都会使用这里的默认配置进行创建
   hugo new posts/test01.md
   ```

5. default.md 查看

   ```shell
   \---
   title: "{{ replace .TranslationBaseName "-" " " | title }}"
   subtitle: ""
   date: {{ .Date }}
   lastmod: {{ .Date }}
   draft: false #是否作为草稿，如果为 true，不会生产静态文件到 public 目录
   author: "赵叔"
   authorLink: ""
   description: ""
   
   tags: []
   categories: []
   
   hiddenFromHomePage: false
   hiddenFromSearch: false
   
   featuredImage: ""
   featuredImagePreview: ""
   
   toc:
     enable: true	#是 否展开右边目录栏
   math:
     enable: false
   lightgallery: false
   license: ""
   
   \---
   
   <!--more-->
   
   
   ```

   

# 配置 Github Pages



1. 登录 github，在设置那里创建个人 repo 仓库，一共 2 个，一个是 <username>.github.io 作为个人站点 public 的静态文件，一个是 MyBlogHugo 作为除了 public 这个静态目录的所有文件的仓库。记得创建空repo，不要添加 README 文件哦。
  
   

  
  
  <img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/zhaouncle.github.io/images/WX20200605-112904@2x.png" width="800" hegiht="250" align=center/>
  
  
  
2. 初始化仓库 MyBlogHugo，public 目录要忽略，不上传
```shell
cd MyBlogHugo
echo "!public/" >> .gitignore 
git init
git remote add origin git@github.com:ZhaoUncle/MyBlogHugo.git
git add .
git commit -m "no public"
git pull --rebase origin master
git push -u origin master
```

3. 初始化仓库 zhaouncle.github.io
```shell
cd public
git init
git remote add origin git@github.com:ZhaoUncle/zhaouncle.github.io.git
git add .
git commit -m "my blog hugo"
git pull --rebase origin master
git push -u origin master
```

**之后就可以打开 http://\<username\>.github.io 进入到你的博客了。**


