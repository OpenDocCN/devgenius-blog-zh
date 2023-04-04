# Docker 绝对初学者指南:什么是容器？

> 原文：<https://blog.devgenius.io/absolute-beginners-guide-to-docker-what-is-a-container-4a9041fa2a6?source=collection_archive---------3----------------------->

![](img/0d50270bd36a647edd47ed8015d48198.png)

Docker 是一个开源软件平台，可以帮助您简化创建、管理、运行和分发应用程序的过程。使用 Docker，您可以将您的应用程序及其所有依赖项打包到一个容器中。容器允许您的应用程序被容易且统一地部署。

今天，我们将深入探讨 Docker，讨论容器、模块、关键术语等等。

**我们将讲述**:

*   Docker 是什么？
*   码头工人 vs 库贝内特斯
*   码头词汇指南
*   安装 Docker
*   接下来学什么

# Docker 是什么？

Docker 是一个运行在 Linux 和 Windows 上的开源软件。有了 Docker，你可以**将你的应用程序及其依赖项打包到容器中**。Docker 允许您将应用程序从基础设施中分离出来。

该公司最初是一个基于 Linux 容器的平台即服务。为了帮助制造和管理容器，他们开发了一个内部工具，昵称为“Docker”，这就是这项技术的诞生。第一个版本于 2013 年发布。

如今，Docker 主导着市场。许多公司使用 Docker 来**简化构建、运行和管理应用程序的过程**。它改变了公司开发应用程序的方式。Docker **将安装它的计算机**的操作系统虚拟化，这赋予了它极其便携的功能。

Docker 用于:

*   DevOps
*   软件
*   信息技术服务
*   人员配备和招聘
*   金融
*   卫生保健
*   零售
*   等等。

在我们进入其他话题之前，让我们先谈谈 Docker 容器。

# 什么是容器？

长期以来，公司一直在使用容器技术来解决虚拟机的弱点。我们可以认为容器是虚拟机的更轻量级版本。容器和虚拟机的重要区别在于，容器不需要自己的操作系统。**主机上的所有容器共享该主机的操作系统**，这释放了大量的系统资源。

现在的现代容器是从 Linux 容器(LXC)开始的。包括 Google 在内的许多贡献者已经帮助将与容器相关的技术引入 Linux 内核。没有这些贡献，我们就不会有今天这样丰富的容器生态系统。

直到码头工人出现，集装箱运输才变得容易。Docker 容器在应用层创建了一个抽象。容器**将您的应用程序及其容器依赖项与运行所需的一切打包在一起，包括**:

*   操作系统
*   应用代码
*   运行时间
*   系统工具
*   系统库
*   等等。

# Docker 模块

Docker **提供了许多不同的模块和插件**。让我们来看看一些最受欢迎的。

**Docker 撰写**

`docker-compose`允许你**定义和运行多容器应用**。使用 Compose，您可以使用一个`YAML`文件来配置应用程序的服务，并在 Docker 守护进程或 Docker Swarm 上编排容器。你可以把它想象成一个自动化的多容器工作流程。Docker Compose 非常适合开发、测试、CI 工作流和试运行环境。

**对接机**

`docker-machine`允许你**将你的容器化应用部署到云**。使用 Docker Machine，您可以创建一个远程虚拟机并管理您的容器。对于创建部署环境和管理运行在应用程序上的微服务来说，这是一个很好的工具。可以配合 AWS、微软 Azure 等热门云服务使用。

**Docker 栈**

Docker stack 允许你用 Docker Swarm 管理 Docker 容器集群。Docker 堆栈嵌入到 Docker 命令行界面(CLI)中。使用 stack，您可以在一个文件中描述多个服务。它消除了维护 bash 脚本来定义服务的需要。

**码头工人群**

Docker Swarm 允许你在不同的主机上管理多个容器。换句话说，它是一个容器编排工具。有了 Swarm，你可以把多个 Docker 主机变成一台主机。

# 码头工人 vs 库贝内特斯

混淆 Docker 和 Kubernetes 是很常见的，所以让我们花些时间来看看这两种技术之间的区别。这些技术可以很好地相互补充，并且经常一起使用。
我们已经在本文中探讨了 Docker，但是让我们强调一些要点。

**码头工人**

Docker 是一个**集装化平台**。我们可以使用 Docker 来构建和运行容器。Docker Engine 是一个运行时环境，允许您在开发机器上构建和运行容器。操作应用程序可能很复杂，尤其是当您在不同的服务器上部署了大量容器时。很难确定协调和调度多个容器的最佳方式，很难弄清楚它们如何相互通信，也很难决定如何扩展容器。这就是 Kubernetes 的用武之地！

**Kubernetes**

Kubernetes 是一款**开源编排软件，用于 Docker** 等容器化平台。它有一个控制容器操作的 API。Kubernetes 允许您组织一个虚拟机集群，并调度容器在这些虚拟机上运行。有了 Kubernetes，你**可以运行 Docker 容器并管理你的容器化应用**。您的容器被分组到窗格中，您可以按照自己的意愿来扩展和管理这些窗格。

> **等等，Kubernetes 和 Docker Swarm 有什么区别？**
> 
> *正如我们上面讨论的，Docker Swarm 允许您跨不同的主机管理多个容器。*
> 
> *Docker Swarm 和 Kubernetes 的区别在于，Kubernetes 比 Docker Swarm 全面得多。它跨集群运行，而 Docker 运行在一个节点上。Kubernetes pods 跨节点划分，以确保可用性。*

# 码头词汇指南

让我们来看看在使用该平台时会看到的一些常见 Docker 术语。

*   **Cgroups** :控制组允许您在系统上运行的进程之间分配资源。
*   **容器映像** : Docker 映像是用于在 Docker 容器中执行代码的文件。
*   **Docker build** : `docker build`是一个用于从 Docker 文件构建映像的命令。
*   **Docker 引擎** : Docker 引擎是 Docker 的核心产品，包括其守护进程和 CLI。它有一个与 Docker 守护进程交互的 API。
*   Docker file:Docker file 是一个基于文本的文档，保存了构建 Docker 映像的指令。
*   Docker Hub : Docker Hub 是一项服务，它允许你找到并与你的组织共享容器。
*   Docker Registry:Docker Registry 允许你存储和分发命名的 Docker 图片。注册表被组织成存储库，它们保存不同映像的所有版本。
*   **Docker run**:run 命令允许你从指定的图像创建一个容器，并使用给定的命令启动该容器。
*   名称空间:名称空间是在运行容器时创建的。它们提供了一个隔离层，因为容器的每个元素都运行在不同的名称空间中。
*   **Pull** : `docker pull`是一个命令，允许您下载一个特定的图像或一组图像。
*   **存储库(repo)** : Docker 存储库允许您与他人共享容器映像。这些图像存储为标签。
*   标签:Docker 标签就像标签一样，你可以分配给任何已完成的构建。
*   **联合文件系统(AUFS)** :联合文件系统将单个主机上的多个目录分层，并将它们呈现为单个目录。

# 安装 Docker

有许多不同的方法和地方安装 Docker。我们将为 Windows 10、Mac 和 Linux 安装 Docker 桌面。

**Docker Desktop 是一款允许你构建和分享容器化应用和微服务的应用**。根据 Docker 文档，它包括:

*   码头引擎
*   Docker CLI 客户端
*   Docker 撰写
*   Docker 内容信任
*   库伯内特斯
*   凭证助手

# Windows 10 安装

在 Windows 10 上下载 Docker Desktop 之前，您必须具备以下条件:

*   64 位版本的 Windows 10 专业版、企业版或教育版
*   必须在系统的 BIOS 中启用硬件虚拟化支持
*   必须在 Windows 中启用 *Hyper-V* 和*容器*功能

从谷歌搜索“安装 Docker 桌面”开始此搜索会将您带到下载页面，您可以在那里下载安装程序并按照说明进行操作。

安装后，您可能需要从 Windows 开始菜单手动启动桌面。

# Mac 安装

与 Windows 10 安装一样，在 Mac 上安装 Docker Desktop 最简单的方法是谷歌“安装 Docker Desktop”。从那里，您可以跟随下载页面上的链接。

安装完成后，您可能需要从 MacOS Launchpad 手动启动桌面。

> ***注*** *:有了 Mac，Docker 引擎就不是原生运行在 MacOS 达尔文内核上了。Docker 守护进程运行在一个轻量级的 Linux VM 中，它向您的 Mac 环境公开守护进程和 API。这意味着您可以在 Mac 上打开终端并使用 Docker 命令。*

# Linux 安装

在 Linux 上安装 Docker 有很多方法。你可以谷歌搜索 Linux 上的 Docker 安装指南。在这一节中，我们将看看安装 Ubuntu Linux 20.04 LTS 的一种方法。我们假设您已经安装了 Linux。我们将分两步安装 Docker:

1.  **更新 apt 包索引**

```
$ sudo apt-get update

Get:1 http://eu-west-1.ec2.archive.ubuntu.com/ubuntu.com/ubuntu focal InRelease [265
kb]

...
```

**2。从官方回购安装 Docker**

```
$ sudo apt-get install docker.io

Reading package lists... Done

Building dependency tree

...
```

# 接下来学什么

祝贺你迈出了与 Docker 的第一步！Docker 是一个流行的开源容器化平台，被许多不同的行业用来简化构建、保护和管理应用程序的过程。Docker 的需求量如此之大，它是增加你技能的一个很好的工具。

获得 Docker 实践经验的一个很好的方法是构建一个项目来添加到您的专业文件夹中。在开始之前，关于 Docker 还有很多东西需要学习，例如:

*   容器生命周期
*   Docker 命令
*   创建新容器
*   运行容器

*快乐学习！*