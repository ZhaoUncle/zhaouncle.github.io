# 【Nginx】url 带有 “https://” 双斜杠特殊处理


<!--more-->

### 问题

最近发现经常有客户端出现 url 复制两次访问的情况，导致触发了 waf 无法访问 403。所以就想着怎么处理，解决这个问题。

当用户请求：`https://www.test.com/https://www.test.com/`或者`https://www.test.com/https://www.test.com`会出问题

###原因

开启压缩之后，nginx 的 uri 会默认把双斜杆`//`自动合并为单斜杆`/`

###方法一：merge_slashes

>启用或禁用将URI中的两个或多个相邻斜杠压缩为单个斜杠。处于安全考虑，一般不关闭。

```
merge_slashes off;
```

### 方法二：


```
if ( $uri ~ "http:(/)localhost(/|)$" ) {
    rewrite ^ $scheme://$host:$server_port/ break;
}  
```

###方法三：

```
if ( $uri ~ "http:/(.*)localhost(/|)$" ) {
    rewrite ^ $scheme://$host:$server_port/ break;
}  
```



# 参考

http://nginx.org/en/docs/http/ngx_http_core_module.html#merge_slashes

https://stackoverflow.com/questions/38175840/match-and-redirect-http-double-slash-from-uri-path-with-nginx
