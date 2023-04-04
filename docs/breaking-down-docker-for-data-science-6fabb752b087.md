# 分解数据科学的 docker

> 原文：<https://blog.devgenius.io/breaking-down-docker-for-data-science-6fabb752b087?source=collection_archive---------19----------------------->

你曾经在 VM 上手动设置过数据库或者任何像 apache Kafka 这样的开源工具吗？为实验或者甚至为生产建立基础设施是相当麻烦的，而且复制整个环境甚至更困难。

![](img/9fd6889977b61b764a713f0052bc5524.png)

来源:[图片](https://www.docker.com/sites/default/files/d8/2019-07/Moby-logo.png)

## 那么“码头工人”到底是什么？

根据官方定义——“Docker 是一组平台即服务产品，使用操作系统级虚拟化来交付称为容器的软件包中的软件。容器是相互隔离的，捆绑了它们自己的软件、库和配置文件；他们可以通过明确定义的渠道相互交流。”

现在让我们试着用更简单的逻辑来理解这一点。

假设您有一台笔记本电脑，在虚拟环境中手动安装了 python 3.8 和几个库，本地运行 postgresql。现在，您觉得需要在五个不同的云虚拟机中进行完全相同的设置，在这种情况下，复制整个环境可能会很繁琐。

仍然手动操作的最佳情况是设置 shell 脚本来自动化整个过程，但是如果您的笔记本电脑使用 windows 作为操作系统，而目标虚拟机使用不同的 Linux 发行版，如 CentOS、Ubuntu 等，该怎么办呢？在某些情况下，您可能会发现这些脚本的行为会因操作系统的构建而有所不同。

在本地设备上创建一个虚拟机怎么样？当然，这就是虚拟化的用武之地。简而言之，你现在可以在同一台笔记本电脑上同时运行不同的操作系统，并且它们的资源可以共享。一个虚拟机有一个功能齐全的操作系统发行版，一切都按预期运行。

太好了！还是没那么伟大？？

这里的问题是:你需要所有这些功能吗？由于其特性，操作系统本身具有巨大的尺寸。这可以进一步优化。

因此，请将 Docker 想象成一个运行封装环境的引擎，或者更确切地说，一个具有您特别提到的极简特性的虚拟机，并且具有足够的操作系统来实现预期的功能。容器只不过是这些独立运行的优化环境，它们共享来自主机的资源。您可以使用使容器能够与本地网络、vpc 或公共网络通信的端口来进一步启用网络。当您停止 docker 容器时，您还可以将您的本地或任何持久存储挂载到这些 docker 容器中的一个，以持久保存更改。

# 数据科学为什么要用 Docker？

如果您一直在本地计算机上使用 pip 和 conda 等包管理器试验 data science 包，在虚拟环境中摸索解决包依赖性，或者由于环境变化而在复制结果时遇到问题。有时，在实验的名义下，我们会在本地环境中发狂，我们可能会错误地破坏一些依赖关系，甚至我们的 GPU 驱动程序配置，Docker 可以为您解决所有这些问题。

## 为什么

你可以运行一个完整的 juypter 环境(有或者没有 conda 环境)。此外，您可以通过最少的设置获得 GPU 支持。有许多 docker 映像支持 GPU，因为它们有高效使用所需的正确版本的驱动程序。例如:

用于设置容器的 docker 文件

你可以找到许多官方组织，比如 tensorflow 和 T2 py torch，公开展示他们的官方图片。您可能需要阅读概述页面上的信息，以决定适合您的用例的图像标签，但这些通常是不言自明的。例如，上面我们使用了已经配置了 tensorflow 的 jupyter，但是如果你喜欢自己安装所有东西，并且只需要基本 env，你可以尝试 [miniconda](https://hub.docker.com/r/conda/miniconda3/) 或 plain ol' [jupyter](https://hub.docker.com/r/jupyter/base-notebook) nb。

您可以将您的笔记本和 requirements.txt 复制到 docker 容器中，或者您可以挂载该卷(基本上只是将您的本地存储附加到 docker 容器中),以防您的文件太大，因为这会严重影响 docker 映像的构建大小。换句话说，你的文件不会被打包在容器中，如果你需要与任何人共享设置，你也必须与容器一起单独共享文件。

您可以通过执行以下操作来运行示例:

```
git clone [https://github.com/smiraldr/docker-jupyter-sample.git](https://github.com/smiraldr/docker-jupyter-sample.git)cd docker-jupyter-sample/appdocker build -t my_tf_jupyterenv .docker run -v /Users/smiralrashinkar/Desktop/free/docker-jupyter-sample/app/notebooks:/app -p 80:80 my_tf_jupyterenv:latest 
#after -v remove the path and paste your own path
```

然后，您可以前往您的 localhost:80，瞧！你让你的 jupyter 实验室在这个容器里运行。此外，您可以在 jupyterlab 上看到您的笔记本，因为我们已经从本地机器上安装了一个卷。

现在，如果你想共享这个环境或在云上部署它，你可以简单地将容器推到 dockerhub 或使用各种解决方案部署它，如 Google cloud run。

![](img/f57f9fff05e5010d3602646ae00359d7.png)

Docker +数据科学= ❤️ |来源:[图片](https://dribbble.com/shots/3122897-Nyan-Whale)(作者允许使用)