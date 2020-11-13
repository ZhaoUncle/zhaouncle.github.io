# 解决 go get下载包慢或者失败的问题


<!--more-->

## 使用goproxy

Go的1.11版本以后 可以设置`GOPROXY`这个变量，来设置代理。 如果你自己有代理服务器就可以把这个环境变量设置成自己的代理。 首先开启`go module`支持:

```go
go env -w GO111MODULE="on"
```



不管是 mac、linux、还是 windows，只要安装了 go 1.11 版本以上，就直接安装代理，简单方便

```go
go env -w GOPROXY=https://goproxy.cn,direct
```


