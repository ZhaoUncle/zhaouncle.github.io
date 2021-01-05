# 端口扫描


<!--more-->

#1. nmap 确认端口是否可以通信

可以通信：

```
[root@ld_openvas ~]# nmap -p9100 192.168.3.31

Starting Nmap 6.47 ( http://nmap.org ) at 2020-11-09 08:11 UTC
Nmap scan report for ip-192-168-3-31.eu-west-2.compute.internal (192.168.3.31)
Host is up (0.000067s latency).
PORT     STATE SERVICE
9100/tcp open  jetdirect
MAC Address: 0A:F3:15:B8:34:4A (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.08 seconds
```

不可通信：

```

[root@ld_openvas ~]# nmap -p9100 192.168.3.31

Starting Nmap 6.47 ( http://nmap.org ) at 2020-11-09 08:12 UTC
sendto in send_ip_packet_sd: sendto(5, packet, 44, 0, 192.168.3.31, 16) => Operation not permitted
Offending packet: TCP 192.168.3.10:58035 > 192.168.3.31:9100 S ttl=41 id=38326 iplen=44  seq=2968465412 win=1024 <mss 1460>
sendto in send_ip_packet_sd: sendto(5, packet, 44, 0, 192.168.3.31, 16) => Operation not permitted
Offending packet: TCP 192.168.3.10:58036 > 192.168.3.31:9100 S ttl=40 id=56951 iplen=44  seq=2968399877 win=1024 <mss 1460>
Nmap scan report for ip-192-168-3-31.eu-west-2.compute.internal (192.168.3.31)
Host is up (0.000033s latency).
PORT     STATE    SERVICE
9100/tcp filtered jetdirect
MAC Address: 0A:F3:15:B8:34:4A (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.28 seconds
```

#2. Telnet 确认端口是否可以通信

`telnet  192.168.3.319100`


