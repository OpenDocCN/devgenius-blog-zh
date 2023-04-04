# 如何用 SASpy 免费运行 Jupyter 笔记本中的 SAS

> 原文：<https://blog.devgenius.io/how-to-run-sas-in-jupyter-notebooks-with-saspy-for-free-6dad72f198fb?source=collection_archive---------1----------------------->

底部有一些代码

![](img/b3ae4a2d58fb1c7d124ec50f229adfa8.png)

# 小介绍

如果你曾经使用过 SAS ，那是因为你足够幸运(或不幸)为一家拥有你可以使用的执照的公司工作或去了一所学校。对于我们其余的人，我们使用 python、R 或 SQL 来满足我们的数据需求。

直到过去几周，我才开始研究 SAS——我是一名数据工程师，我只使用 Python 或 SQL 处理与数据相关的任何事情。但是，在和一个已经使用 SAS 超过 20 年的客户一起工作时，我不得不学习一些相关知识。

首先，让我告诉你，SAS 是一个很大的解决方案。他们甚至有[云方案](https://www.sas.com/en/whitepapers/mit-how-ai-changes-rules-111222.html?utm_source=google&utm_medium=cpc&utm_campaign=platform-us&utm_content=GMS-126091&keyword=%252Bsas+%252Bcloud+%252Bsoftware&matchtype=b&publisher=google&gclid=EAIaIQobChMI_6im-oag7AIVi4bACh18UAYcEAAYASAAEgJLiPD_BwE)。但是作为一个 31 岁的数据迷，我看到的主要问题是它不是开源的。和我一起工作过的大多数人都不会愿意为开源的替代方案付出任何金钱。这说不通。但是我内心的黑客决定质疑你是否真的需要为 SAS 付费…

# 欢迎萨斯皮

我没有对这个库做大量的研究，但是对于所有的意图和目的， [SASpy](https://support.sas.com/en/software/saspy.html) 它是一个 python 库，允许你创建一个 [SAS 会话](https://communities.sas.com/t5/Administration-and-Deployment/What-does-it-means-the-sas-session/td-p/202413)并运行 SAS 代码。我在 jupyter 笔记本上复制了这个，但是我想你可以在任何编辑器上运行它。

![](img/55a701e6d0374ec93008c29b70ba6547.png)

直接取自 SAS 网站

# 但是首先，要求

您可能已经猜到，每年收取数千美元许可费的东西不会只有开源选项。当我在谷歌上搜索“如何免费使用 sas”或类似的东西时，这就是我想出如何免费获得 sas 的第二阶段开始了。我找到了一个名为*“如何免费获得 SAS 软件|在不安装的情况下使用 SAS 100%工作”*的 [youtube 视频](https://www.youtube.com/watch?v=-LjE4aRz2qk)，并认为“这正是我要找的标题——但视频封面图像至少可以说是可疑的。但我还是咬了一口，尝试了大学时代的生活。

![](img/c679822afdeb0fc6a3b846bf4395beeb.png)

是的，这真的是视频封面图片

# 长话短说

那家伙没撒谎。你只需在 [SAS On Demand](https://www.sas.com/en_us/software/on-demand-for-academics.html) 网站上注册，他们会给你发一封新用户名的电子邮件，然后你使用刚刚创建的用户名和密码登录，你就可以登录了。此时，您可以毫无问题地使用 SAS Studio。您使用的 SAS 版本的功能可能会有一些限制，但至少您可以进行一些数据操作。但是如果你想用 SASpy…

# SASpy 设置

先别跑，只有三步远。使用 SAS On Demand 配置文件登录后，您将有两种选择:1 .一个用 SAS Studio，一个用 SASpy。很明显我会选择 SASpy。一会儿我会给你看一些代码。但是，为了使用 SASpy，你必须先[做这些事情](https://support.sas.com/ondemand/saspy.html)(创建两个文件):

*   创建 **sasacfg_personal.py** ,其中包含基于您的 SAS on demand for Academics Home Region 的以下信息。
*   使用下面的作为模板创建 **_authinfo** (。如果在 Linux 或 MacOS 上，则为 authinfo)。有关此主题的更多信息，请参见 saspy 说明。

虽然这个[指令页](https://support.sas.com/ondemand/saspy.html)的格式让你更想去死，但完成整个过程只需要 3 分钟。

# 现在是有趣的事情

## 直接取自我昨天做的一个笔记本:

![](img/74a81a3902ce7bacd0cb994b6bd0226e.png)

加载库并创建数据框架

## 太好了，你打算怎么处理那个数据帧？

不多，我不懂 SAS。但是我可以查找一些基本的函数，比如`means()`并使它成为平均降雨量。

![](img/3cc8cac68b3b1e5ea8aeb94b31e4e995.png)

Aww yisss

## 给我看看其他的

很高兴你问了——我喜欢人们问我我知道答案的问题。最近，有人问我一些问题，我甚至不确定问题中所有单词的意思。"这间公寓的平方根是多少？"—等等什么？顺便说一句，如果你得到了证明，我们应该是朋友。

![](img/d8d154f26d77e3aed417f254956c8273.png)

这是我见过的条形图

## 你还有什么？

此时我只是在自言自语。从未真正关注过这些文章的影响力。10 个人？50 个人？更多？这仅仅意味着这些人可以读到一篇非常非常规的关于书呆子的文章。不客气

![](img/aba72162bab4ed9e1f833a2819f5ee23.png)

漂亮吧

## 如果这不是你见过的最好看的热图，你应该离开

![](img/4c54379860362f21df7d6db1ee4ad635.png)

没错

# 综上

因为我真的需要回去工作了，你已经学会了设置一个 SASpy 并在 jupyter 笔记本中使用它。你应该为自己感到骄傲。如果你今天没有完成任何事情，也不要担心。

最大

[![](img/f0e8054c2c28c51cdbe634a049db49e5.png)](https://www.buymeacoffee.com/31yearoldmoron)