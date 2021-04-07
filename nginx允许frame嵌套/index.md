# Nginx 允许 frame 嵌套


<!--more-->



# X-Frame-Options

The **`X-Frame-Options`** [HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP) 响应头是用来给浏览器指示允许一个页面可以被其他域名进行嵌套 iframe 网页，被第三方调用。站点可以通过确保网站没有被嵌入到别人的站点里面，从而避免 [clickjacking](https://zh.wikipedia.org/wiki/Clickjacking) 攻击。

然而 `X-Frame-Options` 是个已广泛支持的非官方标准，目前 Chrome 和 Firefox 均不支持。

[语法](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/X-Frame-Options#语法)

`X-Frame-Options` 有三个可能的值：

```
X-Frame-Options: deny
X-Frame-Options: sameorigin
X-Frame-Options: allow-from https://example.com/
X-Frame-Options: allow-all
```

### [指南](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/X-Frame-Options#指南)

换一句话说，如果设置为 `deny`，不光在别人的网站 frame 嵌入时会无法加载，在同域名页面中同样会无法加载。另一方面，如果设置为`sameorigin`，那么页面就可以在同域名页面的 frame 中嵌套。

- `deny`

  表示该页面不允许在 frame 中展示，即便是在相同域名的页面中嵌套也不允许。

- `sameorigin`

  表示该页面可以在相同域名页面的 frame 中展示。这里指的是同一个域名，比如你的 nginx server 配置的是 www.test.com，那么 abc.test.com 是不会被允许嵌套的。也就是只允许自己嵌套自己。

- `allow-from *uri*`

  表示该页面可以在指定来源的 frame 中展示。

## [例子](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/X-Frame-Options#例子)

**Note:** 设置 meta 标签是无效的！例如 `<meta http-equiv="X-Frame-Options" content="deny">` 没有任何效果。不要这样用！只有当像下面示例那样设置 HTTP 头 `X-Frame-Options` 才会生效。

### [配置 nginx](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/X-Frame-Options#配置_nginx)

配置 nginx 发送 X-Frame-Options 响应头，把下面这行添加到 'http', 'server' 或者 'location' 的配置中:

```
add_header X-Frame-Options sameorigin always;
```

配置允许其他域名嵌套：

```
add_header X-Frame-Options "ALLOW-FROM https://*.test.com";
```



## [结果](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/X-Frame-Options#结果)

在 Firefox 尝试加载 frame 的内容时，如果 X-Frame-Options 响应头设置为禁止访问了，那么 Firefox 会用 about:blank 展现到 frame 中。也许从某种方面来讲的话，展示为错误消息会更好一点。

以下为在 Google Chrome 的报错信息，能够正常访问，不过还是有报错。

![image-20210402093553412](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210402093553.png)

![image-20210402093729385](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210402093729.png)

# 参考页面

- https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/X-Frame-Options




