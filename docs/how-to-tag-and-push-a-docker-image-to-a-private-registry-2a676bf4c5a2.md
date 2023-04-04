# 如何标记 Docker 映像并将其推送到私有注册表

> 原文：<https://blog.devgenius.io/how-to-tag-and-push-a-docker-image-to-a-private-registry-2a676bf4c5a2?source=collection_archive---------0----------------------->

![](img/eef6c5cb1d95c4c12a27260e56efc97e.png)

[rubén Bagües](https://unsplash.com/@rubavi78?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/container?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的原始照片；logo 由 [Docker](https://www.google.com/url?sa=i&url=https%3A%2F%2Fwww.docker.com%2Fcompany%2Fnewsroom%2Fmedia-resources&psig=AOvVaw0-FD1aPtAUpd4UTZDPP3Lh&ust=1610080968382000&source=images&cd=vfe&ved=0CA0QjhxqFwoTCMiPoJOBie4CFQAAAAAdAAAAABAD) 制作；作者:Tremaine Eto

假设您正在本地机器上开发一个应用程序`sandbox/tremaine-test-app`，并且刚刚制作了映像。当您运行`$ docker images`时，您可以在保存到您机器的所有其他图像中看到该图像。

现在，您希望将它推送到您的私有 Docker 存储库中，以便您在全国各地的朋友可以提取它，然后在他们的机器上运行和测试它。但是，您想要更改图像的名称，并且您还想要将它标记为`SNAPSHOT`版本，以强调它并不是真正的正式发布。

除了可能重新命名图像的纯粹美学原因之外，标记也很重要，因为它告诉 Docker 您希望将图像放在注册中心的哪个存储库中。

# 贴上标签！

这里的关键是标记图像。这基本上意味着你重新命名的形象。它遵循以下格式:

标签是可选的，由括号表示，必须由 ASCII 字符、数字、下划线、句点和破折号组成；另外，*不能以句号或破折号开始*，不能超过 128 个字符。

`yourRegistryHost:<port>`对应于您的私有 Docker 注册表的主机名以及一个端口，如果需要的话。这是为了以后你推新标记的 Docker 图像。

原来有很多不同的方法可以用来标记局部图像。

## 按图像 ID 标记

如果我们看一看本文前面`$ docker images`给我们的列表，我们会看到这些图像对应的图像 id。我们可以在这个命令中使用其中的一个，Docker 的 CLI 将知道引用该 ID。

在前面的例子中，`sandbox/tremaine-test-app`有一个`1.0.0`和一个`latest`版本，但是它们的映像 id 是相同的。那是因为他们在现实中是同一个形象；如果有更多不同版本的`sandbox/tremaine-test-app`标签，那么该标签不一定是最新的*标签，因此它们可能具有不同的图像 id。*

在这个例子和所有后续的例子中，我们用`tremaine/hello-world:1.0.0-SNAPSHOT`标记我们的本地`sandbox/tremaine-test-app`图像。

通过这样做，我们说我们想要有一个名为`hello-world`的图像的新标签，并把版本标签`1.0.0-SNAPSHOT`放在`tremaine`库中。

## 按图像名称标记

在这种情况下，我们明确使用图像名称作为源图像。我喜欢这种方法，因为与复制和粘贴图像 ID 相比，它更容易记住我的图像名称。

在这种情况下，我们没有在我们的源(最左边)图像中指定标签，因此 Docker 将推断它是用于`latest`标签的。

## 按名称和标签标记

这与上一个例子非常相似，但是在这个例子中，我们不希望它默认为源图像的`latest`标签。因此，我们特别指定了`1.0.0`。

# 推它！

现在我们已经有了标记的图像，`yourRegistryHost:<port>/tremaine/hello-world:1.0.0-SNAPSHOT`，我们可以用一个简单的命令将它推送到我们的私有 Docker 注册中心:

由于映像中已经有了 Docker 主机，Docker 会知道将它推到那里。

推送需要一点时间，但当一切都结束后，你的朋友将能够运行`$ docker pull`将图像拉至他们的机器，然后随心所欲地运行它。

在随后发表的一篇文章中，我将介绍一些关于`$ docker push`的更好的细节和选项。

## 来源

*   [Docker 标签文件](https://docs.docker.com/engine/reference/commandline/tag/#examples)

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入媒体

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)