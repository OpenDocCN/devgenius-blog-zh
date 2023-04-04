# 对接码头集装箱第 3 部分:联网

> 原文：<https://blog.devgenius.io/docking-a-docker-container-part-3-networking-edd779f068cb?source=collection_archive---------0----------------------->

> 帮助弱者！打击网络犯罪[了解如何](https://forms.gle/JWAPHzf2gd7jGq2YA)。

在[之前的](https://medium.com/@tomdeore/docking-a-docker-container-part-2-c1206e7c6677#.emitlalza)会话中，我们看到 namespace 和 cgroup 为容器运行创建了一个执行环境。将它们缝合在一起另一个重要部分是网络。当您实际部署您的生产应用程序时，它必须有多个容器，但是这些容器如何相互通信呢？容器框架的 IP 架构应该是怎样的？

# docker 网络是如何工作的？

默认情况下，当您启动 docker 守护程序时，docker 会创建三个网络，即:

1.  [桥](https://docs.docker.com/network/bridge/#:~:text=In%20terms%20of%20Docker%2C%20a,connected%20to%20that%20bridge%20network.)，
2.  没有，
3.  主持人。

```
**[root@docket ]** docker network ls
NETWORK ID          NAME                DRIVER
8b169e0c38fd        **none**                null                
0422d120f29c        **host**                host                
19e614e5beec        **bridge**              bridge **[root@docker ]** docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
**3673e651a804**        ubuntu              "/bin/bash"         16 hours ago        Up 16 hours                             rcv
**148edfe98193**        ubuntu              "/bin/bash"         16 hours ago        Up 16 hours                             src**[root@docker ]** docker network inspect bridge
[
    {
        "Name": "bridge",
        "Id": "19e614e5beeca0bbb73907f37c3ccb81893349a91031264a3610e7a1c158c28d",
        "Scope": "local",
        "Driver": "bridge",
        "IPAM": {
            "Driver": "default",
            "Config": [
                {
                    "Subnet": "172.17.160.1/19",
                    "Gateway": "172.17.160.1"
                }
            ]
        },
        "**Containers**": {
            "**148edfe981937f3f8b15b40e1088273aafaeb9bd57d03a3f102ba6248a9f6575**": {
                "EndpointID": "8c921a07b26255535f97045f9f7ce4a4d379f471c139d8d7837ed70d9d94a0b5",
                "MacAddress": "02:42:ac:11:a0:02",
                "IPv4Address": "**172.17.160.2**/19",
                "IPv6Address": ""
            },
            "**3673e651a804c872584a588d8e39d95cba7219d02541266a8bf4fe903696bb1c**": {
                "EndpointID": "249d1edf5f05304d89652cb10839d2bb731d964e32635f164829fd3702398065",
                "MacAddress": "02:42:ac:11:a0:03",
                "IPv4Address": "**172.17.160.3**/19",
                "IPv6Address": ""
            }
        },
        "Options": {
            "com.docker.network.bridge.default_bridge": "true",
            "com.docker.network.bridge.enable_icc": "true",
            "com.docker.network.bridge.enable_ip_masquerade": "true",
            "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
            "com.docker.network.bridge.name": "docker0",
            "com.docker.network.driver.mtu": "1500"
        }
    }
]**[root@docker ]** docker network inspect none
[
    {
        "Name": "none",
        "Id": "8b169e0c38fdc80337f4e38ab1e89328fb79f260c501ffadad8ff1141e2384f0",
        "Scope": "local",
        "Driver": "null",
        "IPAM": {
            "Driver": "default",
            "Config": []
        },
        "Containers": {},
        "Options": {}
    }
]**[root@docker ]** docker network inspect host
[
    {
        "Name": "host",
        "Id": "0422d120f29cd0a63952fdf5bc41dde7004bc34e4140458ea8ed90b1c60e4bde",
        "Scope": "local",
        "Driver": "host",
        "IPAM": {
            "Driver": "default",
            "Config": []
        },
        "Containers": {},
        "Options": {}
    }
]
```

查看上面的输出，这两个容器被列在 network ' **bridge** 和 ***下。默认情况下，网桥网络下的任何容器都可以通过它们的 IP 地址到达(PINGable)，但不能通过它们的名称*** `***src***` ***和*** `***rcv***`到达。容器也可以使用它们的名字来相互通信，但是为此，它们需要使用——`link`选项(我们稍后会谈到)。

![](img/bde0f511c7b76aff90b8a0ccef885135.png)

与集装箱对接的码头桥

此图显示了将主机上的物理以太网接口连接到 docker 容器的网桥接口。Docker 守护进程拥有一系列来自网桥接口所在子网的 IP 地址，并将其分配给每个新容器，它使用基于 IP 地址的命名。紧逼也隔离了容器与外界的交流，它们只能通过一个桥与外界联系。默认情况下，docker 守护进程中的所有容器都可以相互对话，这是通过 docker 守护进程标志`— icc=true`(容器间通信)来完成的。还有另一个值得一提的标志`— iptables`，它覆盖了`— icc`功能，它阻止 docker 守护进程在`— iptables=false`时对`iptables` 进行更改。

现在，在生产环境中，您肯定会设置适当的寻址方案来避免干扰和最有可能的[私有 IP 寻址](https://en.wikipedia.org/wiki/Private_network)。在本例中，我们为网关桥和其中所有的 NAT 主机使用了 B 类地址。默认情况下，docker 根据 [RFC1918](https://tools.ietf.org/html/rfc1918) 设置一个私有 IP 地址，但有时您可能需要设置自己的私有 IP 方案，此时您需要更改默认行为并设置网桥 IP 地址，以获得更多[选项](https://docs.docker.com/engine/userguide/networking/default_network/custom-docker0/):

```
**[root@docker$ ]** service docker stop 
**[root@docker$** **]** vim /etc/default/dockerDOCKER_OPTS="**--bip=172.16.160.1/20**"**[root@docker$** **]** service docker start
**[root@docker$** **]** ifconfig
docker0   Link encap:Ethernet  HWaddr 02:42:64:0d:8b:b6  
          inet addr:**172.16.160.1**  Bcast:0.0.0.0  Mask:255.255.224.0
          UP BROADCAST MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)eth0      Link encap:Ethernet  HWaddr 08:00:27:44:20:d8  
          inet addr:10.0.2.15  Bcast:10.0.2.255  Mask:255.255.255.0
          inet6 addr: fe80::a00:27ff:fe44:20d8/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:63 errors:0 dropped:0 overruns:0 frame:0
          TX packets:112 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:16848 (16.8 KB)  TX bytes:14347 (14.3 KB)lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:69 errors:0 dropped:0 overruns:0 frame:0
          TX packets:69 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:7229 (7.2 KB)  TX bytes:7229 (7.2 KB)
```

现在，如果您启动这个守护进程中的任何容器，它将自动从上述 IP 地址子网范围中获取 IP 地址。示例:

```
**root@9d51b764a3c1:/#** ifconfig
eth0      Link encap:Ethernet  HWaddr 02:42:ac:11:a0:02  
          inet addr:**172.16.160.2**  Bcast:0.0.0.0  Mask:255.255.224.0
          inet6 addr: fe80::42:acff:fe11:a002/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:10863 errors:0 dropped:0 overruns:0 frame:0
          TX packets:10767 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:25248347 (25.2 MB)  TX bytes:586961 (586.9 KB)lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)
```

同样，有时我们希望这些容器有一个特定的静态 IP 地址(当然是来自相同的子网范围，但是是我们自己选择的，而不是 docker 守护进程分配的随机地址)。有一种方法可以做到这一点:

*   **容器运行时间**:运行容器后，我们可以在`/var/lib/docker/containers/<container ID>`修改存储网络设置的配置文件。但是这可能不是很有用，因为这种网络设置通常需要在启动时给出，因为运行时是易变的，并且可能在下一次启动时消失。除了在启动时，我找不到任何其他合适的方法给容器分配特定的 IP 地址。

```
**[root@docker ]** docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
708290c3743e        centos              "/bin/bash"         35 minutes ago      Up 35 minutes                           silly_visvesvaraya
9d51b764a3c1        ubuntu              "/bin/bash"         39 minutes ago      Up 39 minutes                           small_jang**[root@docker ]** ls -l /var/lib/docker/containers/9d51b764a3c1034585f61a2d8156702b8afbcf258f510d5c93bde91d82d7767d/
total 60
-rw------- 1 root root 36856 Nov  6 23:12 9d51b764a3c1034585f61a2d8156702b8afbcf258f510d5c93bde91d82d7767d-json.log
-rw-r--r-- 1 root root  2398 Nov  6 22:55 config.json
-rw-r--r-- 1 root root   720 Nov  6 22:55 hostconfig.json
-rw-r--r-- 1 root root    13 Nov  6 22:55 hostname
-rw-r--r-- 1 root root   176 Nov  6 22:55 **hosts**
drwxrwxrwt 2 root root    40 Nov  6 22:38 mqueue
-rw-r--r-- 1 root root   190 Nov  6 22:55 **resolv**.**conf**
-rw------- 1 root root    71 Nov  6 22:55 resolv.conf.hash
drwxrwxrwt 2 root root    40 Nov  6 22:55 shm
```

# **链接容器**

在 docker 主机/守护进程中连接容器的最佳方式称为“**链接容器**”。最安全的通信方式，但缺点是它只能在容器间通信，而不能与外界通信。“在某种程度上，这是塞翁失马，焉知非福”。

```
**[root@docker ]** docker run —name=src -d <source container image>**[root@docker ]** docker run —name=rcvr —link=src:alias-src -d <receiver container image>**[root@docker ]** docker inspect rcvr
........
“Links”: [
     “/src:/src/alias-src”
........**[root@docker ]** docker inspect src | grep Links
“Links”: null,
```

这是因为源容器为接收容器提供了相当多的环境变量。它还在/etc/hosts 文件中添加了一个条目，如下所示:

```
**root@1c8b2b26e9fa:/#**  env | grep ALIAS
ALIAS_SRC_PORT_80_TCP_ADDR=172.16.160.2
ALIAS_SRC_PORT_80_TCP_PROTO=tcp
ALIAS_SRC_PORT_80_TCP_PORT=80
ALIAS_SRC_PORT=tcp://172.16.160.2:80
ALIAS_SRC_NAME=/rcvr/alias-src
ALIAS_SRC_PORT_80_TCP=tcp://172.16.160.2:80**root@1c8b2b26e9fa:/#** cat /etc/hosts
172.16.160.3       1c8b2b26e9fa
127.0.0.1          localhost
::1  localhost ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
172.16.160.2        alias-rcvr
```

还有一点很重要，即“src”和“rcvr”都可以相互通信，这意味着它创建了一个双向通道。同样，一个源可以连接到许多接收器，同样，一个接收器可以链接到许多源。

# 将端口暴露给外部世界

这是容器与外部世界之间最常见的通信方式，而对于外部世界与容器的通信来说，这是一种更好的方式，因为这里我们只公开端口，而不公开 IP 地址。这也意味着 docker 主机上的任何物理接口都可以接受传入的数据包，因为 iptable 将规则设置为 **0.0.0.0:80** ，让我们举个例子:

```
**[root@docker ]** vim Dockefile.........
EXPOSE 80
.........**[root@docker ]** docker build -t="apache-img" .
```

在应用程序中使用 EXPOSEing 端口号构建映像后，使用`-p`选项运行映像，并指定:**{ docker 主机的传入端口:docker 容器端口}。**

```
**[root@docker ]** docker run -d -p 5010:80 apache-img**[root@docker ]** docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
9d51b764a3c1        apache-img          "/bin/bash"         2 hours ago         Up 2 hours                  0.0.0.0:5010->80/tcp            small_jang**[root@docker ]** docker port small_jang
80/tcp -> 0.0.0.0:5010
```

这意味着来自端口 5010 的 0.0.0.0(任何接口)上的 docker 主机接口的所有数据包将进入已经暴露端口 80 的 docker 容器的端口 80。

特定接口/IP 地址也可以是特定的，而不是 0.0.0.0，这可以使用:

```
**[root@docker ]** docker run -d -p 192.168.56.5:5010:80 apache-img**[root@docker ]** docker port small_jang
80/tcp -> 192.168.56.5:5010
```

其中，假设 [http://192.168.56.5:5010](http://192.168.56.5:5010) 请求将到达运行在容器中的 apache 服务器的端口 80。

# 在我说“再见”之前

默认网桥将允许[容器到容器的通信](https://docs.docker.com/engine/userguide/networking/default_network/container-communication/)使用它们的 IP 地址(由守护程序动态分配),这变得难以知道容器的 IP，并且更难以编程(重启将不能保证相同的 IP 将被分配回)。在这种情况下，最好的选择是链接容器，即通过它们的名称，在启动之前你就知道容器的名称，并且易于编程。因此，你可能想关闭默认的 IP 地址为基础的通信是不是？这可以通过以下方式实现

```
--icc=false
```

在这之后，你可以连接你的容器，世界将会变得更加安全。

# 结论

没有硬性规定，但是公开端口最适合与外部世界通信，而链接容器最适合容器之间的内部通信。这也是一个隐藏的局域网，与外界隔绝，因此也受到保护。

**你可以在|**[**LinkedIn**](https://www.linkedin.com/in/mdeore/)**|**[**网站**](https://tomdeore.wixsite.com/epoch)**|**[**Github**](https://github.com/milinddeore)**|**