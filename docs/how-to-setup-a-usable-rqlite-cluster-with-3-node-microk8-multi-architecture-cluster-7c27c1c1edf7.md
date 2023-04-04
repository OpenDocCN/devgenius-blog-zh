# 如何使用 microk8 多架构集群建立一个可用的分布式 sqlite 数据库

> 原文：<https://blog.devgenius.io/how-to-setup-a-usable-rqlite-cluster-with-3-node-microk8-multi-architecture-cluster-7c27c1c1edf7?source=collection_archive---------6----------------------->

![](img/6f1b4aee70232de8e4aa79a13b0e140a.png)

我已经建立了一个 3 节点的 microk8 集群，两个 arm64 (raspberry's)和一个 amd64。正如所料，由于大量映像不是为多架构集群构建的，因此发现了许多问题。无论如何，我想这是另一篇未来文章的主题:)

所以我决定使用 [rqlite](https://github.com/rqlite/rqlite) 作为我的家庭集群中的分布式数据库，来存储我定制的物联网应用程序。我选择 rqlite 的原因是我基本上想要像 sqlite 一样轻量级的东西，并且是分布式的(主要是由于可用性，因为我的 raspberry pi 有时会在运行几天后变得无法访问)。所以 rqlite 看起来和其他的一样好:)

我发现 rqlite 的问题是它没有准备好在 kubernetes 集群中运行。所以像 pod 调度这样的事情实际上是一个问题。幸运的是，在 Philip O'Toole(感谢队友:)的帮助下，他能够拥有正确的机制来克服这一点(你可以在这里跟随讨论[)。我遇到的第二个问题是当我想使用 microk8 存储插件时。为此，我需要某种方法在不同节点之间同步我的 statefulset 文件，这样，如果 pod1 从 node1 更改为 node2，所有数据在新节点上仍然可用。](https://github.com/rqlite/rqlite/issues/818#issuecomment-920446136)

所以我首先为 rqlite 映像和有状态集配置创建了一个 docker 配置文件。随后构建了一个存储同步应用程序，作为一个守护程序集运行，它基本上会与所有节点的状态全集(标有*synchronize-nodes*标签)卷进行同步。最后，我们需要构建一个能够更新 peers.json 文件的 recover rqlite 应用程序，以便在大多数 rqlite pods 关闭或重新安排时，consensus cluster 能够恢复。

# Docker 和有状态集配置

虽然这里有一个 docker 图片[可用](https://hub.docker.com/r/rqlite/rqlite)，但它不符合我的要求。基本上是因为所提供的映像不支持多架构(只有 amd64 ),并且集群机制不会很好地工作，因为作为领导者或加入者开始是有区别的。

总之，我创建的 docker 文件如下:

docker 本身没有什么特别的，基本上是从 rqlite github 获取源代码并构建它。主要区别在于入口点文件:

基本上，考虑到我们将作为一个有状态集运行，我们检查它是否是第一个被启动的 pod(以 0 结束),并且没有其他 pod 运行就绪，然后我们作为领导者启动，否则它作为现有运行 pod 的加入者启动。

为了构建支持多架构的 docker 映像，我使用了 buildx。所以我们只需要运行:

```
docker buildx build --platform linux/arm64/v8,linux/amd64 --tag  whatever-repo-you-want:and-version --push .
```

这个映像可以在两种架构上工作。

下一步是构建服务和有状态集配置:

我想配置是不言自明的，没有任何特别之处。要强调的要点是在 volumeClaimTemplates 上，我在那里添加了 synchronize-nodes:true 标签，以便将其标记为需要同步。下一节中描述的存储同步守护进程集将基本上过滤所有包含带有 synchronize 标签的卷的有状态集，并将这些卷同步到 k8 集群的所有节点中。

# 存储同步守护程序集

如上所述，守护程序集的主要目标是将包含标签 synchronize-nodes 的所有有状态集卷与 k8 集群的每个节点同步，以便允许有状态集被重新调度到维护其卷中数据的不同节点。

所以我构建了一个存储同步服务，它主要通过轮询 k8 的 API 来获取所有的卷声明，包括标签和各自的有界有状态集以及 rsync 各自的卷。为此，我在指向 microk8 默认存储主机的守护进程集的每个 pod 中挂载了一个卷。就我而言:

> /var/snap/micro k8s/common/default-storage/

在与绑定到具有同步标签的卷声明的运行状态集相同的 k8 节点中运行的守护程序集将负责运行 rsync 命令(在可配置的短时间间隔内),与在其他节点上运行的其他守护程序集一起向它们提供最新的存储副本。代码可以在[这里](https://github.com/paulosotu/local-storage-sync)找到。

守护进程集的配置也很简单，除了我在下一小节提到的服务帐户。您可能还注意到，需要添加两个证书作为 rsync 使用的秘密。

## 设置服务帐户

有趣的是，为了能够访问不同名称空间上的 pod，我们实际上需要为我们的守护程序设置 pod 设置一个服务帐户。如果我们不这样做，我们的 pods 将被提供默认的服务帐户，该帐户只能访问我们的默认名称空间上的 pods。要查看分配给 pod 的服务帐户，只需运行以下命令即可:

```
kubectl get pods <POD_NAME> -n <POD_NAMESPACE> -o yaml
```

然后我们会在 spec.serviceAccountName 下的 spec 中找到对它的描述。

在我们的例子中，因为我们的守护进程集 pods 运行在 kube-system 名称空间中，而我们的 statefulset pods 运行在默认名称空间中，所以我们至少需要添加对这两个名称空间的访问。虽然在我的例子中可以限制为两个名称空间，但我希望这可以在任何名称空间中与 statefulsets 一起工作(尽管我们可以在这里找到一个很好的教程来说明如何做)。所以我们的 yaml 看起来应该是这样的:

这允许我们的守护程序设置 pods 来获取/观察/列表 pods 和持久的音量声明。

# 恢复 RQLite 守护程序集

恢复 rqlite 守护程序的目标是在每次重新调度 rqlite 集群的一个节点并更改其 IP 时构建 peers.json 文件。这样，即使大多数节点被重新调度，rqlite 也可以恢复。还希望在生成新的 peers.json 后立即重新启动节点，而不更改它的 IP。为此，添加了一个基于文件活性探测器。

所以基本上我重用了为存储同步守护进程构建的存储位置服务来获取有状态集和存储位置，以便生成 json 文件。为了找出是否有任何 rqlite 节点更改了 IP，应用程序主要查看 peers.info，如果有任何节点的 IP 不同于指定的 IP，就会生成文件。如上所述，活跃度和就绪性探测由两个不同的文件管理，通过使用 rqlite API 和/或文件生成来删除或创建这两个文件，以便强制有状态集重启。代码可以在[这里](https://github.com/paulosotu/rqlite-recover)找到。

与中一样，存储同步守护程序的配置非常简单和相似。

这个设置已经运行了两个星期了，一切都像预期的那样正常。要有一个轻量级的分布式 sqlite 还需要一点工作，但这是一个很好的学习途径:)

下一步将描述我在同一个 microk8 集群中的设置或 vernemq。