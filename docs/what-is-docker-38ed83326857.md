# Docker 是什么？

> 原文：<https://blog.devgenius.io/what-is-docker-38ed83326857?source=collection_archive---------5----------------------->

![](img/9fd6889977b61b764a713f0052bc5524.png)

在您的技术之旅中，我相信您一定遇到过术语“Docker”。今天我们将深入探讨 docker 是什么以及为什么要使用 Docker。解决 docker 是什么的问题。根据官方文件:

> Docker 是一个开发、发布和运行应用程序的开放平台。Docker 使您能够将应用程序从基础设施中分离出来，这样您就可以快速交付软件。

嗯，这个定义中有很多东西需要解开。因此，从定义中，你可能会有这样的想法，docker 在开发过程之后进入画面，也就是说，你完成了你的软件，docker 在某种程度上帮助你完成了后续工作。在了解 docker 是什么之前，我们先来看一个问题场景。比方说，你想学习 Redis，为此，你必须在本地运行 Redis 服务器。你可以使用我在文章[https://archita 810 . medium . com/getting-hands-dirty-with-redis-db 4191 CD 1 f 8](https://archita810.medium.com/getting-hands-dirty-with-redis-db4191cd1f8c)c 中描述的冗长过程来完成，但是如果我用 docker 告诉你，它可以用一个简单的命令来完成。

```
Archi % docker run redis
1:C 12 Dec 2021 14:18:10.106 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
1:C 12 Dec 2021 14:18:10.106 # Redis version=6.2.6, bits=64, commit=00000000, modified=0, pid=1, just started
1:C 12 Dec 2021 14:18:10.106 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
1:M 12 Dec 2021 14:18:10.106 * monotonic clock: POSIX clock_gettime
1:M 12 Dec 2021 14:18:10.107 * Running mode=standalone, port=6379.
1:M 12 Dec 2021 14:18:10.107 # Server initializer
1:M 12 Dec 2021 14:18:10.108 * Ready to accept connections
```

只需一个简单的命令，我的服务器就已经启动了。这就是 docker 所做的，它将软件的所有要求存储在一个映像中(我们稍后将探讨什么是映像)，然后我们可以启动我们的容器(也是这个),它将处理在后端运行软件所需的所有依赖关系，使安装软件的过程变得更加容易。还记得有多少次你开始安装软件，却发现缺少了一些依赖项，然后又是另一个，仅仅安装软件就变成了一个乏味的部分，所以这就是定义中的“Docker 使你能够将你的应用程序与你的基础设施分离，这样你就可以快速交付软件”所指的，因为映像列出了所有的依赖项，注意它没有安装，只是一个关于当你运行映像时如何安装它的说明。 内核也由 docker 负责，所以运行可能需要 Linux 环境但你有 windows one 的软件变得更容易。
因为图片是轻量级的，所以很容易分享。容器运行在一个隔离的环境中，所以我们可以在一台主机上运行多个容器，一个容器不会干扰另一个容器的资源。我认为这基本上涵盖了什么是 docker 以及我们为什么使用 docker。回到我们之前遇到的两个术语，什么是图像，什么是容器。

# 图像

码头工人为我们做的一切都不是魔法。一些指令必须给 docker，一旦我们运行图像，图像服务器应该做什么。它只是一个模板，指示码头工人应该做什么。我们既可以创建自己的图像，也可以使用目前互联网上的众多图像。当我们在上面运行命令‘docker run redis’时，我们实际上是在运行一个名为 Redis 的映像，它目前在互联网上可用。Docker hub 维护着当前可用的所有此类图像的列表。在这里，你可以自由探索 https://hub.docker.com。

# 容器

一个图像的运行实例被称为容器，所以我们运行命令“docker run redis ”,作为回报，一个容器被创建。我们可以看到，使用下面的命令:

```
Archi % docker psCONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES
410b6db07e00 redis “docker-entrypoint.s…” 40 minutes ago Up 40 minutes 6379/tcp naughty_varahamihira
```

如您所见，我们已经使用图像 redis 创建了一个容器，我们可以看到与容器相关联的 id 和一些其他信息。每个容器在该容器可访问的硬盘驱动器、网络上具有专用空间，并且一旦该容器停止，所执行的改变就消失，即容器的状态不再持续。

以上是对 docker 的简要概述，在下一篇文章中，我们将安装 docker，探索一些 Docker 命令并创建我们自己的映像。如果你觉得这有帮助，别忘了鼓掌。