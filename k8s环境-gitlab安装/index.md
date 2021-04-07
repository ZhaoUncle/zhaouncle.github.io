# Gitlab安装


<!--more-->

# 使用 docker-compose 安装

我这里简单直接用 http+ip 访问，不做过多配置，如有需要请另行修改

```
mkdir /data/gitlab
vi docker-compose.yml
```



```
web:
  image: 'gitlab/gitlab-ce:latest'
  restart: always
  hostname: 'gitlab.example.com'
  container_name: gitlab
  environment:
    GITLAB_OMNIBUS_CONFIG: |
      external_url 'http://192.168.110.238'
      gitlab_rails['gitlab_shell_ssh_port'] = 622
  ports:
    - '80:80'
    - '443:443'
    - '622:22'
  volumes:
    - '/data/gitlab/src/config:/etc/gitlab'
    - '/data/gitlab/src/logs:/var/log/gitlab'
    - '/data/gitlab/src/data:/var/opt/gitlab'
```

启动

```
docker-compose up -d
```

如果要修改配置，然后重新`docker-compose restart`就可以了

`vi src/config/gitlab.rb `

## 首次访问 gitlab

使用域名`gitlab.test.com`或者该主机 IP 首次登录时会要求设置 root 用户的密码，完成后就可以用 root 和新设密码登录；然后按需创建 Group, User, Projects等，还有相关配置。

## gitlab 数据备份

因为是基于 docker-compose 把配置和数据都挂载本地路径了，所以其实只要把本地路径用 rsync 进行同步就可以达到备份的效果。

比如以下的备份方式：用crontab 去定时同步，不过这个脚本用了 delete，会删除源目录不存在，但是目的目录存在的文件

方法一：rsync 同步，好处是如果故障了，可以直接 docker-compose 直接起一个 gitalb

```
# 创建备份脚本
cat > /root/gitlab-backup.sh << EOF
#!/bin/bash
# 请事先配置 gitlab 服务器到备份服务器的免密码 ssh 登录
rsync -av --delete /srv/gitlab/config '-e ssh -l root' 192.168.1.xx:/backup_gitlab/config
rsync -av --delete /srv/gitlab/data '-e ssh -l root' 192.168.1.xx:/backup_gitlab/data
EOF

# 创建并应用 crontab
cat > /etc/cron.d/gitlab-backup << EOF
## 每3个小时同步备份一次，具体根据需要修改
11 */3 * * * root bash /root/gitlab-backup.sh > /root/gitlab/sync.log 2>&1
EOF

```

方法二：gitlab 的备份命令直接创建备份文件，可以直接新起一个 gitlab 然后进行导入，版本必须和原来一致

```
docker exec -t <container name> gitlab-backup create
```



如果 gitlab 服务器真的出现不可恢复的故障，丢失数据，那么至少保留有3小时前的备份，利用备份的文件，同样再用 docker 挂载 volume的方式运行，这样就可以恢复原 gitlab 服务运行。



## 升级 gitlab

因为前面使用了 docker-compose 方式安装，因此 gitlab 升级很方便。

- 升级前停止/删除容器：docker-compose stop
- 如上节执行备份数据
- 修改 docker-compose.yml 的 images，然后`docker-compose up -d`

#参考：

- https://docs.gitlab.com/omnibus/docker/#install-gitlab-using-docker-compose

- 备份还原请参考：https://docs.gitlab.com/ee/raketasks/backup_restore.html

  
