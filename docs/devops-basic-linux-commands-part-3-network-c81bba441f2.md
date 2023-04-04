# DevOps —基本 Linux 命令第 3 部分，网络

> 原文：<https://blog.devgenius.io/devops-basic-linux-commands-part-3-network-c81bba441f2?source=collection_archive---------7----------------------->

## 网络故障排除

![](img/78ecd5683a25b3d4615b2c856e6e025a.png)

一般网络配置和故障排除是 DevOps 日常工作中必不可少的部分。即使对于使用 Linux 系统的开发人员来说，了解 Linux 网络命令**也是一个额外的优势。**

在本文中，我将向您展示一些用于排除网络问题的常用 Linux 命令。

# 主机名

`hostname`:获取 DNS(域名系统)名称，设置系统主机名或 NIS(网络信息系统)域名。主机名是赋予计算机的名称，它连接到网络。它的主要目的是在网络上唯一地识别。

常见用法:

```
$ hostname -a ==> Get alias name of the host system
$ hostname -A ==> Get all FQDNs
$ hostname -f ==> Get the FQDN. Contains short hostname and DNS domain name
$ hostname -d ==> Get the domain if local domains are set
$ hostname -F file_name ==> Set hostname from file
$ hostname -i ==> Get IP
$ hostname -I ==> Get all IPs
```

# 砰

`ping`:用于检查主机之间的网络连通性。

常见用法:

```
$ ping google.com ==> Check connectivity to google.com
$ ping -c 5 google.com ==> Send only 5 packets
$ ping -s 40 google.com ==> Set packet size
$ ping -i 2 google.com ==> Change time interval to 2 sec
$ ping -c 2 -q google.com ==> Only get summary
$ ping -f google.com ==> Send packets as soon as possible
```

# 宿主

`host`:获取主机 DNS 详细信息。查找特定域名的 IP 地址。

常见用法:

```
$ host google.com ==> Get IP(s) of google.com
$ host ip_address ==> Get domain details of ip_address
$ host -v google.com ==> Enable verbose output
$ host -t ns google.com ==> Get name servers
$ host -t mx google.com ==> Get mail servers
$ host -R 3 google.com ==> Specify number of retries
```

# 卷曲

`curl`:用于传输数据的跨平台实用程序。它可用于解决几个网络问题。

常见用法:

```
$ curl [https://url](https://url) ==> Display content of url
$ curl -# -O [https://url](https://url) ==> Show progress meter
$ curl -o file [https://url](https://url) ==> Save to local file
$ curl -O [https://url](https://url) ==> Save the file with the same name as in url
$ curl -C - -P [https://url](https://url) ==> Resume download that was interrupted
$ curl --limit-rate 1000K -O ==> Limit download to 1000KB
$ curl -u demo:password ==> Download with authentication
$ curl -x [proxy_name]:[port] ==> Set proxy
$ curl --noproxy "google.com" url ==> Not to use proxy for google.com
```

# 互联网协议（Internet Protocol 的缩写）

`ip`:ifconfig 的替代品。可用于配置和检索有关系统网络接口的信息。

常见用法:

```
$ ip addr ==> Show all interface information
$ ip addr show eth1 ==> Show eht1 interface information
$ ip link ==> Display link layer information
$ ip -s link ==> Show link statistics as well
$ ip route ==> Show route information
$ ip a add/del 192.168.1.50/24 dev enp3s0 ==> Add/delete IP to an interface
$ ip link set eth1 up/down ==> Enable/disable eth1
$ ip neighbour ==> View MAC address of the devices
$ ip neighbour add/del (ip_address) dev eth1 ==> Modify ARP entry
```

# traceroute

`traceroute`:使用 ICMP 协议，查找读取目的服务器所涉及的跳数。它还显示了跳之间所用的时间。

常见用法:

```
$ traceroute google.com ==> Print hop info
$ traceroute -4 google.com ==> IPv4 hop info
$ traceroute -6 google.com ==> TPv6 hop info
$ traceroute -f 5 google.com ==> Start from 5th hop
$ traceroute -g 192.168.33.24 google.com ==> Route packet trhpugh gate
$ traceroute -m 5 google.com ==> Set max hops to 5
$ traceroute -n google.com ==> Do not resolve IP to domains
$ traceroute -p 20211 google.com ==> Set dest port to use, default is 33434
```

# 挖苦

`dig`:帮助您获取与域名相关的 DNS 记录。

常见用法:

```
$ dig google.com ==> Query domain A record
$ dig google.com +short ==> Compact information
$ dig google.com +nocomments ==> Remoev comments
$ dig google.com ANY ==> Query all DNS record
$ dig google.com MX ==> Query MX record
$ dig google.com +trace ==> Enable trace info
$ dig google.com @8.8.8.8 ==> Specify name servers
$ dig google.com +noall +answer +stats ==> Query statistics section
```

# 网络管理命令行工具

`nslookup`:检查 DNS 条目。它类似于 dig 命令。

常见用法:

```
$ nslookup google.com ==> Query domain and get details
$ nslookup 172.217.167.174 ==> Reverse DNS lookup
$ nslookup -type=any google.com ==> Lookup for any record
$ nslookup -type=MX google.com ==> Lookup for MX record
```

# netcat (nc)

`nc`:强大的联网工具、安全工具、网络监控工具。

常见用法:

```
$ nc -l -p 1234 ==> Listing on port 1234
$ nc 168.134.12.34 1234 ==> Connect to IP:port
$ nc -z -v 127.0.0.1 22 ==> Scanning single port
$ nc -z -v 127.0.0.1 22 23 ==> Scanning multi ports
$ nc -z -v 127.0.0.1 22-244 ==> Scanning multi ports by range
```

# tcpdump

`tcpdump`:用于捕获、过滤和分析网络流量。

常见用法:

```
$ tcpdump -i eth1 ==> Capture packets from interface eth1
$ tcpdump -n -i eth1 ==> Capture from eth1 with IPs
$ tcpdump -i eth1 tcp ==> Capture only tcp packets
$ tcpdump -c 5 -i eth1 ==> Capture only 5 packets
$ tcpdump -A -i eth1 ==> Capture packets in ASCII format
$ tcpdump -D ==> Display all available interfaces
$ tcpdump -XX -i eht1 ==> Display packets in both HEX and ASCII format
$ tcpdump -w captured.pcap -i eth1 ==> Save captured packets to file
$ tcpdump -r captured.pcap ==> Read in captured file
```

# nmap

`nmap`:用于做网络探测和安全审计。

常见用法:

```
$ nmap google.com ==> Scan with hostname
$ namp 142.250.31.138 ==> Scan with IP
$ nmap -v google.com ==> Show verbose output
$ nmap ip1 ip2 ip3 ==> Scan multiple host
$ nmap 103.76.228.* ==> Scan whole subnet
$ nmap -sA ip ==> Scan to detect firewall settings
$ nmap -iL input.txt ==> Scan from file
$ namp -A ip ==> Scan aggressively provides more valuable info
```