# Kubernetes 中的 ETCD 数据存储简介

> 原文：<https://blog.devgenius.io/an-introduction-to-the-etcd-datastore-in-kubernetes-63507d284ff6?source=collection_archive---------45----------------------->

在这篇文章的结尾，你会看到:

*   了解库伯内特斯的 ETCD 服务。
*   安装了 ETCD 服务。

我们将从什么是键值存储以及它与传统数据库有何不同的基本介绍开始。稍后我们讨论如何快速上手 ETCD 以及如何使用客户端工具进行操作。

# 那么……ETCD 是什么？

![](img/7178494420210895143e1b83dd0f8045.png)

它是一个简单、安全、快速的分布式可靠的键值存储。

# 什么是键值存储？

传统上，数据库以表格格式表示。您一定听说过 SQL 或关系数据库。它们以行和列的形式存储数据。例如，这里有一个表，存储了一些个人的信息。行代表每个人，列代表存储的信息类型。

每个键都引用一个特定的值。在本例中，每个键都引用保存在数据库中的 person 对象中的一个字段。

然后使用这个键以快速的方式从数据库中检索数据。您不能有重复的密钥。因此，它不能替代常规的表格数据库。相反，它用于存储和检索小块数据，如需要快速读写的配置数据。

# 安装 ETCD

安装和开始使用 ETCD 很容易。

```
curl -L https://github.com/etcd-io/etcd/releases/download/${ETCD_VER}/etcd-${ETCD_VER}-darwin-amd64.zip -o /tmp/etcd-${ETCD_VER}-darwin-amd64.zip unzip /tmp/etcd-${ETCD_VER}-darwin-amd64.zip -d /tmp && rm -f /tmp/etcd-${ETCD_VER}-darwin-amd64.zip mv /tmp/etcd-${ETCD_VER}-darwin-amd64/* /tmp/etcd-download-test && rm -rf mv /tmp/etcd-${ETCD_VER}-darwin-amd64
```

确保替换下载 url 以及 etcd 版本:

```
curl -L https://github.com/etcd-io/etcd/releases/download/v3.3.11/etcd-v3.3.11-darwin-amd64.zip -o /tmp/etcd-v3.3.11-darwin-amd64.zip unzip /tmp/etcd-v3.3.11-darwin-amd64.zip -d /tmp && rm -f /tmp/etcd-v3.3.11-darwin-amd64.zip mv /tmp/etcd-v3.3.11-darwin-amd64/* /tmp/etcd-download-test && rm -rf mv /tmp/etcd-v3.3.11-darwin-amd64tar xzvf etcd-v3.3.11-linux-amd64.tar.gz
```

有关安装的更多信息，请访问[完整的安装指南](https://github.com/etcd-io/etcd/releases)

当您运行 ETCD 时，它会启动一个默认监听端口 2379 的服务。

然后，您可以将任何客户端连接到 ETCD 服务来存储和检索信息。默认情况下，ETCD 附带的客户端是 ETCD 控制客户端，这是一个命令行客户端，您可以使用它来存储和检索键值对。

要存储键值对，请运行:

```
./etcdctl set key1 value1
```

这将在数据库中创建一个包含该信息的条目。

要检索存储的数据，请运行:

```
./etcdctl get key1
```

要查看更多命令选项，请运行:

这是对 ETCD 的快速介绍。

在本系列的后面，我们将讨论更多关于在高可用性设置中配置 ETCD 客户的信息，以及相关的最佳实践。

*最初发表于*[T5【http://github.com】](https://gist.github.com/luisalfonsopreciado/0ef45e3f632010627f96268485b6e055)*。*