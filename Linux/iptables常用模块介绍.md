# iptables常用模块介绍

## connlimit
connlimit模块允许你限制每个客户端ip的并发连接数，即每个ip同时连接到一个服务器个数。

connlimit模块主要可以限制内网用户的网络使用，对服务器而言则可以限制每个ip发起的连接数。

```
--connlimit-above n 限制为多少个
--connlimit-mask n 这组主机的掩码,默认是connlimit-mask 32 ,即每ip.
```

＃限制除了172.16.130.40此ip外,其他ip通过TCP访问444端口并发数为30

```
iptables -I INPUT -p tcp --dport 444 -m connlimit --connlimit-above 30 -j REJECT

iptables -I INPUT -s 172.16.130.40 -p tcp --dport 444  -j ACCEPT
```

＃允许每个客户机同时两个telnet连接

```
iptables -A INPUT -p tcp --syn --dport 23 -m connlimit --connlimit-above 2 -j REJECT
或
iptables -A INPUT -p tcp --syn --dport 23 -m connlimit ! --connlimit-above 2 -j ACCEPT
```

＃只允许每组C类ip同时16个http连接

```
iptables -p tcp --syn --dport 80 -m connlimit --connlimit-above 16 --connlimit-mask 24 -j REJECT
```

＃只允许每个ip同时5个80端口转发,超过的丢弃:

```
iptables -I FORWARD -p tcp --syn --dport 80 -m connlimit --connlimit-above 5 -j DROP
```

＃只允许每组C类ip同时10个80端口转发:
```
iptables -I FORWARD -p tcp --syn --dport 80 -m connlimit --connlimit-above 10 --connlimit-mask 24 -j DROP
```

＃为了防止DOS太多连接进来,那么可以允许最多15个初始连接,超过的丢弃.

```
iptables -A INPUT -s 192.186.1.0/24 -p tcp --syn -m connlimit --connlimit-above 15 -j DROP

iptables -A INPUT -s 192.186.1.0/24 -p tcp -m state --state ESTABLISHED,RELATED -j ACCEP
```



本文地址: http://www.cszhi.com/20120510/iptables-modules-connlimit.html


