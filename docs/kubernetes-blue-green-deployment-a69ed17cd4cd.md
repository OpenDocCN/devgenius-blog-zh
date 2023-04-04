# Kubernetes 蓝绿色部署

> 原文：<https://blog.devgenius.io/kubernetes-blue-green-deployment-a69ed17cd4cd?source=collection_archive---------8----------------------->

![](img/15b75a9060f762fb20323231463bb17c.png)

蓝绿部署是一种实现零停机部署的策略。对于云原生应用，通常的做法是在升级应用版本时减少停机时间。由于我们可以轻松地在云上提供新的基础设施，因此采用新的方法来改进部署变得更加容易。

# 蓝绿色部署定义

在本帖中，我们将看看如何在 Kubernetes 中使用蓝绿色部署。寻找定义的最佳地方之一是云计算本地计算基金会( **CNCF** )提供的术语表。CNCF 将[蓝绿部署](https://glossary.cncf.io/blue_green_deployment/)定义为

> *“以最短的停机时间更新运行中的计算机系统的策略。操作员维护两种环境，称为“蓝色”和“绿色”。一个用于生产流量(所有用户当前使用的版本)，而另一个用于更新。一旦对非活动(绿色)环境的测试结束，生产流量就会切换(通常通过使用负载平衡器)。”*

这里的要点是两个名为 blue 和 green 的相同环境，以及使用负载平衡器的流量交换。让我们看看如何将其应用到 Kubernetes 环境中。

# 如何执行 Kubernetes 蓝绿色部署

*   nileshgule/蓝绿色-演示:蓝色
*   nileshgule/蓝绿色-演示:绿色

我们从创建应用程序的两个版本开始。比方说部署到实际环境中的 V1。这就形成了我们的蓝色环境。为了简单起见，我创建了一个简单的 ASP.Net 核心应用程序。它被打包成一个带有两个标签的 Docker 映像。一个标签有蓝色背景，将用于部署到蓝色环境。带有绿色标签的另一个容器映像将用于部署到绿色环境。这两个 docker 容器图像作为

# 步骤 1:设置蓝色环境

![](img/37cb4d6ac0e8b32e221622e3d5d951ee.png)

一旦容器映像被推送到容器注册中心，我们就可以使用 Kubernetes 部署来部署到 Kubernetes 集群。我们使用标记为蓝色的图像创建一个[蓝色部署](https://github.com/NileshGule/canary-demo-kuberentes/blob/main/k8s/blue-deployment.yml)。Kubernetes 部署将在内部创建一个复制集和相关的 pod。在 pod 部署模板中，我们将标签设置为 app: blue。

使用负载平衡器类型的 [Kubernetes 服务](https://github.com/NileshGule/canary-demo-kuberentes/blob/main/k8s/blue-green-service.yml)在 Kubernetes 集群外部公开部署。该服务使用带有 app: blue 值的选择器。

浏览负载平衡器服务的公共 IP，我们将看到蓝色背景的网页。

# 第二步:设置绿色环境

![](img/9116c3f41f5b2d301044b76222785625.png)

蓝绿色部署的第二步是创建绿色环境。这是通过使用图像 nileshgule/blue-green-demo:green 标签创建第二个 [Kuberentes 部署](https://github.com/NileshGule/canary-demo-kuberentes/blob/main/k8s/green-deployment.yml)来完成的。我们还将部署的标签设置为 app: green。清单文件中的其余属性几乎与蓝色部署相同。在应用绿色部署后，该服务仍然为蓝色环境提供 100%的流量。我们可以对部署到绿色环境中的应用程序的下一个版本或发行版进行测试。如果测试成功，我们可以将交通转换到绿色环境。

# 步骤 3:将流量路由到绿色环境

![](img/8c374b1239a3ce19b53748e027c64b31.png)

蓝绿色部署的最后一步是将流量切换到绿色环境。我们可以通过编辑服务清单来做到这一点。我们更新选择器以选择带有 app: green 标签的窗格。将此更改应用于 Kubernetes 服务。所有的交通现在都被重定向到绿色环境。

请注意，蓝色环境仍然存在，但是活动或实时流量由绿色环境提供服务。如果绿色环境有任何问题，我们可以再次更改服务中的选择器，使其快速指向蓝色环境，从而允许我们在对最终用户影响最小的情况下恢复更改。

# Youtube 视频

这篇博文中解释的步骤在 Youtube 视频中有详细演示。观看视频进行现场演示，并随时在视频评论中提供反馈。

# 结论

在 Kubernetes 使用服务的情况下，很容易使用蓝绿部署策略。这种策略对于减少无状态应用程序的停机时间非常有用。有状态应用程序稍微复杂一些。这降低了应用程序升级过程中的风险。有了备用环境，我们可以轻松地回退到旧版本，而不会停机。希望这对你有用。

直到下一次，激情编码，精益求精。

*原载于 2022 年 2 月 8 日*[*【https://www.handsonarchitect.com】*](https://www.handsonarchitect.com/2022/02/kubernetes-blue-green-deployment.html)*。*