# Kubernetes 的 VIP(非常重要的豆荚)

> 原文：<https://blog.devgenius.io/hardening-your-kubernetes-stack-pt-2-2f2db4ff410d?source=collection_archive---------5----------------------->

## Pod 服务质量和 Pod 优先级

强化 Kubernetes (k8s)堆栈的一部分是确保关键应用程序优先于不太关键的应用程序。K8s 提供了几种方法来实现这一点。通过利用 pod 服务质量和 pod 优先级，您可以分别控制如何终止和调度 pod。这是上一篇强化文章的延续:[https://medium . com/dev-genius/hardening-your-kubernetes-stack-pt-1-29b 7006 b 5085](https://medium.com/dev-genius/hardening-your-kubernetes-stack-pt-1-29b7006b5085)

![](img/63c9bcb3e496c9583451d5c1a83d15dc.png)

由[Edvard Alexander lvaag](https://unsplash.com/@edvardr?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 服务质量(QoS)

当节点接近或已经达到资源耗尽时，Pod QoS 提供了关于如何杀死 Pod 的某些保证和排序。在 k8s 中，有两种类型的资源，不可压缩或可压缩。可压缩资源是 k8s 可以对其创建约束的资源。这个时候我们只能用 CPU 做到这一点。正如我在我的[上一篇文章](https://medium.com/dev-genius/hardening-your-kubernetes-stack-pt-1-29b7006b5085)中提到的，你不能违反 CPU 限制，因为 CPU 限制本质上是时间片，你不能过度分配这些时间片。然而，与 CPU 不同，内存和磁盘空间在 k8s 中是不可压缩的资源，因此为了保护节点，k8s 需要在节点耗尽资源时消除工作负载。pod 被淘汰的顺序和优先级基于 QoS 等级。K8s 根据 pod 的资源请求和限制来确定 pod 的 QoS 等级。它由 3 个类指定。

## 担保

这个类意味着 pod 保证不会被杀死，直到它们超过它们的极限。为了让一个 pod 接收保证等级，您必须为该 pod 中的每个容器设置请求和限制。除此之外，每个容器的请求必须符合限制。最简单的方法是设置没有请求的限制，因为 k8s 会默认 pod 的请求限制。

注意第一个容器 echoserver 是如何为两个容器定义请求和限制的，请求和限制值是相同的。第二个容器 echoserver2 只定义了限制，也可以工作。最后，由于 pod 内的所有容器都满足要求，所以 pod 将被分类为保证的。

## 可爆发的

这个类意味着 pod 有最小的资源保证，但是如果需要并且节点有资源，它可以获得更多的资源。当存在内存压力时，如果超出请求，它们可能会被杀死。这发生在杀死 BestEffort 类豆荚之后。当为 pod 中的一个或多个容器设置了请求和可选限制，并且它们不满足保证的标准时，pod 被分配此类。

注意，第一次部署有两个容器。你不需要匹配请求和限制。第二次部署我们只为容器定义了请求，这也很好。

## 尽力而为

它们被视为最低优先级。如果节点内存不足，这些 pod 将首先被终止。当没有设置请求或限制时，pod 被分配此类。

## 摘要

当节点面临资源压力时，k8s 会通过杀死节点上运行的 pod 来试图缓解。我们可以通过利用服务质量等级来确保按照优先级顺序终止某些 pod。K8s 通过如何设置资源请求和限制来确定这些类别。优先级保证>突发>尽力而为。我在本文开头提到磁盘空间是一种不可压缩的资源，但是它不是一种可以在 pod 级别指定的资源，因此我们没有包括磁盘定义。

[来自谷歌](https://medium.com/google-cloud/quality-of-service-class-qos-in-kubernetes-bb76a89eb2c6)关于 QoS 的更多信息和[官方文档](https://kubernetes.io/docs/tasks/configure-pod-container/quality-service-pod/)

## Pod 优先级和先占权

Pod 优先级和抢占控制着 pod 的调度方面。该功能需要创建一个资源调用[优先级类别](https://kubernetes.io/docs/concepts/configuration/pod-priority-preemption/#priorityclass)。优先级类定义了 pods 可以继承的值和类名。

值可以是小于或等于 10 亿的任何 32 位整数。数字越大，它们就越重要。注意这一点，因为可能有系统关键的应用程序以更高的值运行，不应该被工作负载应用程序抢占，请检查您的集群的操作员。在您的集群中，默认情况下还有优先级类，您将看到有 2 个默认类，系统集群关键和系统节点关键，它们的值分别为 20 亿和 20 亿 1 千。正如你所看到的，k8 也利用这一点为自己的资源。K8s 通过将这些资源设置为不可能的高值来控制它们。

如上例所示(第 10 行),通过设置`priorityClassName: {priorityclassname}`为 pod 分配优先级。

在 pod 调度期间，pod 被添加到调度队列中。在队列中，优先级值较高的单元将被放在优先级值较低的单元之前。默认情况下，所有没有优先级的 pod 都被赋予值 0，除非设置了优先级`globalDefault: true`，则默认值将被设置为该优先级的值。

## 先占

默认情况下，还会为所有优先级启用抢占。当具有高优先级的 pod 待定并且没有可用节点时，k8s 将尝试从节点中移除一个或多个具有较低优先级的 pod，以便调度待定的高优先级 pod。可以通过在优先级上设置`PreemptionPolicy: Never`来禁用抢占。因为具有这些类的 pod 不会抢占在节点上运行的 pod，所以它们会受到调度回退的影响。这意味着，如果一个 pod 不能被调度，它们将会以指数后退的方式重试。这将允许具有可能较低优先级的其他单元在较高优先级单元之前被调度。

## **总结**

您可以为 pod 创建优先级类别，以便控制调度 pod 的优先级。您可以通过检查每个优先级类的值来确定顺序。您也可以通过设置`globalDefault: true`为所有没有优先级的 pod 创建一个全局默认优先级。默认情况下，如果 k8s 不能调度高优先级 pod，则较高优先级等级的 pod 将抢占节点上较低优先级的 pod。您可以通过在优先级上设置`PreemptionPolicy: Never`来关闭此功能。

[第 1 部分:资源请求和限制](https://medium.com/dev-genius/hardening-your-kubernetes-stack-pt-1-29b7006b5085)
[第 3 部分:滚动更新和 Pod 中断预算](https://medium.com/dev-genius/hardening-your-kubernetes-stack-pt-3-b260d45fe6e)
[第 4 部分:水平 Pod 自动缩放和集群自动缩放](https://medium.com/dev-genius/hardening-your-kubernetes-stack-pt-4-cc72b09b4557)