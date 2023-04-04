# 了解 Kubernetes 中的 YAML 文件

> 原文：<https://blog.devgenius.io/understanding-yaml-files-in-kubernetes-f0121b40abbc?source=collection_archive---------2----------------------->

![](img/351855f2586afefc9a5da578a1d3b630.png)

读完这篇文章后，你将理解使用 YAML 定义文件创建 kubernetes 对象的基础。您还将了解如何使用 YAML 文件创建 POD。

让我们开始吧。

Kubernetes 使用 YAML 文件作为创建对象的输入，例如:

-pod
-副本集
-部署
-服务

**所有这些都遵循相似的结构。**

通常，Kubernetes YAML 定义文件包含四个顶级字段:

1.apiVersion
2。种类
3。元数据
4。投机

这些是顶级或根级属性。

它们也是必填字段，因此您必须在配置文件中包含它们。

## 让我们来看看他们每一个人。

# ApiVersion

这是您用来创建对象的 kubernetes API 的版本。根据您想要创建的内容，您必须使用正确的 API 版本。

在这种情况下，因为我们正在与豆荚。我们将 apiVersion 设置为 v1。

此字段的其他一些可能值包括:

# 种类

种类是指我们试图创建的对象的类型，在本例中是一个 POD。

所以我们将它设置为一个 Pod:

其他一些可能的值有:

# [计]元数据

元数据字段包含关于对象的信息，如名称、标签等。

正如您所看到的，不像前两个，您已经指定了一个字符串值。元数据字段采用字典的形式。

元数据下的所有内容都向右缩进，因此名称和标签是元数据的“孩子”。

提示:name 和 labels 这两个属性前面的空格数无关紧要，但是它们应该相同，因为它们是兄弟。

示例:

在前面的代码片段中，labels 左边的空格比 name 多，所以它现在是 name 属性的子属性，而不是兄弟属性。这是不正确的！

在元数据下，名称是一个字符串值，因此您可以将 POD 命名为:myapp-pod。

Labels 是元数据字典中的一个字典，它可以有任意的键和值对:

您可以添加您认为合适的其他标签，这将有助于您在稍后的时间点识别这些对象。

例如，有数百个 pod 运行前端应用程序，数百个 pod 运行后端应用程序或数据库。

一旦部署完毕，您将很难对这些吊舱进行分组。

如果您将 kubernetes POD 标记为“前端”、“后端”或“数据库”，您将能够在以后的某个时间点根据此标签过滤 POD。

需要注意的是，在元数据下，你只能指定名称或标签，或者 kubernetes 期望在元数据下的任何东西。

你不能在此基础上随意添加任何其他财产。

然而，根据您的需要，在标签下您可以有任何种类的键或值对。

# 投机

配置文件中的最后一部分是规范部分，写为“spec”。

在这里，我们将向 kubernetes 提供与该对象相关的附加信息，这些信息将根据我们想要创建的对象而有所不同。

提示:参考文档部分以获得每个 kubernetes 对象的正确格式。

因为我们只创建一个包含单个容器的 pod，spec 是一个字典，所以在它下面添加一个名为 containers 的属性。

容器是一个列表或数组。该属性是一个列表的原因是因为窗格中可以有多个容器。

在这种情况下，我们将只在列表中添加一个项目，因为我们计划在 pod 中只添加一个容器。

名称前的破折号表示这是列表中的第一个项目。

清单上的项目是一本字典。所以添加一个名称和图像属性，图像的值在“nginx”中，这是 docker 存储库中 Docker 图像的名称。

创建文件后，运行以下命令，根据文件名创建一个 kubernetes 对象:

```
kubectl create -f pod-definition.yaml
```

# 摘要

回想一下四个顶级属性:

- apiVersion
-种类
-元数据
-规格

命令查看可用窗格的列表。

```
kubectl get pods
```

查看 POD 的详细信息。

```
kubectl describe pod myapp-pod
```

这将告诉您关于 pod 的信息，它是何时创建的，分配给它什么标签，以及作为它的一部分的 docker 容器和与该 POD 相关联的事件。

如果你想了解更多关于 kubernetes 的信息，我推荐你阅读[Kubernetes 集群架构简化版](https://medium.com/dev-genius/the-kubernetes-cluster-architecture-simplified-3c4a5fb41449)。

*原载于* [*我的博客*](https://luispreciado.blog/posts/kubernetes/core-concepts/pod-yaml)