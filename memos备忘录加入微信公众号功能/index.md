# Memos备忘录加入微信公众号功能


<!--more-->

# 1. 注意

**没有80和443端口就不要看了，浪费时间，因为微信公众号接入只支持80和443**


# 2. 安装memos

> https://github.com/usememos/memos


## 2.1 docker 安装
```
docker run \
  --name memos \
  --publish 5230:5230 \
  --volume ~/.memos/:/var/opt/memos \
  neosmemo/memos:latest \
  --mode prod \
  --port 5230
```
## 2.2 docker-compose 安装
```
version: '3'

services:
  memos:
    image: neosmemo/memos:latest 
    container_name: memos
    ports:
      - 5230:5230 
    volumes:
      - ./memos/:/var/opt/memos
    command: "--mode prod --port 5230"
```
打开登录页面，注册新账号密码
![image-20220820143219776](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202208201432855.png)
登录之后需要 api 到 python 脚本。
![image-20220820144031676](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202208201440724.png)

### 2.3 nginx 反代 memos 对外访问

```
server {
    listen  80 ;
    server_name memos.test.com;
    return 301 https://$host$request_uri ;
}
server {
    listen  443 ssl ;
    server_name memos.test.com;

    ssl_protocols       TLSv1.1 TLSv1.2;
    ssl_certificate     ssl/test.com/cert.pem;
    ssl_certificate_key  ssl/test.com/key.pem;

    client_header_buffer_size 128k;
    large_client_header_buffers 4 128k;
    client_max_body_size    5m;

    location /  {
         proxy_pass http://127.0.0.1:5230 ;
           proxy_http_version 1.1;
           proxy_set_header Connection "";
           proxy_set_header  Host $host;
           proxy_set_header Cdn-Src-Ip $http_true_client_ip ;
           proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for ;
    }

    access_log  /var/log/nginx/memos.acc main;
}
```

# 3. memos-proxy 代理服务启动

> https://github.com/ZhaoUncle/memos-proxy

## 3.1 简易安装

```
git clone https://github.com/ZhaoUncle/memos-proxy.git
cd memos-proxy
mv config_demo.ini config.ini

## 按需修改config.ini里面的配置，主要是把memos 的外网访问地址和微信公众号的token复制进去替换启动就行

## docker-compose 安装

docker-compose up -d

## docker run 安装

docker run -d -p 5000:5000  -v "/data/go/memos-proxy/config.ini:/app/config.ini" --name memos-proxy zhaoweiping/memos-proxy

```

### 3.2 nginx 反代
```
server {
    listen  80 ;
    server_name memos-proxy.test.com;
    return 301 https://$host$request_uri ;
}
server {
    listen  443 ssl ;
    server_name memos-proxy.test.com;

    ssl_protocols       TLSv1.1 TLSv1.2;
    ssl_certificate     ssl/test.com/cert.pem;
    ssl_certificate_key  ssl/test.com/key.pem;

    client_header_buffer_size 128k;
    large_client_header_buffers 4 128k;
    client_max_body_size    5m;

    location /  {
         proxy_pass http://127.0.0.1:5000 ;
           proxy_http_version 1.1;
           proxy_set_header Connection "";
           proxy_set_header  Host $host;
           proxy_set_header Cdn-Src-Ip $http_true_client_ip ;
           proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for ;
    }

    access_log  /var/log/nginx/wechat.acc main;
}
```
### 3.3 如果要限制用户从公众号访问memos，需要在config.ini限制openid
第一次要访问失败报错，让他显示openid出来，然后提取出来放到config.ini
<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202208201441274.png" alt="image-20220820144150234" style="zoom: 33%;" />

![image-20220820144200851](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202208201442903.png)

# 4. 微信公众号接入指南：

参考：https://developers.weixin.qq.com/doc/offiaccount/Basic_Information/Access_Overview.html

1. 登录微信公众号：https://mp.weixin.qq.com/
2. 打开“设置于开发”-->“基本配置”-->“服务器配置（已启用）”-->“修改配置”
<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202208201442033.png" alt="image-20220820144216003" style="zoom:33%;" />
3. 按相应要求填写**URL、Token、EncodingAESKey**，然后选择**明文模式**，提交，确认提交成功就好了。
 url 地址是：https://memos-proxy.test.com/wechat
  <img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202208201442725.png" alt="image-20220820144228686" style="zoom: 33%;" />

4. 点击**启用**。
<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202208201442298.png" alt="image-20220820144245265" style="zoom:50%;" />

python 代码参考：https://blog.51cto.com/u_13916362/3801595

# 5. 扩展

## 功能从以下copy ，懒得写

>https://mp.weixin.qq.com/s/JqzNE6yWTL7KkKzb3MIqfw
>https://blog.csdn.net/wbsu2004/article/details/125594736

### 之前账号名称太长了，可以将用户名称改短一点

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202208201442471.png" alt="image-20220820144255439" style="zoom:50%;" />

### 这里可以创建多个账号
<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202208201443013.png" alt="image-20220820144303985" style="zoom:50%;" />

### 创建信息和上传图片也没有太大问题，这里有个小bug，图片名称如果有“（）”，上传显示会有问题
<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202208201443356.png" alt="image-20220820144313324" style="zoom:50%;" />

### 创建#tag，记得“#tag ”后面说有空格
<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202208201443348.png" alt="image-20220820144322315" style="zoom:50%;" />

### 归档，点击“Archive”，感觉相当于回收站
<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202208201443626.png" alt="image-20220820144332594" style="zoom:50%;" />

### 删除文件，需要点击两次
<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202208201443626.png" style="zoom:50%;" />

### 资源，存放图片的
<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202208201443732.png" alt="image-20220820144353702" style="zoom: 50%;" />

### 每日回顾，卡片形式挺好看的
<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202208201444983.png" alt="image-20220820144407913" style="zoom: 50%;" />

### 捷径，相当于筛选功能
<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202208201444985.png" alt="image-20220820144417909" style="zoom:50%;" />

### 这样就可以比较快速筛选想要的信息了
<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202208201444476.png" alt="image-20220820144431414" style="zoom:50%;" />


## flomo · 浮墨笔记
https://flomoapp.com/

![f1a7052eccfb72223d48f608a2fd86e6.png](/Users/aomine/Documents/hugo/MyBlogHugo/content/posts/nas/:/786d5ac4959f4797b1b8c4309f967c75)

