# 每个工程师都应该学习的命令

> 原文：<https://blog.devgenius.io/kubernetes-imperative-commands-every-engineer-should-learn-3b5d8217fa29?source=collection_archive---------2----------------------->

在本文中，您将了解 kubernetes 提供的基本命令，这些命令将允许您更有效地创建和部署对象，并作为一名 DevOps 工程师脱颖而出。

![](img/b44b04924dd407d4e978c024be827ff6.png)

使用 kubernetes 时，您将主要使用 YAML 定义文件以声明的方式创建对象。

但是，命令式命令有助于快速完成一次性任务，并轻松生成定义文件模板，从而节省大量时间。

开始之前，请熟悉以下命令参数:

`--dry-run`:默认情况下，一旦执行该命令，资源就会被创建。

`--dry-run=client`:这将**而不是**创建资源。相反，kubernetes 会告诉你是否有可能召唤这个物体。

`-o yaml`:这将在屏幕上输出 YAML 格式的资源定义文件。

结合使用后两者可以方便地生成资源定义文件。稍后，您可以根据需要修改它，而不是从头开始编写文件。

例如，下面生成一个 POD 清单文件:

```
kubectl run nginx --image=nginx --dry-run=client -o yaml
```

让我们来看看用于生成 pod 的更多命令。

# Pod 命令

创建 NGINX Pod:

```
kubectl run nginx --image=nginx
```

生成 POD 清单 YAML 文件:

```
kubectl run nginx --image=nginx --dry-run=client -o yaml
```

提醒一下， **-o yaml** 参数告诉 kubectl 输出一个 yaml 文件，而 **—模拟运行**指示 kubernetes**而不是**创建 POD。

现在，让我们了解与部署相关的命令。

# 部署命令

要从命令行创建部署，请执行以下操作:

```
kubectl create deployment --image=nginx nginx
```

生成部署 YAML 文件模板:

```
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml
```

> **kubectl 创建部署**没有 **—副本**选项。话虽如此，如果您希望扩展部署，您必须首先创建它，然后使用 **kubectl scale** 命令扩展它。

以下命令将创建一个名为“nginx-deployment.yaml”的部署 YAML 定义文件:

```
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yaml
```

同样， **-o** 参数告诉 kubectl 输出一个 yaml 文件，而**>nginx-deployment . YAML**参数为 kubectl 提供所需的输出文件名。

然后，您可以根据需要修改 YAML 文件。

# 服务命令

以下命令创建一个名为“redis-service”的服务，类型为 **ClusterIP** ，目的是在端口 6379 上显示 redis pod。

```
kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml
```

> 注意:这将自动使用 POD 的标签作为选择器

备选方案:

```
kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml
```

上面的命令不会使用 POD 的标签作为选择器，而是假设选择器为 app=redis。您不能将选择器作为选项传入。或者，在创建服务**之前生成文件并修改选择器。**

以下命令创建一个名为 nginx 的 NodePort 类型的服务，以在节点上的端口 30080 上公开 nginx pod 的端口 80:

```
kubectl expose pod nginx --port=80 --name nginx-service --type=NodePort --dry-run=client -o yaml
```

> 注意:这将自动使用 pod 的标签作为选择器，但是您不能指定节点端口。您必须生成一个定义文件，然后在使用 pod 创建服务之前手动添加节点端口。

或者

```
kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml
```

这不会将 pod 的标签用作选择器。

最后几个命令有自己的挑战:一个不能接受选择器，另一个不能接受节点端口。在我看来，我建议使用`kubectl expose`命令。

如果需要指定节点端口，请使用相同的命令生成一个定义文件，并在创建服务之前手动输入节点端口。

如果你喜欢这个教程，我建议你看看我在 kubernetes 上关于 YAML 文件的帖子。

[](https://medium.com/dev-genius/understanding-yaml-files-in-kubernetes-f0121b40abbc) [## 了解 Kubernetes 中的 YAML 文件

### 读完这篇文章后，你将理解使用 YAML 定义文件创建 kubernetes 对象的基础。你…

medium.com](https://medium.com/dev-genius/understanding-yaml-files-in-kubernetes-f0121b40abbc) 

*原载于*[*https://luispreciado . blog*](https://luispreciado.blog/posts/kubernetes/core-concepts/imperative-commands)*。*