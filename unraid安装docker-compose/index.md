# Unraid 安装 Docker Compose


<!--more-->

# 1、介绍

插件地址：https://forums.unraid.net/topic/114415-plugin-docker-compose-manager/

参考示例：https://github.com/dannymate/revolt-unraid-self-hosted

[USING DOCKER-COMPOSE WITH UNRAID](https://unraid.kushan.fyi/)

[在 DOCKER 模板中为 UNRAID 特定信息使用 DOCKER 标签，以允许在 UNRAID 模板和 DOCKER-COMPOSE 文件之间进行 1:1 映射。](https://forums.unraid.net/topic/105284-use-docker-labels-for-unraid-specific-information-in-docker-templates-to-allow-for-a-11-map-between-unraid-templates-and-docker-compose-files/)

[有人会对使用 docker-compose 和 unraid 的详细指南感兴趣吗？](https://old.reddit.com/r/unRAID/comments/mlcbk5/would_anyone_be_interested_in_a_detailed_guide_on/)

 [从正在运行的容器创建 compose 文件](https://github.com/danmed/Docker-Compose-Backup/blob/main/create-compose-files.sh)

[Generate a docker-compose yaml definition from a running container](https://github.com/Red5d/docker-autocompose)

[docker run asdlksjfksdf > docker-composerize up](https://github.com/magicmark/composerize)

# 2、安装 docker-compose

## 2.1 方法一：插件安装，后面示例也是用的插件

![image-20220208163501477](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202202081635602.png)

![image-20220208163718189](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202202081637292.png)

## 2.2 手动安装

官网：https://docs.docker.com/compose/install/

unraid用户建议修改go文件，在文件中添加如下内容：

```
curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
```

可以使用命令行或者`CA Config Editor`插件

![image-20220208184044073](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202202081840109.png)

# 3、 进入ui 设置界面

## 3.1 方法一：在 docker 界面进入

![image-20220208163810311](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202202081638354.png)

## 3.2 方法二：在插件页面进入

url 是：http://ip:port/Settings/compose.manager

![image-20220208173011805](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202202081730853.png)

# 4、创建一个compose yml 配置 nginx 示例

![image-20220208164153512](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202202081641625.png)

![image-20220208164320586](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202202081643636.png)

![image-20220208173246839](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202202081732894.png)



![image-20220208173347796](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202202081733883.png)



## 4.1 选择 “EDIT STACK“，编辑 yml 配置文件

注意：玩 unraid docker 的要知悉 `/mnt/user/appdata/` 是 docker 挂载文件到本地的默认路径

```yaml
version: '3'
services:
  web:
    image: nginx
    volumes:
     - /mnt/user/appdata/nginx/templates:/etc/nginx/templates
    ports:
     - "8080:80"
    environment:
     - NGINX_HOST=foobar.com
     - NGINX_PORT=80
```

![image-20220208174114101](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202202081741149.png)

![image-20220208174143915](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202202081741964.png)

![image-20220208174227193](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202202081742234.png)



## 4.2 会在 docker 里面生成一个 docker 应用，但是因为不是用的默认 docker 配置，所以不能自定义图标和 webui 等快捷功能，请知悉。

![image-20220208174625018](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202202081746065.png)

## 4.3 docker-compose yml 地址

默认在 unraid 的路径是： `/boot/config/plugins/compose.manager/projects/`

![image-20220208183358949](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202202081833991.png)



# 5. 如果你不想用 web ui，直接在你喜欢的路径下使用命令行模式，docker-compose 使用就好了，这里不多做介绍


