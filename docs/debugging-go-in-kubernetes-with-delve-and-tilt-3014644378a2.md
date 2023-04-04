# 使用 Delve 和 Tilt 在 Kubernetes 进行调试

> 原文：<https://blog.devgenius.io/debugging-go-in-kubernetes-with-delve-and-tilt-3014644378a2?source=collection_archive---------4----------------------->

本文的想法是介绍一种调试运行在 Kubernetes 中的 Go 应用程序的简单方法。

在我们言归正传之前，让我们简单介绍一下我们将要使用的工具。

**为什么是调试器？**

谁没有发现他们的程序由于某种未知的原因而无法工作？在这种情况下，令人惊讶的是(或者不是)大多数人用于调试的是`Println`。我承认滥用它无数次…

![](img/ab5437b646e416ac070a70c0b453d50c.png)

[https://github.com/MariaLetta/free-gophers-pack](https://github.com/MariaLetta/free-gophers-pack)

通常使用`Println`是一个坏主意，因为它效率低下，因为您需要手动添加所有的变量，除了让代码变得混乱之外，还存在未覆盖的情况未打印的风险。

> 调试器前来解救！

为了解决这个问题，我们将使用[**delver**](https://github.com/go-delve/delve)，一个 Golang 调试器。解释它是如何工作的不在本文的范围内，但是有很多好的教程。使用调试器的一些优点是:

*   在调试点检查所有变量。
*   调用堆栈的可见性。
*   动态更新变量。

> 说得对……但是在 Kubernetes 集群上运行的应用程序呢？

**为什么倾斜？**

[**Tilt**](https://github.com/tilt-dev/tiltç) 是一个开源的微服务开发环境，开发者可以将他们的应用部署到 Kubernetes。

主要目标是通过帮助本地持续开发应用程序并将其部署到本地 Kubernetes 集群中来简化开发人员的体验。它通过监控源代码并自动构建和推进部署来实现这一点。

另一个有用的功能是提供 UI 来监控部署过程，其中包含所有部署和日志的信息。

> 让我们两全其美，深究与倾斜！
> 
> 从现在开始，我假设您已经安装了**Dever**和 **Tilt** 并且运行了一个本地的 Kubernetes 集群，我将使用 [**Minikube**](https://minikube.sigs.k8s.io/docs/start/) 来实现它。

**言归正传！**

让我再一次在更高的层次上解释我们试图实现什么，以便我们有一个清晰的思维模式。我们的想法是通过调试服务器启动我们的应用程序，并公开它，这样我们就可以从我们的终端或 IDE 远程连接来调试它，就像我们在机器上运行我们的应用程序一样。

首先，我创建了一个简单的应用程序作为我们的实验品:

一旦我们有了应用程序，我们需要能够将应用程序部署到集群(请忽略构建映像的步骤，因为它是在我们接下来将看到的`Tiltfile`中完成的):

有了这一切，我们只需编写我们的`Tiltfile`，一个 Tilt 将在启动时运行的文件。它是用`Python`的方言 [Starlark](https://github.com/bazelbuild/starlark) 写的。想了解更多，可以查看[公文](https://docs.tilt.dev/tiltfile_concepts.html)。

让我们看一下文件，然后解释我们真正在做什么:

虽然评论已经解释了每一步都做了什么，但是为了更好地理解，我将试着把它分解一下。

首先，我们创建几个变量，并允许使用`allow_k8s_contexts.`函数将 Tilt 部署到我们的集群。

接下来，我们用`docker_build_with_restart`函数加载`restart_process`扩展。

之后，我们在本地构建要在 docker 文件中使用的二进制文件。注意，在 docker 文件中，我们还安装了 Delve 调试器。

然后，我们调用`docker_build_with_restart`函数，它基本上是`docker_build`的一个包装器，来构建镜像，另外它知道如何在一个实时更新结束时重新启动这个过程。

最后，我们从 yaml 文件`k8s_yaml(deployment.yaml)`创建 Kubernetes 部署，并用`port_forward`对其进行配置。从广义上讲，这将引导来自本地端口的所有流量，我们使用了与 Delver 服务器端口`50100`相同的端口，但它可能与 Delver 服务器本身暴露的 pod 端口不同。因此，这将允许我们本地连接到在集群中运行的调试器。

**如何运行它**

现在我们已经有了所有的部分，让我们看看如何用调试器服务器运行 Tilt，然后连接它。

为了运行应用程序，我在`Makefile`中创建了一个简单的 make 目标，如下所示:

```
.PHONY: tilt-up
tilt-up:
   eval $$(minikube docker-env) && tilt up; tilt down
```

这将启动 Tilt，从而运行 Tiltfile。运行该命令后，您将在集群中运行您的应用程序，公开调试器服务器端口`50100`。查看 pod 日志，您应该会看到类似这样的内容:

```
sample-app │ API server listening at: [::]:50100
sample-app │ 2022–10–02T20:51:33Z warning layer=rpc Listening for remote connections (connections are not authenticated nor encrypted)
```

现在我们已经准备好了应用程序，让我们连接到它。

```
dlv connect localhost:50100 --api-version=2
```

如果您按顺序执行了所有步骤，您应该能够毫无问题地连接到调试器。

> **奖励:**我将把配置您首选 IDE 的调试器作为一项任务留给您，以便远程连接到我们的调试服务器，而不是通过终端手动访问它。

**关闭**

我希望这篇文章对你有用，我已经说服你下次至少试一试调试器😊

在 Kubernetes 集群中运行调试器的事实增加了一点复杂性，但是诚实地使用 Tilt 消除了大部分的麻烦。

欢迎任何建议或改进，请分享。

编码快乐！