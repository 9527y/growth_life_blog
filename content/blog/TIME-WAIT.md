---
title: Linux系统消除未被及时释放的TIME_WAIT状态的TCP连接
categories:
  - 运维
tags:
  - Linux
date: 2021-04-09 15:13:09
---



Linux系统下，TCP连接断开后，会以TIME_WAIT状态保留一定的时间，然后才会释放端口。当并发请求过多的时候，就会产生大量的TIME_WAIT状态的连接，无法及时断开的话，会占用大量的端口资源和服务器资源。这个时候我们可以优化TCP的内核参数，来及时将TIME_WAIT状态的端口清理掉。

```sh
[root@web1 ~]netstat -n | awk '/^tcp/ {++S[$NF]} END {for(a in S) print a, S[a]}'
TIME_WAIT 1280
FIN_WAIT1 7
SYN_SENT 1
FIN_WAIT2 7
ESTABLISHED 247
LAST_ACK 1
```

我们只用关心TIME_WAIT的个数，在这里可以看到，有1280多个TIME_WAIT，这样就占用了1280多个端口，端口的数量只有65535个，占用一个少一个，会严重的影响到后继的新连接，就需调整下Linux的TCP内核参数，让系统更快的释放TIME_WAIT连接。

vi /etc/sysctl.conf 增加如下内容：

```sh
net.ipv4.tcp_syncookies = 1   #表示开启SYN Cookies。当出现SYN等待队列溢出时，启用cookies来处理，可防范少量SYN攻击，默认为0，表示关闭；
net.ipv4.tcp_tw_reuse = 1    #表示开启重用。允许将TIME-WAIT sockets重新用于新的TCP连接，默认为0，表示关闭；
net.ipv4.tcp_tw_recycle = 1  #表示开启TCP连接中TIME-WAIT sockets的快速回收，默认为0，表示关闭；
net.ipv4.tcp_fin_timeout = 30    #修改系統默认的 TIMEOUT 时间。
```

sysctl -p 让内核参数生效
流量小的机器一般就可以了，如果是大流量机器增加如下参数：

```conf
net.ipv4.tcp_keepalive_time = 1200   #表示当keepalive起用的时候，TCP发送keepalive消息的频度。缺省是2小时，改为20分钟。
net.ipv4.ip_local_port_range = 10000 65000   #表示用于向外连接的端口范围。缺省情况下很小：32768到61000，改为10000到65000。（注意：这里不要将最低值设的太低，否则可能会占用掉正常的端口！）
net.ipv4.tcp_max_syn_backlog = 8192  #表示SYN队列的长度，默认为1024，加大队列长度为8192，可以容纳更多等待连接的网络连接数。
net.ipv4.tcp_max_tw_buckets = 6000  #表示系统同时保持TIME_WAIT的最大数量，如果超过这个数字，TIME_WAIT将立刻被清除并打印警告信息。默认180000，改为6000。对于Apache、Nginx等服务器，上几行的参数可以很好地减少TIME_WAIT套接字数量，但是对于 Squid，效果却不大。此项参数可以控制TIME_WAIT的最大数量，避免Squid服务器被大量的TIME_WAIT拖死。

内核其他TCP参数说明：
net.ipv4.tcp_max_syn_backlog = 65536  #记录的那些尚未收到客户端确认信息的连接请求的最大值。对于有128M内存的系统而言，缺省值是1024，小内存的系统则是128。
net.core.netdev_max_backlog = 32768   #每个网络接口接收数据包的速率比内核处理这些包的速率快时，允许送到队列的数据包的最大数目。
net.core.somaxconn = 32768   #web应用中listen函数的backlog默认会给我们内核参数的net.core.somaxconn限制到128，而nginx定义的NGX_LISTEN_BACKLOG默认为511，所以有必要调整这个值。

net.core.wmem_default = 8388608
net.core.rmem_default = 8388608
net.core.rmem_max = 16777216           #最大socket读buffer,可参考的优化值:873200
net.core.wmem_max = 16777216           #最大socket写buffer,可参考的优化值:873200
net.ipv4.tcp_timestsmps = 0    #时间戳可以避免序列号的卷绕。一个1Gbps的链路肯定会遇到以前用过的序列号。时间戳能够让内核接受这种“异常”的数据包。这里需要将其关掉。
net.ipv4.tcp_synack_retries = 2   #为了打开对端的连接，内核需要发送一个SYN并附带一个回应前面一个SYN的ACK。也就是所谓三次握手中的第二次握手。这个设置决定了内核放弃连接之前发送SYN+ACK包的数量。
net.ipv4.tcp_syn_retries = 2  #在内核放弃建立连接之前发送SYN包的数量。
net.ipv4.tcp_tw_reuse = 1     # 开启重用。允许将TIME-WAIT sockets重新用于新的TCP连接。

net.ipv4.tcp_wmem = 8192 436600 873200   # TCP写buffer,可参考的优化值: 8192 436600 873200
net.ipv4.tcp_rmem  = 32768 436600 873200  # TCP读buffer,可参考的优化值: 32768 436600 873200
net.ipv4.tcp_mem = 94500000 91500000 92700000  # 同样有3个值,意思是:
net.ipv4.tcp_mem[0]:低于此值，TCP没有内存压力。
net.ipv4.tcp_mem[1]:在此值下，进入内存压力阶段。
net.ipv4.tcp_mem[2]:高于此值，TCP拒绝分配socket。
上述内存单位是页，而不是字节。可参考的优化值是:786432 1048576 1572864

net.ipv4.tcp_max_orphans = 3276800   #系统中最多有多少个TCP套接字不被关联到任何一个用户文件句柄上。
如果超过这个数字，连接将即刻被复位并打印出警告信息。
这个限制仅仅是为了防止简单的DoS攻击，不能过分依靠它或者人为地减小这个值，
更应该增加这个值(如果增加了内存之后)。
net.ipv4.tcp_fin_timeout = 30   #如果套接字由本端要求关闭，这个参数决定了它保持在FIN-WAIT-2状态的时间。对端可以出错并永远不关闭连接，甚至意外当机。缺省值是60秒。2.2 内核的通常值是180秒，你可以按这个设置，但要记住的是，即使你的机器是一个轻载的WEB服务器，也有因为大量的死套接字而内存溢出的风险，FIN- WAIT-2的危险性比FIN-WAIT-1要小，因为它最多只能吃掉1.5K内存，但是它们的生存期长些。
```

具体需根据服务器性能配置高低调优，最后sysctl -p 生效；