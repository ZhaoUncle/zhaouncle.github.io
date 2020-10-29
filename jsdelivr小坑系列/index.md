# Jsdelivr小坑系列


<!--more-->

# 1. 50m Size 限制

## 1.1 问题

![image-20201029150424740](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201029150424740.png)

url 访问：https://cdn.jsdelivr.net/gh/ZhaoUncle/images@master/books/20201029101540.png

出现以下错误

```
Package size exceeded the configured limit of 50 MB. Try https://github.com/ZhaoUncle/images/tree/master/books/20201029101540.png instead.
```



## 1.2 解决

### 1.2.1 进入你的 github image repo，然后创建一个 **Releases** 版本为 “master”

> 坑：这里必须你是还没满 50M 的时候先创建的 master，否则等满了之后再创建就无效了，所以我这里也是约到坑了，重新创建了一个新的 repo 创建了 master 的 release

![image-20201029112457974](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201029112457974.png)




