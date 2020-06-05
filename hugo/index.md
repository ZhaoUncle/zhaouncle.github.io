# Hugo + Github Pages 搭建个人博客


<!--more-->


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

``` shell
hugo
```

# 配置 Github Pages

1. 登录 github，在设置那里创建个人 repo 仓库，一共 2 个，一个是 <username>.github.io 作为个人站点 public 的静态文件，一个是 MyBlogHugo 作为除了 public 这个静态目录的所有文件的仓库。记得创建空repo，不要添加 README 文件哦。
   
   {{< figure src="/images/WX20200605-112904@2x.png" title="图 1" >}}

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



