# 【Nginx】修改 nginx 用户 id 和 gid


<!--more-->



# 前因

近期创建的 web 服务器上安装了新的 nginx 扩容 web 节点，然后发现挂载了 AWS 的 EFS 在 /upload/目录下，ll 发现文件的用户和用户组的权限都不是 nginx，后来得知发现一开始那台 EFS 挂载机的 nginx uid 和 gid 是 993 和 997，与新的这台的 nginx id 不一致，导致权限错乱。



# 解决办法

1. 先停掉 nginx 服务
2. 修改 nginx gid 和 uid，不能与已经存在的冲突，如果非要用已经存在的，要先修改已经存在的uid 和 gid，通过`cat /etc/passwd`寻找
3. 改了之后要修改之前为 nginx 属主属组的文件和目录，用 `chown -R nginx` 或者 `chown -R nginx.nginx` 递归修改



查看 当前 nginx 用户 id

```
[root@proxy conf.d]# id nginx
uid=996(nginx) gid=993(nginx) groups=993(nginx)
```

查看文件和目录属性

```
[root@proxy tmp]# ll html/
total 0
drwxr-xr-x 2 nginx nginx 6 May 17 06:24 index
-rw-r--r-- 1 nginx nginx 0 May 17 06:24 index.html
```

修改 nginx 的 gid

```
[root@proxy conf.d]# groupmod -g 1013 nginx
[root@proxy conf.d]# id nginx
uid=996(nginx) gid=1013(nginx) groups=1013(nginx)

```

再次查看文件和目录属性，这个时候 nginx 用户组就变成 993了

```
[root@proxy tmp]# ll html/
total 0
drwxr-xr-x 2 nginx 993 6 May 17 06:24 index
-rw-r--r-- 1 nginx 993 0 May 17 06:24 index.html
```



修改 nginx 的 uid 就报错了，该用户正在使用，无法修改

```
[root@proxy conf.d]# usermod -u 1013 -g 1013 nginx
usermod: user nginx is currently used by process 1730
```

停掉 nginx，在进行修改

```
[root@proxy conf.d]# systemctl stop nginx
[root@proxy conf.d]# usermod -u 1013 -g 1013 nginx
[root@proxy conf.d]# id nginx
uid=1013(nginx) gid=1013(nginx) groups=1013(nginx)
```

第三次查看文件和目录权限，默认原来的 uid 和 gid

```
[root@proxy tmp]# ll html/
total 0
drwxr-xr-x 2 996 993 6 May 17 06:24 index
-rw-r--r-- 1 996 993 0 May 17 06:24 index.html
```

修改文件目录属性为 nginx

```
[root@proxy tmp]# chown -R nginx.nginx html/
[root@proxy tmp]# ll html/
total 0
drwxr-xr-x 2 nginx nginx 6 May 17 06:24 index
-rw-r--r-- 1 nginx nginx 0 May 17 06:24 index.html
```

全局确认一下还有哪些文件或者目录的属主和属组没有更改

```
find / -uid 996 -ls
find / -gid 993 -ls

```



useradd 创建示例：创建 test 用户

```
[root@proxy conf.d]# groupadd -g 1012 test &&  useradd -u 1012 -g 1012 -s /sbin/nologin -M test 
[root@proxy conf.d]# cat /etc/passwd|grep test
test:x:1012:1012::/home/test:/sbin/nologin
```

> -M ：不创建家目录
>
> -s /sbin/nologin ：该用户不能登录



userdel 删除用户

```
[root@proxy tmp]# userdel -r test
userdel: test home directory (/home/test) not found
```

> -r ：删除用户家目录
