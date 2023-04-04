# Docker 撰写-现在在 AWS 上！

> 原文：<https://blog.devgenius.io/docker-compose-now-on-aws-42c83460c93e?source=collection_archive---------28----------------------->

![](img/206942efa170f5649140ee3b37e5965b.png)

Guillaume Bolduc 在 [Unsplash](https://unsplash.com/s/photos/boxes?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

昨天，亚马逊 AWS 的弹性容器服务和 Docker 的工程师发布了一些非常酷的消息——Docker Compose 将很快在 ECS 中全面实现。这可以极大地简化部署过程，使开发人员能够更专注于构建应用程序，而不是管理各种配置文件。

对于我们大多数开发容器化应用的人来说， [Docker Compose 是我们开发环境的一个重要部分。](https://docs.docker.com/compose/)我们不同组件或服务的代码库(例如，前端、后端和数据库)都是在各自的 Docker 容器中建立的，并配有各自的 Docker 文件配置。当我们想一起测试应用程序的所有元素并确保一切正常时，我们创建一个 docker-compose.yml，它基本上是一个特殊的 docker 文件，列出了所有单独的容器，并描述了它们如何相互通信。这是一个非常简单和容易设置的 api，使得应用程序的容器化变得轻而易举。

然而，Docker Compose 实际上只适用于本地开发环境。当你测试完你的应用并且[想要在 AWS 上部署它时，你必须创建一个 Dockerrun.aws.json](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/single-container-docker-configuration.html) 文件，它包含与 Docker compose 文件相同的信息，只是格式完全不同，并且包含更多与弹性容器服务相关的具体细节。

在更大的容器化应用程序中，这是有意义的，因为您将真正微调每个容器的特定配置，并以各种方式优化它们。拥有特定的 aws json 允许您在方便的位置管理它。对于没有大量流量或奇怪数据流的简单应用程序，这个过程可能会感觉非常多余，因为如果只需要最少的配置，Docker compose 和 dockerrun 文件将看起来相同。

输入一个新的 AWS 功能，目前处于测试阶段:Docker ECS！Docker 和 Amazon 合作创建了一个新的 docker CLI 功能，允许您将 Docker Compose 直接上传到 AWS ECS，无需任何额外的配置和调整！它所需要的只是你的 aws 配置文件凭证和默认区域，然后你就可以直接从 docker 把你的应用程序部署到 AWS 上了。

对于小规模的应用程序开发来说，这个新的 api 真的是天赐之物。花在整理各种 yaml 和 json 文件以及为小型个人应用程序调整额外 ECS 控制台上的时间现在可以专门用于开发和测试。你不必担心基础设施的问题，直到你的应用程序变得足够大，需要它。

请密切关注 Docker-ECS，它将很快退出测试版，并成为 Docker 客户端的正式功能！

[](https://aws.amazon.com/blogs/containers/aws-docker-collaborate-simplify-developer-experience/) [## AWS 和 Docker 合作简化开发者体验|亚马逊网络服务

### 开发人员现在可以使用 Docker Compose 和 Docker Desktop 将应用程序部署到 Amazon ECS，如果您有任何疑问…

aws.amazon.com](https://aws.amazon.com/blogs/containers/aws-docker-collaborate-simplify-developer-experience/) [](https://www.docker.com/blog/from-docker-straight-to-aws/) [## 从 Docker 直接到 AWS - Docker 博客

### 就在大约六年前的一天，Docker 迎来了 Docker Compose 的第一个里程碑，这是一种简单的方式来布局您的…

www.docker.com](https://www.docker.com/blog/from-docker-straight-to-aws/) [](https://github.com/docker/ecs-plugin) [## docker/ecs 插件

### 这是在 2020 年 AWS 云容器大会上宣布的，阅读博客帖子。❗的 Docker ECS 插件仍然在…

github.com](https://github.com/docker/ecs-plugin)