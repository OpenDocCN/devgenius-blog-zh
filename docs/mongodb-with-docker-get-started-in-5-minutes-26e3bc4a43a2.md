# MongoDB with Docker:5 分钟内开始

> 原文：<https://blog.devgenius.io/mongodb-with-docker-get-started-in-5-minutes-26e3bc4a43a2?source=collection_archive---------3----------------------->

![](img/6fe17df4a9c94fb2ea490f3dba3fd55e.png)

MongoDB 是一个流行的 **NoSQL 数据库，它使用文档来存储数据**。MongoDB 被认为是*无模式的*，这意味着它不需要定义数据库模式。如果您想快速扩展和发展，这是一个很好的工具，因为它支持快速迭代开发，并允许多个团队成员协作。

Docker 是一个工具，你可以用它来**构建在你的主机操作系统**上运行的应用程序。它在 Linux 上自然运行。Docker **使用容器**并允许你将你的应用程序和它所有的依赖项合并成一个单元。使用 Docker，很容易创建一个容器并开始使用不同的技术。

在本教程中，我们将使用 Docker 创建一个 MongoDB 容器。

我们开始吧！

**我们将介绍**:

*   为什么要用 MongoDB 搭配 Docker？
*   入门指南
*   设置 MongoDB 容器
*   与 MongoDB 容器交互
*   总结和后续步骤

# 为什么要用 MongoDB 搭配 Docker？

MongoDB 支持**高可用性和可伸缩性**。它在 Docker 容器这样的分布式环境中工作得很好。**通过 Docker 使用 MongoDB，我们可以拥有一个可移植的数据库**，它可以在任何服务器平台上运行，而不必担心它的配置。我们可以使用 Docker 和 MongoDB 容器映像来使数据库部署过程更加高效和简单。

容器化数据库提供了跨不同环境的一致性，并支持更快的开发设置。Docker 容器非常便携，这意味着您可以在任何想要操作的地方使用容器。它们可以在云上轻松运行，并与多个云平台协同工作。

# 入门指南

是时候创建我们的 Mongo 容器了！在我们开始之前，让我们确保我们的设备上安装了 Docker。一旦我们安装了 Docker，我们就可以开始了。

我们将从从 Docker Hub 下载最新的 MongoDB Docker 映像开始。Docker 映像是一个允许我们在 Docker 容器中执行代码的文件。这个文件包含创建在 Docker 上运行的容器的指令。MongoDB 图像有不同的版本。每个 Mongo 图像都有不同的用途。

我们将使用标准的 Mongo 映像，并使用以下命令:

```
sudo docker pull mongo
```

如果我们想下载 MongoDB 的特定版本，我们可以使用相同的命令并附加 version 标记。它看起来会像这样:

```
sudo docker pull mongo: 4.0.4
```

> ***提示*** *:查看 Docker Hub，找到适合你的标签。*

# 设置 MongoDB 容器

现在我们已经创建了我们的 Mongo 映像，我们准备设置我们的 MongoDB 容器。我们将**使用 Docker run 命令**来部署一个 MongoDB 实例，并给它一个容器名:

```
docker run --name tutorial mongo
```

如果我们下载了 Mongo 的 4.0.4 版本，我们会将 version 标签附加到末尾，如下所示:

```
docker run --name tutorial mongo:4.0.4
```

> ***注*** *:* `*tutorial*` *是我们给自己的 MongoDB 容器起的名字。*

# 与 MongoDB 容器交互

我们创造了我们的容器！我们将通过 bash shell 客户端与数据库进行交互。我们将在交互终端中使用`docker exec`命令来连接它:

```
docker exec -it tutorial bash
```

上面的命令将使用交互终端连接到我们名为`tutorial`的部署。它还将启动 bash shell。现在，我们准备开始使用 MongoDB。我们可以使用以下命令启动 MongoDB shell 客户端:

```
mongo
```

让我们创建一个新的数据库，并将其命名为“educativeblog”。我们可以用下面的命令来完成:

```
use educativeblog
```

我们不能使用我们的数据库，直到我们添加数据到它。我们将在我们的`educativeblog`数据库中的`dogs`集合中创建三个文档。

```
db.dogs.save({ name: “Spot” })
db.dogs.save({ name: “Lucky” })
db.dogs.save({ name: “Mochi” })
```

如果我们想查询我们的 MongoDB 数据，我们可以这样做:

```
db.dogs.find({ name: “Spot” })
```

# 总结和后续步骤

祝贺您迈出了使用 MongoDB 和 Docker 的第一步！用 Docker 运行容器非常高效。创建一个 MongoDB 容器允许我们使用一个可移植的、可扩展的 NoSQL 数据库，而不用担心运行它的设备的底层配置。MongoDB 是最流行的 NoSQL 数据库系统，它可以用于许多方面。关于 MongoDB 还有很多需要学习的地方。
接下来推荐的主题包括:

*   通过 Node.js 使用 MongoDB
*   MongoDB 服务器
*   MongoDB API 和 JSON 实体

*快乐学习！*