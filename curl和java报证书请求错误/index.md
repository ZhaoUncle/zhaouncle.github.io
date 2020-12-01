# curl 和 java 报证书请求错误


<!--more-->

# 1. 说明：

以下：例子的域名因为工作环境的问题，被我拿自己的博客域名替代了，所以无法进行模拟测试，请珍重，哈哈！

# 2. 环境：

```shell
centos：7.5

java jdk：1.8.0_74
```

# 3. curl 请求报错

```curl
[root@test01 tmp]# curl "https://www.zhaouncle.com/api/v2/app/getBopomofo?source=%e8%b5%b5%e8%b6%99" 
curl: (60) Peer's Certificate issuer is not recognized.
More details here: http://curl.haxx.se/docs/sslcerts.html

curl performs SSL certificate verification by default, using a "bundle"
 of Certificate Authority (CA) public keys (CA certs). If the default
 bundle file isn't adequate, you can specify an alternate file
 using the --cacert option.
If this HTTPS server uses a certificate signed by a CA represented in
 the bundle, the certificate verification probably failed due to a
 problem with the certificate (it might be expired, or the name might
 not match the domain name in the URL).
If you'd like to turn off curl's verification of the certificate, use
 the -k (or --insecure) option.
```

## 3.1 解决办法一，治本之法

### 3.1.1 Firefox 火狐浏览器

打开 [**https://www.zhaouncle.com**](https://www.zhaouncle.com)，然后依次点击以下操作

“🔐安全锁图标”——》“向右箭头”——》“更多信息”——》“查看证书”——》“中间那个证书”，下载为 pem 文件

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201201113243446.png" width="800" hegiht="250" align=center/>

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201201113636722.png" width="800" hegiht="250" align=center/>

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201201113817091.png" width="800" hegiht="250" align=center/>

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201201113934843.png" width="800" hegiht="250" align=center/>

### 3.1.2 进入 centos 系统

将下载的 pem 文件放入`/etc/pki/ca-trust/source/anchors` 目录，然后执行 `update-ca-trust extract` 命令

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201201115018829.png" width="800" hegiht="250" align=center/>

## 3.2 解决办法二**，治标**

`curl -k` 或者 `curl --insecure`，在命令行上直接避免证书校验

```
curl -k "https://www.zhaouncle.com/api/v2/app/getBopomofo?source=%e8%b5%b5%e8%b6%99" 
curl --insecure "https://www.zhaouncle.com/api/v2/app/getBopomofo?source=%e8%b5%b5%e8%b6%99" 
```

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201201115154044.png" width="800" hegiht="250" align=center/>



##3.3 解决办法三**，治标**

###3.3.1 wget 解决方法：

```shell
echo "check_certificate = off" >> ~/.wgetrc
```

###3.3.2 curl 解决方法：

```shell
echo "insecure" >> ~/.curlrc
```

# 4. Java 和 curl 都请求证书错误

## 4.1 以下是 curl 报错：

```shell
[centos@test01 ~]$ curl "https://www.zhaouncle.com/api/v2/app/getBopomofo?source=%e8%b5%b5%e8%b6%99"
curl: (60) Peer's Certificate issuer is not recognized.
More details here: http://curl.haxx.se/docs/sslcerts.html

curl performs SSL certificate verification by default, using a "bundle"
 of Certificate Authority (CA) public keys (CA certs). If the default
 bundle file isn't adequate, you can specify an alternate file
 using the --cacert option.
If this HTTPS server uses a certificate signed by a CA represented in
 the bundle, the certificate verification probably failed due to a
 problem with the certificate (it might be expired, or the name might
 not match the domain name in the URL).
If you'd like to turn off curl's verification of the certificate, use
 the -k (or --insecure) option.
[centos@test01 ~]$ ping www.zhaouncle.com
PING www.zhaouncle.com (10.0.0.100) 56(84) bytes of data.
64 bytes from test01 (10.0.0.100): icmp_seq=1 ttl=64 time=262 ms
64 bytes from test01 (10.0.0.100): icmp_seq=2 ttl=64 time=262 ms
64 bytes from test01 (10.0.0.100): icmp_seq=3 ttl=64 time=262 ms
^C
--- www.zhaouncle.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2002ms
rtt min/avg/max/mdev = 262.636/262.636/262.637/0.418 ms

```

##4.2 以下是 java 请求 www 证书报错

```
org.springframework.web.client.ResourceAccessException: I/O error on GET request for "https://www.zhaouncle.com/api/v2/app/getBopomofo": sun.security.validator.ValidatorException: PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target; nested exception is javax.net.ssl.SSLHandshakeException: sun.security.validator.ValidatorException: PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target
```

## 4.3 说明

### 4.3.1 疑问：大家肯定很好奇我为啥会 ping 域名，然后还是内网 ip？

解答：因为绑定了 hosts，然后请求的是内网 ip，于是走的是本地的 nginx，nginx 配置了证书，而不是外部的 cdn，如果是走外部请求走 cdn，curl 和 java 都不会报错，因为 cdn 已经绑定了证书，这个证书上传了证书链。哎，没错了，证书链。

### 4.3.2 何谓证书链

>加速 https 需要上传 SSL 证书，打开公钥 domain.com.crt ，发现里面有 3 个证书：
>
>证书链。一般是一个用户证书，一个中间证书，和一个根证书。
>
>一般只需要 **用户证书+中间证书** 就可以了, 根证书不用传, 除非你这个证书链不是三级,而是有两个中间证书.
>
>一般来讲，只有传 **用户证书** 才能正常工作，可以同时传 **用户证书和中间证书** 或者 **用户证书和中间证书和根证书**
>
>注意这些证书必须在同一个文件里面

格式如下：

```
-----BEGIN CERTIFICATE-----
MIIFSzCCBDOgAwIBAgIQHV3ex3xRLXOHkz2GjVAKrjANBgkqhkiG9w0BAQsFADCB
......后面省略，第一个是用户证书
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
MIIGCDCCA/CgAwIBAgIQKy5u6tl1NmwUim7bo3yMBzANBgkqhkiG9w0BAQwFADCB
......后面省略，第二个是中间证书
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
MIIFdDCCBFygAwIBAgIQJ2buVutJ846r13Ci/ITeIjANBgkqhkiG9w0BAQwFADBv
......后面省略，第三个是根证书
-----END CERTIFICATE-----
```

### 4.3.3 解决：

解决方法：在哪 nginx 那里把 www 的 crt 证书添加进中间证书，ok，就解决了所以问题，而且还不需要 **步骤 3** 但对对系统和命令行进行从处理，就可以解决问题。

参考：https://ep.gnt.md/index.php/curl-60-peers-certificate-issuer-is-not-recognized/


