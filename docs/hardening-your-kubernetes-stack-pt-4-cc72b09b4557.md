# Kubernetes 进行尺寸搜索

> 原文：<https://blog.devgenius.io/hardening-your-kubernetes-stack-pt-4-cc72b09b4557?source=collection_archive---------5----------------------->

## 利用 HPA 和 CA 扩展您的集群和应用

大多数集群不是静态的。您的应用程序的副本数量和节点数量将根据您的流量模式而变化。那么，如何根据您的需求扩展您的 Kubernetes (k8s)集群呢？K8s 提供两种资源来处理扩展，即水平 Pod 自动扩展(HPA)和集群自动扩展。两者都是 k8s 社区提供的。他们将一起根据资源利用率和分配来管理集群的大小调整。

![](img/2388e4b0f7f78c08417566889184d4df.png)

照片由 [Denys Nevozhai](https://unsplash.com/@dnevozhai?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

## **水平 Pod 自动缩放器(HPA)**

水平 pod autoscaler 是 K8s 中的一个资源，它将帮助您基于各种指标动态地增加和减少 pod 的数量。默认情况下，它将支持 CPU 和内存，但是，你也可以挂钩自定义指标。为了启用 HPA，您需要在您的 k8s 集群上安装一个[度量服务器](https://github.com/kubernetes-sigs/metrics-server)，并向您的 pod 添加资源请求和/或限制，如本系列的[第 1 部分](https://medium.com/dev-genius/hardening-your-kubernetes-stack-pt-1-29b7006b5085)中所述。HPA 遵循一个简单的算法来调整 pod 的数量:

`desiredReplicas = ceil[currentReplicas * ( currentMetricValue / desiredMetricValue )]`

HPA 循环运行，默认情况下每 15 秒重新评估一次`desireReplicas`。可以通过设置 [kube 控制器管理器](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-controller-manager/)上的`— horizontal-pod-autoscaler-sync-period`来改变周期。每隔一段时间，HPA 都会从指标服务器查看目标 pod 上的指标，以计算`currentMetricValue`。然后将`currentMetricValue`除以 pod 的资源请求`desiredMetricValue`，得到当前的资源利用率。根据当前的资源利用率和目标利用率，HPA 使用上述算法来计算所需的副本，并根据它来调整当前副本。

例子

从上面的例子中，您可以通过使用`scaleTargetRef`并定位您的部署的名称和 API 版本来定位您的部署(第 6–9 行)。HPA 将接管该部署的复制副本设置。您还可以看到，您可以在 HPA 上设置最小值和最大值(第 10 行和第 11 行),以限制复制副本的数量。然后，指标部分指定 HPA 将利用哪些指标来扩展事件。在本例中，我们同时使用了 CPU 和内存，目标利用率为 60%(第 16 行和第 20 行)。HPA 将对每个指标执行相同的计算，并选择最大的所需复制副本数量进行扩展或缩减。在缩减事件期间，如果无法计算任何指标的所需复制副本，HPA 将跳过扩展。

**总结**

启用 HPA 可让您的群集更好地利用资源，并通过根据当前资源利用率扩展或缩减应用程序来增加群集的灵活性。这意味着你的 pod 不太可能压倒你的节点，因为你的应用程序的更多实例将被旋转来处理负载，将负载分散开来。这也意味着，当您接收的流量减少时，pod 将会降低转速，以减少过度分配。

[正式文件](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/)

但是，如果您将规模扩大到由于集群没有足够的资源而无法再将 pod 分配到集群上，会发生什么情况呢？

## 集群自动缩放器

Cluster autoscaler 是另一个 k8s 社区项目。它是一个在您的集群上运行的 pod，负责根据您的集群的需求扩展和缩减节点。群集根据您的部署所请求的资源自动伸缩节点。我们用一个例子来说明这一点。

现在，您的集群有 1 个节点，1 个 vCPU 和 1Gi 内存。假设您有一个包含 2 个副本的新部署，每个副本需要 1 个 vCPU 和 500Mi 内存。在当前设置下，您没有足够的 vCPU 来运行两个副本，因此其中一个副本将陷入`Pending`状态。在您的集群上运行的 Cluster autoscaler 会注意到一个 pod 由于缺乏资源而处于`Pending`状态，并将尝试在您的集群上部署一个新节点来解决这个问题。同样，只有在您为部署设置资源请求和/或限制时，这才会起作用。

## 摘要

Cluster autoscaler 根据资源需求扩展您的集群。如果您的集群需要的资源多于它当前拥有的资源，那么它将按节点进行扩展。如果群集有未充分利用的节点，资源利用率低，并且重要的 pod 可以从这些节点中移出，那么它将被删除相当长的时间。

[正式文件](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#what-is-cluster-autoscaler)

[第 1 部分:资源请求和限制](https://medium.com/dev-genius/hardening-your-kubernetes-stack-pt-1-29b7006b5085)
[第 2 部分:Pod 服务质量和优先级](https://medium.com/dev-genius/hardening-your-kubernetes-stack-pt-2-2f2db4ff410d)
[第 3 部分:滚动更新和 Pod 中断预算](https://medium.com/dev-genius/hardening-your-kubernetes-stack-pt-3-b260d45fe6e)