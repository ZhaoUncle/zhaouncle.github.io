# Prometheus热加载方法


<!--more-->

# 在运行时热加载Prometheus的配置信息

Promtheus的时序数据库在存储了大量的数据后，每次重启Prometheus进程的时间会越来越慢，还会导致数据丢失甚至触发报警（报警系统肯定要两套才能符合互相热备避免唯一性）。 而在日常运维工作中会经常调整Prometheus的配置信息，实际上Prometheus提供了在运行时热加载配置信息的功能。

## Prometheus配置的热加载

Prometheus配置信息的热加载有两种方式：

###第一种热加载方式：

查看Prometheus的进程id，发送`SIGHUP`信号:

```shell
kill -HUP <pid>
```



###第二种热加载方式：

发送一个POST请求到`/-/reload`，需要在启动时给定`--web.enable-lifecycle`选项：

```shell
curl -X POST http://localhost:9090/-/reload
```



如果配置热加载成功，Prometheus会打印出下面的log `Loading configuration file`:

```
Jan 11 02:48:22 proxy01 prometheus[392]: level=info ts=2021-01-11T02:48:22.215Z caller=main.go:799 msg="Loading configuration file" filena...heus.yml
```



###我们使用的是第一种热加载方式，systemd unit文件如下：

我的 prometheus 是在 centos7 系统下面yum 安装的 rpm 包，默认是没有 ExecReload 的系统启动配置，只有 start，所以加上这一句则可。

`[root@ld_proxy01 ~]# cat /etc/systemd/system/prometheus.service`

```
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
CPUAffinity=3
Type=simple
ExecStart=/etc/prometheus/prometheus \
    --config.file /etc/prometheus/prometheus.yml \
    --storage.tsdb.path /var/lib/prometheus/ \
    --storage.tsdb.retention.time=90d \
    --web.console.templates=/etc/prometheus/consoles \
    --web.console.libraries=/etc/prometheus/console_libraries \
    --web.external-url=http://prometheus.op.hwtech.hk:10080
ExecReload=/bin/kill -HUP $MAINPID
[Install]
WantedBy=multi-user.target
```

然后重新加载一下配置即可，一定要执行，否则会报错。

```
systemctl daemon-reload
```



在更改完`/etc/prometheus/prometheus.yml`之后，仅需要重新加载配置，而不需重启进程时，只需要运行以下命令即可。

```
systemctl reload prometheus
```






