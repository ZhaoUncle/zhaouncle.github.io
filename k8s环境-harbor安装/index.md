# Harbor安装


<!--more-->

# 1. 环境

- 系统：centos7
- docker：19.03.9
- docker-compose：1.28.6
- Harbor: 2.2.1



# 2. 安装 docker

```
yum remove docker docker-common docker-selinux docker-engine
yum install -y yum-utils device-mapper-persistent-data lvm2
wget -O /etc/yum.repos.d/docker-ce.repo https://download.docker.com/linux/centos/docker-ce.repo
sed -i 's+download.docker.com+mirrors.tuna.tsinghua.edu.cn/docker-ce+' /etc/yum.repos.d/docker-ce.repo
yum makecache fast
yum install docker-ce

```

##2.1 配置 docker 镜像加速和日志分割

```
cat << EOF > /etc/docker/daemon.json 
{
  "registry-mirrors": ["http://f1361db2.m.daocloud.io"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
EOF
```



# 3. 安装 docker-compose

```
curl -L "https://github.com/docker/compose/releases/download/1.28.6/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
docker-compose --version
```



# 4. 安装 harbor

**Tips：**为什么要用 https 而不用 http访问 harbor 镜像仓库呢，因为如果用 http 的话，必须在访问的客户端更改 docker 的 daemon.json 文件添加`--insecure-registry`，如果你客户端机器多的话，改起来麻烦还得重启 docker，但是用 https 访问，就无须更改 daemon.json 配置文件了，这是好处，如果你重启不受影响，那就随你 http 或者 https 了。

## 4.1 生成 https 证书

我为了测试方便，直接使用 IP 方式作为访问，如果要改成域名，直接一键替换就好，还要 在步骤 2.3 要把 v3.ext 的IP=192.168.110.239`改成 以下方式：

```
[alt_names]
DNS.1=yourdomain.com
DNS.2=yourdomain
DNS.3=hostname
```

实际步骤：

```
mkdir /root/harbor/cert/harbor -p
cd /root/harbor/cert 

###1. 生成证书颁发机构证书
#1.1 生成CA证书私钥
openssl genrsa -out ca.key 4096
#1.2 生成CA证书
openssl req -x509 -new -nodes -sha512 -days 365000 \
 -subj "/C=CN/ST=Beijing/L=Beijing/O=example/OU=Personal/CN=192.168.110.239" \
 -key ca.key \
 -out ca.crt

###2. 生成服务器证书
#2.1 生成私钥
openssl genrsa -out 192.168.110.239.key 4096 
#2.2 生成证书签名请求（CSR）
openssl req -sha512 -new \
    -subj "/C=CN/ST=Beijing/L=Beijing/O=example/OU=Personal/CN=192.168.110.239" \
    -key 192.168.110.239.key \
    -out 192.168.110.239.csr
#2.3 生成一个x509 v3扩展文件
cat > v3.ext <<-EOF
authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
extendedKeyUsage = serverAuth
subjectAltName = @alt_names

[alt_names]
IP=192.168.110.239
EOF
#2.4 使用该v3.ext文件为您的Harbor主机生成证书
openssl x509 -req -sha512 -days 365000 \
    -extfile v3.ext \
    -CA ca.crt -CAkey ca.key -CAcreateserial \
    -in 192.168.110.239.csr \
    -out 192.168.110.239.crt
    
###3. 提供证书给Harbor和Docker
#3.1 将服务器证书和密钥复制到Harbor主机上的certficates文件夹中
cp 192.168.110.239.crt /data/harbor/certs/harbor
cp 192.168.110.239.key /data/harbor/certs/harbor
#3.2 转换yourdomain.com.crt为yourdomain.com.cert，供Docker使用
openssl x509 -inform PEM -in 192.168.110.239.crt -out 192.168.110.239.cert

#3.3 将服务器证书，密钥和CA文件复制到Harbor主机上的Docker证书文件夹中，如果将默认nginx端口443映射到其他端口，请创建文件夹/etc/docker/certs.d/yourdomain.com:port或/etc/docker/certs.d/harbor_IP:port。
mkdir -p /etc/docker/certs.d/192.168.110.239/
cp 192.168.110.239.cert /etc/docker/certs.d/192.168.110.239/
cp 192.168.110.239.key /etc/docker/certs.d/192.168.110.239/
cp ca.crt /etc/docker/certs.d/192.168.110.239/

#重新启动Docker Engine
systemctl restart docker

```



证书位置说明；

```
[root@node05 data]# tree /data/harbor/certs/
/data/harbor/certs/
├── 192.168.110.239.cert
├── 192.168.110.239.crt
├── 192.168.110.239.csr
├── 192.168.110.239.key
├── ca.crt
├── ca.key
├── ca.srl
├── harbor
│   ├── 192.168.110.239.crt
│   └── 192.168.110.239.key
└── v3.ext
[root@node05 data]# tree  /etc/docker/certs.d/
/etc/docker/certs.d/
└── 192.168.110.239
    ├── 192.168.110.239.cert
    ├── 192.168.110.239.key
    └── ca.crt
    
```



部署说明：

1. 如果还没有部署 harbor，在生成证书之后，修改 harbo.yml 的配置hostname 和 https 的属性就好，然后`./install`

2. 如果已经部署好 harbor，那么也是要修改 harbor.yml 的配置hostname 和 https 的属性，

   ```
   #2.1 运行prepare脚本以启用HTTPS
   ./prepare
   #2.2 如果Harbor正在运行，请停止并删除现有实例,图像数据保留在文件系统中，因此不会丢失任何数据
   docker-compose down -v
   #2.3 重新启动 harbor
   docker-compose up -d
   ```



验证：

1. 浏览器打开 https://192.168.110.239，账号密码 admin/Harbor12345 
2. 修改 docker 的 daemon.json 把 `"insecure-registries": ["https://192.168.110.239"],` 添加进去，然后

`docker login https://192.168.110.239` ，如果你用的不是默认的 443 端口，记得要用 https://92.168.110.239:888 就是+端口号哦，不然会默认走 80 或者 443 的，导致验证出了问题。



## 4.2 安装

harbor 分为在线安装和下线安装两种，其实就是你的安装包完不完整的问题而已啦，我这里用下线安装方式

```
cd /data/
wget https://github.com/goharbor/harbor/releases/download/v2.2.1/harbor-offline-installer-v2.2.1.tgz
tar xf harbor-offline-installer-v2.2.1.tgz
cd /data/harbor
cp harbor.yml.tmpl harbor.yml
```

修改 harbor 配置文件

```
[root@node05 harbor]# cat harbor.yml|grep -Ev '#|^$'
hostname: 192.168.110.239  #如果 https 证书用的域名，这里要改为域名方式
http:
  port: 80  #如果你想简单点不走 https，直接把 https 关掉，就可以省去上面配置 https 证书的问题了。
https:
  port: 443
  certificate: /data/harbor/certs/harbor/192.168.110.239.crt 
  private_key: /data/harbor/certs/harbor/192.168.110.239.key
harbor_admin_password: Harbor12345   #登录账号 admin 的默认密码
database:
  password: root123			#数据库密码
  max_idle_conns: 50
  max_open_conns: 1000
data_volume: /data   #这里是默认的 docker-compose 的挂载数据的路径，保存在本地
trivy:
  ignore_unfixed: false
  skip_update: false
  insecure: false
jobservice:
  max_job_workers: 10
notification:
  webhook_job_max_retry: 10
chart:
  absolute_url: disabled
log:
  level: info
  local:
    rotate_count: 50
    rotate_size: 200M
    location: /var/log/harbor
_version: 2.2.0
proxy:
  http_proxy:
  https_proxy:
  no_proxy:
  components:
    - core
    - jobservice
    - trivy
```



## 4.3 harbor 开启 helm 的 charts 仓库配置

```
./install.sh --with-chartmuseum	
```

![image-20210405123446713](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210405123446.png)



## 4.4 使用Harbor作为Chart仓库

启用后，默认创建的项目就带有helm charts功能了。

**1、安装push插件**

https://github.com/chartmuseum/helm-push

```shell
helm plugin install https://github.com/chartmuseum/helm-push
```

**2、添加repo**

```
helm repo add  --username admin --password Harbor12345 myrepo http://192.168.110.239/chartrepo/library
```

如果用的自签名的证书，则会报错

```
# 增加仓库，因为使用的自签名证书，所以命令上需要加上，若不加上则会报错：Error: Looks like "https://www.harbor.mobi/chartrepo/myrepo" is not a valid chart repository or cannot be reached: Get https://www.harbor.mobi/chartrepo/myrepo/index.yaml: x509: certificate signed by unknown authority
```

指定证书添加 repo

```
helm repo add --ca-file /data/helm/ssl/192.168.110.239.crt --cert-file /data/helm/ssl/192.168.110.239.cert --key-file /data/helm/ssl/192.168.110.239.key myrepo https://192.168.110.239/chartrepo/library
```

查看

```
[root@node01 ssl]# helm repo ls
NAME            URL                                                   
stable          http://mirror.azure.cn/kubernetes/charts              
aliyun          https://kubernetes.oss-cn-hangzhou.aliyuncs.com/charts
incubator       https://charts.helm.sh/incubator                      
helm-stable     https://charts.helm.sh/stable                         
azure           http://mirror.azure.cn/kubernetes/charts/             
elastic         https://helm.elastic.co                               
library         https://192.168.110.239/chartrepo/library 
```



**3、推送与安装Chart**

```
helm push ms-0.1.0.tgz --username=admin --password=Harbor12345 --ca-file /data/helm/ssl/192.168.110.239.crt --cert-file /data/helm/ssl/192.168.110.239.cert --key-file /data/helm/ssl/192.168.110.239.key https://192.168.110.239/chartrepo/library

helm install ms myrepo/ms
```


