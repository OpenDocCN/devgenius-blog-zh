# Docker 磁盘空间管理—管理资源的提示

> 原文：<https://blog.devgenius.io/docker-disk-space-management-tips-for-managing-resources-ca96999a6d47?source=collection_archive---------11----------------------->

许多 Docker 用户面临的一个常见挑战是找到有效管理磁盘空间的方法。这篇博文将讨论解决这个问题的各种方法，并帮助您保持系统平稳运行！

![](img/7d6f990f7b75340351dba40f15fc4c01.png)

照片由[艾萨克·史密斯](https://unsplash.com/@isaacmsmith?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 磁盘空间消耗

创建映像和运行容器会消耗磁盘空间，稍后您可能想要回收这些空间。以下是不知不觉中消耗磁盘空间的一些方式:

*   停止后，未通过使用 *docker run* 命令上的`--rm`开关或使用 *docker rm* 命令移除的停止容器。
*   未使用的图像:未被其他图像或容器引用的图像。
*   悬空图像:没有名字的图像。当你 *docker 构建*一个标签和以前一样的图像，新的标签替换了旧的标签，旧的标签变得悬空时，就会发生这种情况。
*   未使用的卷。

Slimmer、UnionFS 和 Init-containers 等工具可以通过将多个图像合并到一个层中来帮助您减小图像大小。像 docker-gc 这样的工具会删除使用 Docker API 而被孤立的停止的容器

手动一个接一个地删除它们可能很繁琐，但是有垃圾收集命令可以帮助您做到这一点。

# 回收磁盘空间

大多数命令要求交互式确认，但是如果您想在无人值守的情况下运行它们，您可以添加-f 开关。

您可以运行以下命令来删除不需要的项目:

```
docker container prune -f
docker volume prune -f
docker image prune -ff
```

回收磁盘空间的另一种方法是删除未使用的卷。您可以使用`docker volume ls` 命令列出您的所有卷，并查看哪些卷没有被使用:

要删除未使用的卷，可以运行以下命令:

```
docker volume rm <VOLUME_NAME>
```

这将删除容器、卷和图像。请注意，这不会删除您的数据或这些项目中的任何内容；它只是从 Docker 中删除它们的关联

请注意，只有悬挂的图像会被删除。保留未使用的图像，这在网络连接稀缺或不可用的情况下很好，因为这意味着您保留了基本图像，这些图像在以后可能会有用。如果您想删除所有未使用的图像，只需使用以下命令:

```
docker image prune  — all
```

默认情况下，如果当前没有使用卷的容器，则不会删除卷以防止重要数据被删除。运行命令时使用`--volumes`标志来清理卷。

```
docker system prune -a --volumes
```

Docker 是一个开源项目，它通过提供额外的抽象和自动化层来自动化软件容器内的应用程序部署。在任何应用程序中，磁盘空间管理问题都是一个值得关注的问题，所以在使用 docker 时，知道如何正确管理您的资源是很重要的，希望这篇文章对您有所帮助，下次再见！