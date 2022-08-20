# Markdown文章同步平台之doocs Md测评


<!--more-->



# 1.  安装 doocs-md

https://github.com/doocs/md

https://github.com/soulteary/docker-wechat-markdown-editor

## 1.1 unraid 安装

```
Name: markdown-doocsmd
Overview: https://github.com/soulteary/docker-wechat-markdown-editor && https://github.com/doocs/md
Repository: soulteary/wechat-markdown-editor:1.5.7
Docker Hub URL: https://hub.docker.com/r/soulteary/wechat-markdown-editor
Icon URL: https://camo.githubusercontent.com/1a28c56ce7d0ad891e9349b4713b439ea5630c97305cdaa292ce8bc0b7fea64e/68747470733a2f2f63646e2e6a7364656c6976722e6e65742f67682f646f6f63732f6d642f7075626c69632f6173736574732f696d616765732f6c6f676f2d322e706e67
WebUI: http://[IP]:[PORT:8080]/

WebUI: 18181
Container Port: 8080
```



![image-20220128213616267](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202201282136388.png)

![image-20220128215804525](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202201282158591.png)

## 1.2 在线编辑器

- Netlify: [https://mdhere.netlify.app](https://mdhere.netlify.app/)
- Gitee Pages：https://doocs.gitee.io/md
- GitHub Pages：https://doocs.github.io/md

# 2. 微信公众号平台测评

## 2.1 图片上传问题

自带 gitee、github、阿里云 oss、腾讯云 oss、七牛云，还有自定义模式

经过测试，gitee raw 可以复制到公众号，并成功上传，免费者的福音。付费的阿里云 oss 和腾讯云 oss 也可以解决微信公众号图片上传的问题。

![image-20220128215857346](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202201282158396.png)



# 3. 其他平台测试结果

## 3.1 知乎平台测试

表格、标题等富文本皆失败，只能是正常的 h1、h2 格式

## 3.2 张大妈测试

张大妈家样式失败了，图片上传成功，建议还是使用 markdown-here。

