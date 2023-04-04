# 所有你需要知道的开始与 ETCD 在库伯内特斯。

> 原文：<https://blog.devgenius.io/all-you-need-to-know-to-get-started-with-etcd-in-kubernetes-6755dbce8748?source=collection_archive---------18----------------------->

让我们来谈谈 ETCD 在《库伯内特》中的角色。

![](img/7a0e30ee5604463e9fc2e8b089cea493.png)

ETCD 数据存储存储有关集群组件的信息，例如:

*   节点
*   分离舱
*   配置
*   秘密
*   帐目
*   角色
*   粘合剂

运行“kubectl get”命令时显示的所有信息都来自 ETCD 服务器。您对集群所做的每一个更改，比如添加额外的节点、部署 pod 或副本集，都会在 ETCD 服务器中更新。只有当信息在 ETCD 服务器中更新时，更改才被视为完成。

根据您设置集群的方式，ETCD 的部署会有所不同。在本节中，我们将讨论两种类型的 kubernetes 部署。

1.  从头开始部署
2.  使用 kubeadm 工具进行第二次部署。

知道这两种方法的区别是很好的。

如果您从头开始设置您的集群，那么您可以通过自己下载 ETCD 二进制文件、安装二进制文件并将 ETCD 配置为主节点中的服务来部署 ETCD。

```
kubectl exec etcd-master -n kube-system -- sh -c "ETCDCTL_API=3 etcdctl get / --prefix --keys-only --limit=10 --cacert /etc/kubernetes/pki/etcd/ca.crt --cert /etc/kubernetes/pki/etcd/server.crt --key /etc/kubernetes/pki/etcd/server.key"
```

传递给服务的选项有很多。其中一些与证书有关。我们将在另一节中了解关于这些证书的更多信息。

其他是关于将 ETCD 配置为一个集群。当我们在 kubernetes 中设置高可用性时，我们将考虑这些选项。

现在唯一需要注意的选项是广告的客户端 url。这是 ETCD 监听的地址。它恰好位于服务器的 IP 地址和端口 2379 上，这是 ETCD 监听的默认端口。这是当 kube-api 服务器试图访问 etcd 服务器时应该在其上配置的 URL。

如果您使用 kubeadm 设置集群，那么它会将 ETCD 服务器作为一个 POD 部署到 kube-system 名称空间中。您可以使用此窗格中的“etcdctl 实用程序”来浏览 etcd 数据库。要列出 kubernetes 存储的所有密钥，请运行:

```
kubectl exec etcd-master -n kube-system etcdctl get / --prefix -keys-only
```

Kubernetes 将数据存储在特定的目录结构中。根目录是一个注册表，在它下面有各种各样的 kubernetes 结构，比如 minions、nodes、pods、replicasets、deployments 等等。

# 高可用性环境中的 ETCD

在高可用性环境中，集群中有多个主节点，还将有多个 ETCD 实例分布在主节点上。

在这种情况下，通过在 ETCD 服务配置中设置正确的参数，确保 ETCD 实例相互了解。

初始群集选项是您必须指定 ETCD 服务的不同实例的地方。

```
--initial-cluster controller-0=https://${CONTROLLER0_IP}:2380,controller-1=https://${CONTROLLER1_IP}:2380 \\
```

我们将在本系列的后面更详细地讨论高可用性，但是现在这个概述应该足够了。

# 有用的 ETCD 命令

ETCDCTL 是用于与 ETCD 交互的 CLI 工具。

ETCDCTL 可以使用两个 API 版本(版本 2 和版本 3)与 ETCD 服务器进行交互。默认情况下，它设置为使用版本 2。每个版本都有不同的命令集。

ETCDCTL V2 支持以下命令:

```
etcdctl backup etcdctl cluster-health etcdctl mk etcdctl mkdir etcdctl set
```

另一方面，V3 中的命令有所不同:

```
etcdctl snapshot save etcdctl endpoint health etcdctl get etcdctl put
```

要设置 API 的版本，请将环境变量 ETCDCTL_API 设置为:

```
export ETCDCTL_API=3
```

除此之外，您还必须指定证书文件的路径，以便 ETCDCTL 可以对 ETCD API 服务器进行身份验证。证书文件可在 etcd-master 的以下路径中找到。我们将在安全性一节中讨论更多关于证书的内容。因此，如果这看起来很复杂，也不用担心:

```
--cacert /etc/kubernetes/pki/etcd/ca.crt --cert /etc/kubernetes/pki/etcd/server.crt --key /etc/kubernetes/pki/etcd/server.key
```

*最初发布于*[https://static-blog-gamma . vercel . app/posts/kubernetes/core-concepts/etcd-pt-2](https://static-blog-gamma.vercel.app/posts/kubernetes/core-concepts/etcd-pt-2)