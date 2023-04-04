# 给你的 Kubernetes 系列添加护栏

> 原文：<https://blog.devgenius.io/hardening-your-kubernetes-stack-pt-1-29b7006b5085?source=collection_archive---------7----------------------->

## 资源限制和要求

## 部署资源

本系列将带您了解如何提高堆栈的整体弹性。在本节中，我们将通过提供资源需求，让 Kubernetes (k8s)对资源消耗做出反应来实现这一点。

![](img/1373d029624eb72829ceb9ae5719a8ad.png)

斯文·布兰德斯马在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

## 介绍

我经常注意到团队遇到稳定性问题，因为他们不知道 Kubernetes (k8s)如何执行资源管理。由于很多 k8s 的资源管理功能不会在你不给它提供信息的时候踢它，k8s 最后就瞎飞了。

k8s 管理资源主要有两种方式。首先是资源分配，这是经典的装箱问题。我如何把 x，y，…卷的一样多的项目..第二个是资源利用率。k8s 就是这样调整节点数量来满足当前的资源利用率的。如果集群缺乏，则向上扩展；如果有剩余，则向下扩展。这两个特性都需要容器配置中的资源定义才能工作。

Kubernetes 不是一个神奇的系统。如果你不告诉它，它不知道你的应用需要多少资源。通过在您的部署/ pod 上指定资源请求和限制，它可以帮助您的集群将您的应用程序正确地打包到节点上。[官方文件](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/)

## 资源请求:

设置一个资源请求告诉 k8s 你的应用需要运行什么样的基线资源。这将告知 k8s 如何根据可用资源(节点上的可用资源≥应用请求的资源)在集群中打包应用。

示例(第 21–24 行):

1 个 vCPU/核等于 1000m (m 代表毫 CPU)。CPU 是由你获得多少 CPU 时间来定义的。默认情况下，它基于 100ms(毫秒)的周期。因此，0.5 vCPU 等于每 100 毫秒 50 毫秒的 CPU 时间。

内存是以字节为单位定义的，用后缀表示:E，P，T，G，M，K 或等价的 2 的幂:Ei，Pi，Ti，Gi，Mi，Ki

因此，在示例应用程序中，应用程序需要 0.25 的 vCPU 和 500 兆的内存。K8s 将查看您的集群中的节点，并找到适合该应用程序的位置，并在那里安排它。

一旦在容器上指定了资源，就会启用四个非常重要特性。

1.K8s 现在会根据资源可用性将您的应用程序适当地调度到节点中。每个节点都有有限的可分配 CPU 和内存，每个容器都消耗这些资源。如果没有资源请求或限制，k8s 不知道每个应用程序需要什么资源，k8s 可能会将太多的应用程序打包到一个节点上，导致节点不堪重负。

2.启用使用[水平 Pod 自动缩放器](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) (HPA)的能力。Horizontal pod autoscaler 是 k8s 社区提供的一个应用程序，用于根据特定资源的目标利用率来扩展 pod。默认情况下，支持 CPU 和内存，但可以扩展到自定义指标。HPA 还需要[一个度量服务器](https://github.com/kubernetes-sigs/metrics-server)来工作。这有助于您的应用程序向上和向下扩展，以便每个单元不会过度利用资源。

3.启用使用[集群自动缩放器](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler)的能力。Cluster autoscaler 也是 k8s 社区提供的应用。当集群中请求的资源大于集群的可用资源时，集群自动缩放器会启动并向集群添加节点。这通常发生在 pod 由于资源不足而陷入“待定”状态时。

4.工人节点储备受到重视。工作者节点预留的行为类似于部署请求，但针对的是 kubelet 和 OS 之类的工作者节点资源。K8s 会将 pod 分配到一个节点上，直到没有剩余的可分配资源。如果您的 pods 没有使用请求，K8s 不知道它是否过度分配，这会导致节点不稳定，因为工作节点资源可能会被您的应用程序消耗。

## 限制

限制限定了应用程序可以消耗多少资源。内存限制的表现类似于您的计算机上只有 x 数量的内存。从应用程序的角度来看，可用内存量将是您在容器定义中设置的限制。除此之外，如果您的容器决定消耗超过部署中设置的限制，k8s 将使您的 pod 成为内存不足杀死(OOM 杀死)的候选对象。如前所述，CPU 资源是基于时间片的，所以当你的应用程序在一个周期内需要的 CPU 超过 CPU 限制时，它将被限制到下一个周期。

对 CPU 的限制要非常小心。如果设置不当，CPU 限制会影响您的应用程序。根据我的经验，大多数计量工具都不够精确，无法准确跟踪 CPU 消耗。因此，您的应用可能会因为您的限制而受到抑制，但这并没有反映在您的指标中。对于产生许多线程的应用程序来说，设置 CPU 限制也特别困难，比如大多数 HTTP 服务对每个传入请求都使用阻塞 IO 线程。例如，在一个有 10 个内核且应用程序运行 10 个线程的节点上，您将 vCPU 限制设置为 0.5。这意味着如果产生 10 个线程，假设所有 10 个线程同时运行，每个线程将只获得 0.5/10 vCPU 的时间(50 毫秒)。也就是说，在您耗尽应用程序的 CPU 之前，总处理时间为 50 毫秒。

CPU 限制很难，值得深入研究。我强烈推荐这篇文章，它对这个问题有更深入的解释:[https://medium . com/@ betz . mark/understanding-resource-limits-in-kubernetes-CPU-time-9 eff 74d 3161 b](https://medium.com/@betz.mark/understanding-resource-limits-in-kubernetes-cpu-time-9eff74d3161b)

示例(第 21–24 行):

## 摘要

在您的部署上设置请求和限制将为 k8s 提供基本信息，以适当地将您的应用程序放置在集群中，从而最大化效率并防止您的应用程序淹没节点。这将极大地提高您的集群对不稳定负载和过度消耗资源的错误应用程序的弹性。这也将迫使你更好地理解你的应用程序的资源需求。

[第 2 部分:Pod 服务质量和优先级](https://medium.com/dev-genius/hardening-your-kubernetes-stack-pt-2-2f2db4ff410d)
[第 3 部分:滚动更新和 Pod 中断预算](https://medium.com/dev-genius/hardening-your-kubernetes-stack-pt-3-b260d45fe6e)
[第 4 部分:水平 Pod 自动缩放和集群自动缩放](https://medium.com/dev-genius/hardening-your-kubernetes-stack-pt-4-cc72b09b4557)