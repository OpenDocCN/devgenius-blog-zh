# 2 分钟的亚马逊机器映像(AMI)是什么？

> 原文：<https://blog.devgenius.io/what-is-an-amazon-machine-image-ami-in-2-minutes-6071c6985b27?source=collection_archive---------1----------------------->

![](img/0205fbf4c41a3a745796b3a3c7226685.png)

图片由 [Unsplash](https://unsplash.com/photos/XEB8y0nRRP4) 上的 [Alexandru Acea](https://unsplash.com/@alexacea) 拍摄

您可能以前听说过 AMI 或 Amazon 机器映像，并想知道它到底是什么。

在两分钟的阅读中，我们将回顾一下。

当你提出一个 EC2 实例时——这是另一篇文章的主题，但基本上是一台虚拟计算机(“云中的计算能力”，正如亚马逊[所说](https://aws.amazon.com/ec2/?ec2-whats-new.sort-by=item.additionalFields.postDateTime&ec2-whats-new.sort-order=desc))——那么你必须指定一个 AMI 来配合它。

AMI 本质上是您的服务器的快照。您可以重用它，并且知道当您将 AMI 用于另一个实例时，无论什么内容都将存在。

换句话说，它就像一个模板。

在这个模板中，你可以放入操作系统(Ubuntu，Debian，等等)。)并确保具有此 AMI 的任何其他实例将具有相同的操作系统。AMI 包含用于启动实例的信息。

您可以创建自己的 AMI 或使用公开发布的 AMI。有时你甚至可以从第三方购买。

除了操作系统，AMI 还可以指定区域、底层操作系统的体系结构、权限等等。

在以后的文章中，我们将更深入地研究 ami 的细节以及您可以用它们做的一些很酷的事情。

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入媒体

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)