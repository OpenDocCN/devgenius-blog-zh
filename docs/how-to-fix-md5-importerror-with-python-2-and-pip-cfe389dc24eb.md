# 如何用 Python 2 和 pip 修复 md5 ImportError

> 原文：<https://blog.devgenius.io/how-to-fix-md5-importerror-with-python-2-and-pip-cfe389dc24eb?source=collection_archive---------2----------------------->

![](img/6d0173bd2d65cc7229edda0616985274.png)

由[马克埃塔·马塞洛娃](https://unsplash.com/@ketdee?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/python?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

最近，当我试图在 Mac 上运行 Python 脚本时，我遇到了一个特定 Python 模块的导入错误:

# 错误

我以为解决办法会相当容易；我可以使用 Python 的安装包`pip`，然后安装`boto.sts`。然而，当我尝试运行`$ pip install boto`时，我收到了一个令人沮丧和困惑的错误:

*“好吧，”*我心想。*“我大概只需要再走一点* `*pip*` *的路程，装上* `*md5*` *。”*没有这样的运气。同样的错误——对我来说，这比什么都更能说明`pip`的问题。

# 我的解决方案

请注意，您的里程可能会有所不同；我只是想记录下对我有用的东西，因为我只是想在查找时找到关于这个问题的任何资源。

首先，我跑`$ python --version`的时候发现我的 Python 版本是 **2.7.16** 。

## 使用自制软件卸载 Python 2

别担心。您将退回到 Mac 默认的 Python 版本。我的情况是 **2.7.10** 。我不想在这里做过多的假设，但是有可能在某个时候我的机器安装了一个新版本的 Python，这个版本与`pip`有问题。

# 就这么简单

这就是我所需要的。在运行完那一条命令后，我能够很好地运行我的脚本(使用`pip`安装`boto`和一些我还没有的其他模块)。

这是一个巨大的解脱。当然，这适用于我的特定 Python 安装和我的环境，但是我想为任何其他正在寻找这个不直观问题的解决方案的紧张的开发人员添加结果。

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入媒体

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)