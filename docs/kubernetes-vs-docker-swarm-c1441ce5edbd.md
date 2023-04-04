# Kubernetes vs Docker Swarm

> 原文：<https://blog.devgenius.io/kubernetes-vs-docker-swarm-c1441ce5edbd?source=collection_archive---------30----------------------->

## 我们将会看到一系列的特性，以及 Kubernetes 和 Docker Swarm 之间的比较

![](img/5f175f49e91916aaffe24201614c2449.png)

图片由[黑客资本](https://unsplash.com/@hackcapital?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# **安装和集群配置**

**Kubernetes:**

手动设置集群非常复杂。不同操作系统之间的配置有所不同。就设置而言，它需要大量的预先规划。存储和网络等组件需要配置。像 kubectl 等第三方包是必需的。

**码头工人群:**

安装 Docker Swarm 集群很简单。它只需要几个命令就可以设置一个集群，然后添加更多的工作节点或管理节点。该设置也是独立于操作系统的，因此开发人员不必花费任何时间来学习基于操作系统的新命令。

# **负载均衡**

**Kubernetes:**

它必须手动设置，但并不复杂。入口可用于负载平衡。pod 作为服务公开。

**码头工人群:**

默认情况下会进行负载平衡，并且会自动分配端口。来自一个集群的所有容器保持在一个公共网络中。

# **可扩展性**

**Kubernetes:**

提供自动扩展到具有多个容器的大量节点。它有一个大的 API 和非常稳定的集群状态。但是，这种复杂性会导致纵向扩展时部署速度变慢。

码头工人群:

自动缩放不是现成的。但是，可以使用命令来实现这一点。容器可以比 Kubernetes 部署得更快，从而允许更快的反应时间。

# **监控和记录**

**Kubernetes:**

内置的监控和日志记录是可用的。然而，像 AppDynamics 或 Splunk 这样的工具也可以用来查看重要的指标。

**码头工人群:**

没有内置的日志工具。但是像 ELK 这样的第三方工具可以用来实现这一点。

# **联网**

**Kubernetes:**

Kubernetes 有一个扁平的网络模型。它能让所有的豆荚互相交流。策略定义了 pod 之间的通信方式。平面网络被实施为覆盖网络，需要两个 CIDRs:一个用于服务，另一个用于 IP 地址。

**码头工人群:**

为加入群的节点创建覆盖网络。该网络跨越所有主机，并且是容器的仅主机 docker 桥网络。用户可以加密容器之间的数据流量。

# **可用性**

**库伯内斯:**

Kubernetes 通过在节点之间分布 pod 来提供可用性。负载平衡器会识别出不健康的 pod 并将其停用，从而容忍应用程序出现故障

**码头工人群:**

Swarm 提供高可用性，因为所有服务都可以在节点中克隆。管理器节点管理所有资源和集群。