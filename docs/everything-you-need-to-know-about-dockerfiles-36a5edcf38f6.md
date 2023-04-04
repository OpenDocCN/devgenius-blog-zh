# 关于 Dockerfiles 你需要知道的一切

> 原文：<https://blog.devgenius.io/everything-you-need-to-know-about-dockerfiles-36a5edcf38f6?source=collection_archive---------12----------------------->

## 简单地说，学习如何使用 Docker 文件创建 Docker 图像。

![](img/547872ce76315e1435489b25ae6255a6.png)

照片由[菲利普·欧塞尔](https://unsplash.com/@ourselp?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/docker?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

在过去，我已经写了大量关于 Docker 的重要性的文章，尤其是现在当我们在云或者混合环境中部署大多数应用程序的时候。然而，世界上仍有许多组织尚未采用 Docker。因此，许多公司严重依赖 Docker 工具来实现高网络可用性、服务连续性和具有高可扩展性的服务提供。

Docker 映像包含一组指令，这些指令在执行时会创建一个 Docker 容器。我通常将 Docker 映像称为应用程序快照或构建应用程序所需的任何东西。Docker 图像只能使用 docker 文件创建。

# 码头文件

Docker 文件是一个文件，它只包含一系列指令，以及创建 Docker 映像所需的参数。

所有 Dockerfiles 文件必须以`**FORM**`指令开始。
`**RUN**`指令将安装依赖项或更新包。
`**COPY**`指令将源代码从本地复制到 Docker 镜像上。
`**ENTRYPOINT**`指令允许指定当图像作为容器运行时将运行的命令。

这是构建 Docker 映像时最常用的四个主要指令。[还有其他命令和指令，可以在这里找到](/docker-the-most-loved-platform-every-developer-must-learn-6e5bb702c97b)。

简单的 Dockerfile 文件通常包含带参数的指令。

```
**FROM python:3.6****RUN pip install flask** **COPY . /opt/** **EXPOSE 8080** **WORKDIR /opt** **ENTRYPOINT ["python", "app.py"]**
```

# 分层架构

当 Docker 创建一个图像时，它是在一个分层的架构中完成的。指令的每一行都会在 Docker 映像中产生一个新层，该层仅包含前一层的更改。由于每个层仅保存前一层的更改，因此它也反映在大小中。

假设 Docker 文件中的任何层出现故障，当您修复该指令并尝试重建映像时，Docker 将重用缓存中以前的层，并继续构建剩余的层。例如，在上面的 Dockerfile 示例中，有六条指令。这些指令中的每一条都会创建一个层。如果图像构建在`**COPY**`指令中失败，重建图像不会花费时间，因为先前的层将从缓存中取出。因此，重建映像会更快。

# 常见的 Docker 相关命令

**构建 Docker 镜像:**
`**docker build -t viveknaskar/my-app .**`

Dockerfile 文件应该在当前的工作目录中。
`**-t**`用于标记码头工人图像。
`**viveknaskar/my-app**`是图像的名称。

**查看建筑各层的历史记录图片:** `**docker history viveknaskar/my-app**`

**查看操作系统所使用的基本镜像:**
`**docker run python:3.6 cat /etc/*release***`

这里，python:3.6 是基础映像。

**从创建的图像创建容器:** `**docker run -p 8888:8080 viveknaskar/my-app**`

Docker 就是这么一个重要的学习平台。事实上， [*根据 2020 年栈溢出调查，Docker 是第二大最受开发者喜爱的平台*](https://insights.stackoverflow.com/survey/2020#most-loved-dreaded-and-wanted) 。我希望你对 Dockerfiles 有一个简单的了解，知道如何建立 Docker files，以及它们在建立 Docker 形象中的重要性。

了解 Dockerfiles 最佳实践的最佳地方是它们的官方文档。

如果你喜欢读这篇文章，你可能也会发现下面的文章值得你花时间去读。

[](/docker-the-most-loved-platform-every-developer-must-learn-6e5bb702c97b) [## docker——每个开发人员必须学习的最受欢迎的平台

### 实际上，根据 Stackoverflow 调查，它是第二大最受开发者喜爱的平台。

blog.devgenius.io](/docker-the-most-loved-platform-every-developer-must-learn-6e5bb702c97b) [](https://javascript.plainenglish.io/7-youtube-channels-that-every-javascript-developer-should-follow-4dcffc9fc0c0) [## 每个 JavaScript 开发者都应该关注的 7 个 YouTube 频道

### 如果你是一个有抱负的或者经验丰富的开发者，强烈推荐这些 YouTube 频道。

javascript.plainenglish.io](https://javascript.plainenglish.io/7-youtube-channels-that-every-javascript-developer-should-follow-4dcffc9fc0c0) 

如果你喜欢阅读有助于你更好地学习、生活和工作的故事，可以考虑 [*成为*](https://viveknaskar.medium.com/subscribe) *的订阅者。成为会员后，你可以无限制地阅读 10000 篇故事、文章和作家。每月只要 5 美元。* [*如果你使用我的链接*](https://viveknaskar.medium.com/membership) *注册，我将获得一点佣金，帮助我写更多的文章。*