# Linux —如何测量网络性能

> 原文：<https://blog.devgenius.io/linux-how-to-measure-network-performance-c859a98abbf0?source=collection_archive---------2----------------------->

## 测量网络性能的有用工具和指标

![](img/608d45f38ee72bde2588a15885c74159.png)

在我的上一篇文章“Linux——网络深度探讨”( T7)中，我谈到了 Linux 网络的基础知识。简单回顾一下，Linux 网络栈是基于 TCP/IP 模型的。TCP/IP 模型由五层组成:

*   应用层
*   传输层
*   网路层
*   数据链路层
*   物理层

当一个应用程序通过 socket 接口发送一个数据包时，必须在网络协议栈中自上而下进行一层一层的处理，然后最终发送到网卡进行发送；当收到一个数据包时，它必须首先从底层通过网络堆栈，逐层处理，最后发送到应用程序。

那么，如何衡量网络性能呢？您应该使用什么工具或命令？让我们来看看 Linux 网络性能的有用工具和指标。

# 网路性能

我们通常用带宽、吞吐量、延迟和每秒包数(PPS)等指标来衡量网络性能。

*   **带宽**，表示链路的最大传输速率，通常以`b/s`(比特每秒)为单位。
*   **吞吐量**，表示单位时间内成功传输的数据量，也以`b/s`为单位，或`B/s`。吞吐量受带宽限制，吞吐量/带宽是网络的使用率。
*   **延迟**，从发送网络请求到收到远程响应的时间延迟。
*   **PPS** ，每秒包数，表示网络包的传输速率。PPS 通常用于评估一个网络的转发能力，比如硬件交换机，通常可以实现线性转发(即 PPS 可以达到或接近理论最大值)。

除了这些指标，还有网络的可用性(网络是否能正常通信)、并发连接数(TCP 连接数)、丢包率(丢包百分比)、重传率(重传的网络数据包比例)等。也是常用的性能指标。

# 网络结构

分析网络问题的第一步通常是查看网络接口的配置和状态。您可以使用`ifconfig`或`ip`命令查看网络配置。`ip`该命令是首选命令，原因如下:

> ifconfig 和 ip 分别属于 net-tools 和 iproute2 包，iproute2 是下一代 net-tools。通常它们默认安装在发行版中。但是如果您找不到 ifconfig 或 ip 命令，您可以安装这两个包。

例如:

```
$ ip -s addr show dev eth0
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9001 qdisc mq state UP group default qlen 1000
    link/ether 12:08:83:db:bb:67 brd ff:ff:ff:ff:ff:ff
    inet 172.31.82.28/20 brd 172.31.95.255 scope global dynamic eth0
       valid_lft 3209sec preferred_lft 3209sec
    inet6 fe80::1008:83ff:fedb:bb67/64 scope link
       valid_lft forever preferred_lft forever
    RX: bytes  packets  errors  dropped overrun mcast
    192867565  153389   0       0       0       0
    TX: bytes  packets  errors  dropped carrier collsns
    5866543    55639    0       0       0       0
```

从输出来看，有几个指标需要注意。

*   **状态**:输出`ip`中的 LOWER_UP，表示物理网络已连接并在使用中。
*   **MTU** :网络接口上配置的最大传输单元(MTU)规定了最大的 IP 包大小。默认的 MTU 通常是 1500 字节。
*   **IP 地址/子网/MAC**
*   网络发送和接收的字节数、数据包数、错误数和数据包丢失数。

# 套接字信息

`ip`命令只显示网络接口发送和接收的数据包的统计，但在实际的性能问题中，我们还必须注意网络协议栈中的统计。您可以使用`netstat`或`ss`查看套接字、网络堆栈、网络接口和路由表信息。

我个人推荐使用`ss`来查询网络连接信息，因为它比`netstat`提供更好的性能(更快)。

例如，您可以运行以下命令来查询套接字信息:

```
# -l means how only listen sockets
# -t means show only TCP sockets
# -n means display numeric address and port
# -p means display process information\
$ ss -ltnp | head -n 3
State    Recv-Q    Send-Q        Local Address:Port        Peer Address:Port
LISTEN   0         128                 0.0.0.0:111              0.0.0.0:*        users:(("rpcbind",pid=2646,fd=8))
LISTEN   0         4096                0.0.0.0:80               0.0.0.0:*        users:(("docker-proxy",pid=3844,fd=4))
```

在上面的输出中，接收队列(Recv-Q)和发送队列(Send-Q)需要你特别注意，它们通常应该是 0。当你发现它们不为 0 时，说明有网络包的堆积。当然，还要注意，它们在不同的套接字状态下有不同的含义。

当插座处于连接状态(已建立)时，

*   Recv-Q 表示套接字缓冲区中尚未被应用程序占用的字节数(即接收队列的长度)。
*   Send-Q 表示远程主机尚未确认的字节数(即发送队列的长度)。

当套接字处于监听状态(监听)时，

*   Recv-Q 代表全连接队列的长度。
*   Send-Q 表示完全连接队列的最大长度。

# 协议栈统计

同样，使用`netstat`或`ss`，可以查看协议栈信息:

```
$ ss -s
Total: 214 (kernel 507)
TCP:   16 (estab 4, closed 4, orphaned 0, synrecv 0, timewait 2/0), ports 0Transport Total     IP        IPv6
*   507       -         -
RAW   0         0         0
UDP   8         4         4
TCP   12        9         3
INET   20        13        7
FRAG   0         0         0$ netstat -s
Ip:
    140661 total packets received
    2 with invalid addresses
    3252 forwarded
    0 incoming packets discarded
    137407 incoming packets delivered
    99885 requests sent out
    31 dropped because of missing route
Icmp:
    4 ICMP messages received
   ...
Tcp:
    3589 active connections openings
    1769 passive connection openings
    45 failed connection attempts
    25 connection resets received
    4 connections established
    129357 segments received
    88728 segments send out
    344 segments retransmited
    28 bad segments received.
    294 resets sent
    InCsumErrors: 28
Udp:
    8045 packets received
    1 packets to unknown port received.
...
```

这些堆栈的统计数据非常直观。`ss`仅显示已连接、已关闭、孤立套接字等的简要统计信息。，而`netstat`则提供了更详细的网络协议栈信息。您可以使用`ss`进行快速检查，如果您想了解更多细节，您可以使用`netstat`。

# 网络吞吐量和 PPS

要查看系统的当前网络吞吐量和 PPS，可以使用`sar`命令。在`sar`中添加`-n`参数，查看网络统计数据，如网络接口(DEV)、网络接口错误(EDEV)、TCP/UDP、ICMP 等:

```
$ sar -n DEV 1
Linux 5.10.96-90.460.amzn2.x86_64 (ip-172-31-82-28.ec2.internal)  02/20/2022  _x86_64_ (2 CPU)10:07:17 PM     IFACE   rxpck/s   txpck/s    rxkB/s    txkB/s   rxcmp/s   txcmp/s  rxmcst/s
10:07:18 PM        lo      0.00      0.00      0.00      0.00      0.00      0.00      0.00
10:07:18 PM      eth0      2.00      1.00      0.08      0.04      0.00      0.00      0.00
10:07:18 PM   docker0      0.00      0.00      0.00      0.00      0.00      0.00      0.00
10:07:18 PM vethb9669ac      0.00      0.00      0.00      0.00      0.00      0.00      0.0010:07:18 PM     IFACE   rxpck/s   txpck/s    rxkB/s    txkB/s   rxcmp/s   txcmp/s  rxmcst/s
10:07:19 PM        lo      0.00      0.00      0.00      0.00      0.00      0.00      0.00
10:07:19 PM      eth0      2.00      2.00      0.10      0.66      0.00      0.00      0.00
10:07:19 PM   docker0      0.00      0.00      0.00      0.00      0.00      0.00      0.00
10:07:19 PM vethb9669ac      0.00      0.00      0.00      0.00      0.00      0.00      0.00
```

这里有许多指标输出，你至少需要了解:

*   `rxpck/s`和`txpck/s`:接收和发送 PPS
*   `rxkB/s`和`txkB/s`:接收和发送吞吐量
*   `rxcmp/s`和`txcmp/s`:接收和发送的压缩包数量

# 连接和延迟

最后，我们通常使用`ping`来测试远程主机的连通性和延迟，这是基于 ICMP 协议的。例如:

```
$ ping -c3 google.com
PING google.com (172.217.9.206) 56(84) bytes of data.
64 bytes from iad30s14-in-f14.1e100.net (172.217.9.206): icmp_seq=1 ttl=110 time=0.817 ms
64 bytes from iad30s14-in-f14.1e100.net (172.217.9.206): icmp_seq=2 ttl=110 time=0.896 ms
64 bytes from iad30s14-in-f14.1e100.net (172.217.9.206): icmp_seq=3 ttl=110 time=0.816 ms--- google.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2017ms
rtt min/avg/max/mdev = 0.816/0.843/0.896/0.037 ms
```

ping 的输出可以分为两部分。

*   第一部分是每个 ICMP 请求的信息，包括 ICMP 序列号(icmp_seq)、TTL(生存时间或跳数)和往返延迟。
*   第二部分总结了三个 ICMP 请求。

比如上面的例子，发送 3 个网络包，收到 3 个响应，没有丢包，说明测试主机连接到 172.217.9.206；平均往返延迟(RTT)为 0.843 毫秒

# 结论

我们通常用带宽、吞吐量、延迟等指标来衡量网络性能；相应地，您可以使用`ifconfig`、`netstat`、`ss`、`sar`、`ping`等工具查看这些网络的性能指标。