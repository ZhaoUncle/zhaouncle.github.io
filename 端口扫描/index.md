# 端口扫描和脚本批量测通


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



# 3. nc，netcat 简称

## 通

```
[root@proxy ~]# nc -vz 127.0.0.1 22
Ncat: Version 7.50 ( https://nmap.org/ncat )
Ncat: Connected to 127.0.0.1:22.
Ncat: 0 bytes sent, 0 bytes received in 0.01 seconds.
```



##不通

```
[root@proxy ~]# nc -vz 127.0.0.1 223
Ncat: Version 7.50 ( https://nmap.org/ncat )
Ncat: Connection refused.
```



# 4. wget

通的情况：

```
[root@proxy ~]# wget 127.0.0.1:22
--2021-03-22 17:19:02--  http://127.0.0.1:22/
Connecting to 127.0.0.1:22... connected.
HTTP request sent, awaiting response... 200 No headers, assuming HTTP/0.9
Length: unspecified
Saving to: ‘index.html’

    [ <=>                                                                                                                     ] 40          --.-K/s   in 0s      

2021-03-22 17:19:02 (7.47 MB/s) - Read error at byte 40 (Connection reset by peer).Retrying.

--2021-03-22 17:19:03--  (try: 2)  http://127.0.0.1:22/
Connecting to 127.0.0.1:22... connected.
HTTP request sent, awaiting response... 200 No headers, assuming HTTP/0.9
Length: unspecified
Saving to: ‘index.html’

    [ <=>                                                                                                                     ] 0           --.-K/s   in 0s      


Cannot write to ‘index.html’ (Connection reset by peer).
```

不通的情况

```
[root@proxy ~]# wget 127.0.0.1:223
--2021-03-22 17:19:20--  http://127.0.0.1:223/
Connecting to 127.0.0.1:223... failed: Connection refused.
```

# 5. ssh

## 通

```
[root@proxy ~]# ssh 127.0.0.1 -p 25
^C
[root@proxy ~]# ssh 127.0.0.1 -p 25 -v
OpenSSH_7.4p1, OpenSSL 1.0.2k-fips  26 Jan 2017
debug1: Reading configuration data /etc/ssh/ssh_config
debug1: /etc/ssh/ssh_config line 58: Applying options for *
debug1: Connecting to 127.0.0.1 [127.0.0.1] port 25.
debug1: Connection established.
debug1: permanently_set_uid: 0/0
debug1: identity file /root/.ssh/id_rsa type 1
debug1: key_load_public: No such file or directory
debug1: identity file /root/.ssh/id_rsa-cert type -1
debug1: key_load_public: No such file or directory
debug1: identity file /root/.ssh/id_dsa type -1
debug1: key_load_public: No such file or directory
debug1: identity file /root/.ssh/id_dsa-cert type -1
debug1: key_load_public: No such file or directory
debug1: identity file /root/.ssh/id_ecdsa type -1
debug1: key_load_public: No such file or directory
debug1: identity file /root/.ssh/id_ecdsa-cert type -1
debug1: key_load_public: No such file or directory
debug1: identity file /root/.ssh/id_ed25519 type -1
debug1: key_load_public: No such file or directory
debug1: identity file /root/.ssh/id_ed25519-cert type -1
debug1: Enabling compatibility mode for protocol 2.0
debug1: Local version string SSH-2.0-OpenSSH_7.4
debug1: ssh_exchange_identification: 220 proxy ESMTP Sendmail 8.14.7/8.14.7; Mon, 22 Mar 2021 17:23:40 +0800


debug1: ssh_exchange_identification: 500 5.5.1 Command unrecognized: "SSH-2.0-OpenSSH_7.4"


```



## 不通

```
[root@proxy ~]# ssh 127.0.0.1 -p 223 -v
OpenSSH_7.4p1, OpenSSL 1.0.2k-fips  26 Jan 2017
debug1: Reading configuration data /etc/ssh/ssh_config
debug1: /etc/ssh/ssh_config line 58: Applying options for *
debug1: Connecting to 127.0.0.1 [127.0.0.1] port 223.
debug1: connect to address 127.0.0.1 port 223: Connection refused
ssh: connect to host 127.0.0.1 port 223: Connection refused
```


