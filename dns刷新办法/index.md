# DNS刷新办法


<!--more-->

# Linux 刷新 dns 缓存

1. **刷新DNS－Systemd Resolved**

```
systemd-resolved --flush-caches
```

2. **刷新DNS－NSCD**

```
systemctl restart nscd.service
```



# windows 刷新 dns 缓存

```
ifconfig/flushdns
```



# Mac 刷新 dns 缓存

```
sudo dscacheutil -flushcache;sudo killall -HUP mDNSResponder
```


