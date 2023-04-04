# Linux —如何解决 DNS 问题

> 原文：<https://blog.devgenius.io/linux-how-to-troubleshoot-dns-is-babd95c76363?source=collection_archive---------1----------------------->

## 解决 DNS 问题的有用工具

![](img/6131a0478b00f4830ddaf290fe7f2ac0.png)

在我之前的文章《 [DNS 深潜](https://medium.com/geekculture/dns-deep-dive-421e321a0a06)》中，我讲过 DNS 是如何工作的，在 TCP/IP 栈中，DNS 协议属于应用层，但实际传输还是基于 UDP 或者 TCP(大部分是 UDP)，域名服务器一般监听 53 端口。

由于域名是分层结构管理的，相应的，域名解析实际上是递归的(从顶层开始，以此类推)，发送到每一层的域名服务器，直到得到解析结果。

一般来说，每一级 DNS 服务器都有一个最近解析记录的缓存。当缓存命中时，用缓存中的记录直接回复就足够了。如果缓存过期或不存在，它将进行递归查询。

因此，系统管理员在配置 Linux 系统的网络时，除了配置 IP 地址外，还需要配置 DNS 服务器，使其能够通过域名访问外部服务。例如:

```
$ cat /etc/resolv.conf
nameserver 114.114.114.114
```

# 跟踪 DNS 请求

当我们访问某个网站时，需要通过 DNS 的 A 记录查询域名对应的 IP 地址，然后通过这个 IP 访问 Web 服务。

以域名“maps.google.com”为例，执行下面的`nslookup`命令查询该域名的 A 记录:

```
$ nslookup maps.google.com
Server:  114.114.114.114
Address: 114.114.114.114#53Non-authoritative answer:
Name: maps.google.com
Address: 142.250.190.46
Name: maps.google.com
Address: 2607:f8b0:4009:80a::200e
```

这里需要注意的是，由于`114.114.114.114`并不直接管理 maps.google.com 的域名服务器，所以查询结果并不具有权威性。

除了`nslookup`，另一个常用的 DNS 解析工具是`dig`。它提供了 trace 功能，可以显示递归查询的整个过程。例如:

```
$ dig +trace +nodnssec maps.google.com; <<>> DiG 9.10.6 <<>> +trace +nodnssec maps.google.com
;; global options: +cmd
.   479254 IN NS c.root-servers.net.
.   479254 IN NS h.root-servers.net.
.   479254 IN NS m.root-servers.net.
.   479254 IN NS d.root-servers.net.
.   479254 IN NS g.root-servers.net.
.   479254 IN NS b.root-servers.net.
.   479254 IN NS f.root-servers.net.
.   479254 IN NS i.root-servers.net.
.   479254 IN NS j.root-servers.net.
.   479254 IN NS l.root-servers.net.
.   479254 IN NS e.root-servers.net.
.   479254 IN NS a.root-servers.net.
.   479254 IN NS k.root-servers.net.
;; Received 811 bytes from 192.168.1.1#53(192.168.1.1) in 7 mscom.   172800 IN NS l.gtld-servers.net.
com.   172800 IN NS b.gtld-servers.net.
com.   172800 IN NS c.gtld-servers.net.
com.   172800 IN NS d.gtld-servers.net.
com.   172800 IN NS e.gtld-servers.net.
com.   172800 IN NS f.gtld-servers.net.
com.   172800 IN NS g.gtld-servers.net.
com.   172800 IN NS a.gtld-servers.net.
com.   172800 IN NS h.gtld-servers.net.
com.   172800 IN NS i.gtld-servers.net.
com.   172800 IN NS j.gtld-servers.net.
com.   172800 IN NS k.gtld-servers.net.
com.   172800 IN NS m.gtld-servers.net.
;; Received 840 bytes from 192.5.5.241#53(f.root-servers.net) in 6 msgoogle.com.  172800 IN NS ns2.google.com.
google.com.  172800 IN NS ns1.google.com.
google.com.  172800 IN NS ns3.google.com.
google.com.  172800 IN NS ns4.google.com.
;; Received 292 bytes from 192.31.80.30#53(d.gtld-servers.net) in 13 msmaps.google.com. 300 IN A 142.250.64.110
;; Received 60 bytes from 216.239.38.10#53(ns4.google.com) in 12 ms
```

`dig`跟踪的输出解释如下:

*   第一部分是一些根域名服务器的 NS 记录(。)
*   第二部分是从 NS 记录结果中选择一个(f.root-servers.net)，查询顶级域名 com 的 NS 记录。
*   第三部分是选取 google.com 的 NS 记录之一。(d.gtld-servers.net)并查询二级域名 google.com 的 NS 服务器。
*   最后一部分是从 NS 服务器(ns4.google.com)查询最终主机 maps.google.com 的 A 记录。

此输出中显示的各级域名的 NS 记录实际上是各级域名服务器的地址，可以帮助您更清楚地了解 DNS 解析过程。

# 案例一故障排除

在这种情况下，我们将模拟 DNS 解析失败。让我们用下面的命令运行`dnsutils`容器:

```
# --rm: Automatically remove the container when it exits
$ docker run -it --rm -v $(mktemp):/etc/resolv.conf feisky/dnsutils bash
root@7c3be4ef05f3:/#
```

现在我们试着查询 maps.google.com:

```
root@7c3be4ef05f3:/# nslookup maps.google.com
;; connection timed out; no servers could be reached
```

您会发现，在该命令被长时间阻止后，它仍然会失败，报告连接超时和无法访问任何服务器错误。

你可能觉得网络没起来，是真的吗？让我们 ping 一下本地 DNS 服务器:

```
$ root@7c3be4ef05f3:/# ping 192.168.65.5
PING 192.168.65.5 (192.168.65.5): 56 data bytes
64 bytes from 192.168.65.5: icmp_seq=0 ttl=63 time=0.988 ms
64 bytes from 192.168.65.5: icmp_seq=1 ttl=63 time=0.136 ms
^C--- 192.168.65.5 ping statistics ---
2 packets transmitted, 2 packets received, 0% packet loss
round-trip min/avg/max/stddev = 0.136/0.562/0.988/0.426 ms
```

网络已经建立，但为什么`nsloopup`命令会失败？让我们启用调试模式，再试一次:

```
root@7c3be4ef05f3:/# nslookup -debug maps.google.com
;; Connection to 127.0.0.1#53(127.0.0.1) for maps.google.com failed: connection refused.
;; Connection to ::1#53(::1) for maps.google.com failed: address not available.
```

从这次的输出可以看出，nslookup 未能连接到环回地址的端口 53(127 . 0 . 0 . 1 和::1)。问题来了，为什么连接环回地址而不是实名服务器？让我们检查 DNS 配置文件:

```
root@7c3be4ef05f3:/# cat /etc/resolv.conf
```

可以看到这个`resolv.conf`文件是空的，这意味着没有配置 DNS 服务器。让我们添加 DNS 名称服务器，然后重试:

```
root@7c3be4ef05f3:/# echo "nameserver 192.168.65.5" > /etc/resolv.conf
root@7c3be4ef05f3:/# nslookup -debug maps.google.com
Server:  192.168.65.5
Address: 192.168.65.5#53Non-authoritative answer:
Name: maps.google.com
Address: 142.250.65.238
Name: maps.google.com
Address: 2607:f8b0:4006:81f::200e
```

现在问题解决了！你可以退出容器，Docker 会自动清理容器。

# 案例二故障排除

在这种情况下，我们将模拟 DNS 解析不稳定。执行以下命令:

```
$ docker run -it --rm --cap-add=NET_ADMIN --dns 8.8.8.8 feisky/dnsutils bash
root@fc61cee62a7b:/#
```

然后，和前面的情况一样，运行 nslookup 命令来解析 maps.google.com 的 IP 地址。但是，这次我们需要添加一个 time 命令来输出解析花费的时间。如果一切正常，您可能会看到如下输出:

```
/# time nslookup maps.google.com
Server:    8.8.8.8
Address:  8.8.8.8#53Non-authoritative answer:
Name: maps.google.com
Address: 142.251.35.174
Name: maps.google.com
Address: 2607:f8b0:4006:81e::200ereal  0m10.349s
user  0m0.004s
sys  0m0.0
```

如您所见，这个解析非常慢，实际上需要 10 秒钟。如果多次运行以上 nslookup 命令，您可能偶尔会遇到以下错误:

```
/# time nslookup maps.google.com
;; connection timed out; no servers could be reachedreal  0m15.011s
user  0m0.006s
sys  0m0.006s
```

换句话说，与前一种情况类似，解析失败。一般情况下，DNS 解析的结果不仅慢，还会出现超时故障。

那么，为什么会出现这种情况呢？我们遇到了什么样的问题？

其实根据前面的解释，我们知道 DNS 解析，说白了就是客户端和服务器交互的过程，这个过程也使用了 UDP 协议。

那么，对于整个过程来说，分析结果是不稳定的，有很多种可能的情况。例如:

*   DNS 服务器本身有问题，响应慢且不稳定；
*   或者，客户端到 DNS 服务器的网络延迟比较大；
*   或者，在某些情况下，链路中的网络设备会丢失 DNS 请求或响应数据包。

根据上面`nslookup`的输出，可以看到现在客户端连接的 DNS 是 8.8.8.8，是 Google 提供的 DNS 服务。我们比较放心 Google，DNS 服务器出现问题的概率应该比较小。DNS 服务器的问题基本排除。是不是第二种可能，机器到 DNS 服务器的延迟比较大？

让我们来看看延迟:

```
/# ping -c3 8.8.8.8
PING 8.8.8.8 (8.8.8.8): 56 data bytes
64 bytes from 8.8.8.8: icmp_seq=0 ttl=31 time=137.637 ms
64 bytes from 8.8.8.8: icmp_seq=1 ttl=31 time=144.743 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=31 time=138.576 ms
--- 8.8.8.8 ping statistics ---
3 packets transmitted, 3 packets received, 0% packet loss
round-trip min/avg/max/stddev = 137.637/140.319/144.743/3.152 ms
```

从 ping 输出中可以看出，这里的延迟达到了 140ms，这可以解释解析为什么这么慢。事实上，如果您多次运行上述 ping 测试，您仍然会看到偶尔的数据包丢失。

```
$ ping -c3 8.8.8.8
PING 8.8.8.8 (8.8.8.8): 56 data bytes
64 bytes from 8.8.8.8: icmp_seq=0 ttl=30 time=134.032 ms
64 bytes from 8.8.8.8: icmp_seq=1 ttl=30 time=431.458 ms
--- 8.8.8.8 ping statistics ---
3 packets transmitted, 2 packets received, 33% packet loss
round-trip min/avg/max/stddev = 134.032/282.745/431.458/148.713 ms
```

这进一步解释了为什么 nslookup 偶尔会由于网络链路中的数据包丢失而失败。

那么遇到这种问题应该怎么办呢？显然，由于延迟太大，请改用延迟较小的 DNS 服务器。其次，您可以启用 DNS 缓存以获得更好的性能。最简单的方法就是用`dnsmasq`。

`dnsmasq`是最常用的 DNS 缓存服务之一，通常用作 DHCP 服务。它的安装和配置相对简单，性能可以满足大部分应用对 DNS 缓存的需求。

要启用它，只需运行以下命令:

```
/# /etc/init.d/dnsmasq start
 * Starting DNS forwarder and DHCP server dnsmasq                    [ OK ]
```

然后修改`/etc/resolv.conf`，将 DNS 服务器改为 dnsmasq 的监听地址，这里是 127.0.0.1。然后，多次重新执行 nslookup 命令:

```
/# echo "nameserver 127.0.0.1" > /etc/resolv.conf
/# time nslookup maps.google.com
Server:    127.0.0.1
Address:  127.0.0.1#53Non-authoritative answer:
Name:  maps.google.com
Address: 142.251.35.174real  0m0.492s
user  0m0.007s
sys  0m0.006s/# time nslookup maps.google.com
Server:    127.0.0.1
Address:  127.0.0.1#53Non-authoritative answer:
Name:  maps.google.com
Address: 142.251.35.174real  0m0.011s
user  0m0.008s
sys  0m0.003s
```

现在我们可以看到只有第一次解析很慢，用了 0.5s，之后的每次解析都很快，只用了 11ms。而且每次后续 DNS 解析所需的时间也是稳定的。

# 结论

DNS 是互联网中最基础的服务，为域名和 ip 地址的映射关系提供查询服务。很多应用最初开发的时候，并没有考虑 DNS 解析的问题。后来出现了一个问题，过了好几天才发现其实是 DNS 解析慢造成的。

所以在应用开发的过程中，一定要考虑到 DNS 解析可能带来的性能问题，掌握常用的优化方法。