# Github和 gitee之raw使用


<!--more-->

# github Raw 文件

例如，GitHub 文件原链接：

```awk
https://github.com/ZhaoUncle/image/blob/main/blog/08a268a7536468e8263fda199e84ebed.jpg
```

可通过以下几种 URL 查看 raw 文件：

```awk
https://raw.githubusercontent.com/ZhaoUncle/image/main/blog/08a268a7536468e8263fda199e84ebed.jpg

https://github.com/ZhaoUncle/image/raw/main/blog/08a268a7536468e8263fda199e84ebed.jpg

https://github.com/ZhaoUncle/image/blob/main/blog/08a268a7536468e8263fda199e84ebed.jpg?raw=true

```

# RawGit

> When you request certain types of files (like JavaScript, CSS, or HTML) from raw.githubusercontent.com or gist.githubusercontent.com, GitHub serves them with a Content-Type header set to text/plain. As a result, most modern browsers won't actually interpret these files as JavaScript, CSS, or HTML and will instead just display them as text.

RawGit 作为一个缓存代理，提供的功能是缓存 GitHub 中 raw 文件并添加上正确的 Content-Type header，从而使文件能被浏览器正确渲染。

RawGit 对未开通 GitHub Pages 的项目中的任意 HTML/CSS/JS 文件以及 Gist 代码的渲染展示提供了方便。

使用方法：[https://rawgit.com/](https://link.segmentfault.com/?enc=hz22B6UkI8czhIc9oa2dEQ%3D%3D.JpP14X0ta3T%2F0GExDBumbqnJW9zrQ25%2F%2FvlouRr5gIQ%3D) 或 [https://raw.githack.com/](https://link.segmentfault.com/?enc=gU%2Bct4s0NpUQh86aZ%2F%2Fr8g%3D%3D.twgsCrg7uuVEkyzxxCMW5vx8%2FXaCgIGAE2fRwTlWf1A%3D)

# gitee raw 文件

gitee 原文件：https://gitee.com/qingfeng689/image/blob/main/blog/08a268a7536468e8263fda199e84ebed.jpg

可通过以下几种 URL 查看 raw 文件：

```
https://gitee.com/qingfeng689/image/raw/main/blog/08a268a7536468e8263fda199e84ebed.jpg
```



# 参考文章

https://segmentfault.com/a/1190000006669149

