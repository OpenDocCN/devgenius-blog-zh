# Linux 中调度任务的真实例子

> 原文：<https://blog.devgenius.io/scheduling-tasks-in-linux-with-real-world-example-a969cb687ed9?source=collection_archive---------12----------------------->

N 冰遇见你，好奇的心！✌️:在这篇文章中，我想分解你如何在 linux 中调度任务。

在很大程度上有两种方法: **cron & systemd timers** 。虽然我将把所有的时间花在前者上，但后者也是一个你可以自己探索的选择。🕺

此外，我将展示我们用来在 [Picklang](https://picklang.ml) 实现 **cron** 来为用户运行推送通知的概念。

‼️Before 潜水 in‼️，我想做一个 discalimer，我不是一个顶尖的系统管理员，但是一个软件工程师，他有 Linux 的经验，并且用它做了很多事情。这篇文章中的知识来自我自己的经验/书籍/文章/同事

> **顺便说一下**，如果你想借助 AI 学习外语，请访问 [https://picklang.ml](https://picklang.ml) 了解更多
> 
> **不是**好玩的事实😩:我花了大约一个月的时间涉猎 cron &它是与 Docker 的交互。如果我早一点阅读[大卫·柯林顿的书](https://medium.com/u/9b247ed89b9d?source=post_page-----a969cb687ed9--------------------------------) [Linux in Action](https://www.manning.com/books/linux-in-action) ，我会节省几十个小时

**文章结构:**

*   cron 概述
*   cron 的类型
*   非常重要的说明
*   在一个 docker 文件中运行 cron + Gunicorn 的示例
*   最后的话

![](img/d92d70b76bd6922f542f7a5d0abefb1f.png)

加布里埃尔·海因策在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## cron 概述

什么是 **cron** ，总而言之？它是一个内置的 linux 调度程序，如果您需要定期运行一些操作，可以利用它。➰

克朗住在哪里？你可能知道，所有的 linux 系统都源于强大的**根:/** 。那么，让我们来看一下 **linux 文件系统**的概述:

```
 / - root
       - /etc - **here everything cron related lives**
       - /var
       - /home
       - /sbin
       - /bin
       - /lib
       - /usr
```

目录中有各种目录和文件。

运行`ls /etc | grep cron`，它会吐出各种类型的&味道

## cron 的类型

**文件夹**

还有像`cron.daily`、`cron.hourly`、`cron.monthly`、`cron.weekly`、&最后是`cron.d`这样的文件夹。你需要把你的 cron 文件放在里面，linux 会执行它们。

但是`{.daily, .hourly, .monthly, .weekly}`有几个限制:

1.  文件必须没有扩展名
2.  你需要把`**#! /bin/bash**` 放在文件的开头
3.  文件必须是可执行的:`chmod +x file`

停，Linux 怎么会自己执行文件？🧐

让我们再来看看文件夹的扩展:放入带有`daily`、`hourly`等的文件，会使文件按照时间执行。

在这里，我希望你得到了要点。但是`cron.d`呢？啊，在这里你把你的文件和具体时间。怎么会？

首先参观名为**克朗塔布古鲁**:[https://www.manning.com/books/linux-in-action](https://www.manning.com/books/linux-in-action)的宏伟遗址

明白暗示了吗？您可以指定希望任务运行的频率。

例如，您可以输入`cron.d`:

`*/5 * * * * root apt update && apt upgrade`每五分钟运行一次。

> **至关重要:**不要添加任何扩展名。仅仅是`touch some_file`而已

而且，很明显，如果放在有`daily`等的文件夹里——>就不需要那些`*`

**文件**

还有 2 个文件:`crontab` & `anacron`

1.  `crontab`类似于`cron.d`文件夹

*   只玩`/etc`文件夹中的`crontab`，因为它是当前用户的`crontab`。
*   如果您想利用它来运行您的任务，您可以像我们在启动时那样做。您在`crontab`中指定文件周期，就像在普通 cron 中一样，然后指定要执行哪些文件:

```
01 * * * * root run-parts /etc/cron.hourly
```

2.`anacrontab`是有点不同类型的野兽。下面我来写一下。

如果您不确定您的服务器/机器是否会在 cron 文件中指定的时间启动并运行，该怎么办？不好…😮‍💨但并非不可收拾。救援来了。它允许在启动的特定时间后运行某些东西**。**

😶‍🌫️ **题外话**:您可以在`crontab` & `anacrontab`文件中为上述目录指定特定的逻辑:

```
*/1 * * * * root run-parts /etc/cron.hourly
```

所以，`cron.hourly`里面的文件会被运行

然后，你可以列出所有现存的克隆人`crontab -l` &编辑他们`crontab -e`

😶‍🌫️

回到`anacrontab`:有 2 个数字，而不是 5 个。当我们谈到开机后的时间**时，您需要指定:**

*   间隔天数:`0 || 7`是星期天，星期一是`1`
*   启动后的时间间隔:分钟

`1 10 someJob /home/user_name/some_script.sh`:每周一开机十分钟后运行。`someJob`是一个**作业标识符**，它对于每个 cron 必须是唯一的。

您可能会唱反调，说:“如果我们在**anacrontab**&**crontab**中放两个相同的行会怎么样？”我会及时给你答案:" **Anacrontab** *比普通 **crontab** 更流行*"

## 非常重要的说明

如果你想用时间运行一些东西:输入`cron.d`。如果你想有`.daily`、`.weekly`等等:放在相应的文件夹里。

然后，如果您想让文件在引导后以特定的间隔运行:`anacrontab`。您可以在其他目录中指定文件/脚本。

另外，请阅读下面关于“serverfault”的关于`cron.d`的帖子，以及 Linux 如何在不使用`crontab`的情况下发现那里的文件:

[](https://serverfault.com/a/358430) [## cron.d 如何通知系统？

### 感谢您对服务器故障的回答！请务必回答问题。提供详细信息并分享…

serverfault.com](https://serverfault.com/a/358430) 

和另一个关于“unix.stackexchange”上的`crontab` & `cron.d`的区别:

[](https://unix.stackexchange.com/a/417327) [## cron.d(如/etc/cron.d/)和 crontab 有什么区别？

### 感谢您对 Unix & Linux 堆栈交换的回答！请务必回答问题。提供…

unix.stackexchange.com](https://unix.stackexchange.com/a/417327) 

## 使用 Dockerfile 运行 cron 的 Picklang 示例

在启动阶段，我们决定结合 Django 管理命令和 cron 向用户发送推送。怎么做到的？🤔

> PS:我不能透露真正的代码或片段，这就是为什么我在这里将使用分离的例子

1.  `management/commands`中的`.py`文件现在被认为是一个可行的管理命令。在文件中，我们有根据标准选择用户的逻辑。然后我们利用 APNS 来推动。
2.  创建**纯文本**文件，我在其中放置 cron 的逻辑:

*   这里`*`的内容是每 45 分钟运行一次 cron。
*   然后，输入`app` dir。为什么？- >在 **Dockerfile** 中默认全部使用`app`
*   接下来，利用 Dockerfile python3 中安装的
*   `python3 manage.py send_push`是调用一个 *Django 管理命令*
*   那个奇数`> /proc` …是标准输出或标准输出

3.Dockerfile 示例:

1.  将纯文本文件复制到`cron.d`:第 3 行
2.  指定权限并向 crontab 注册我们的 cron
3.  用&在后台运行 cron
4.  用&&把它和 gunicorn 粘在一起运行 app

## 最后的话🙌

我希望你能得到一些对你的工作有帮助的东西& side-projects⚡️

此外，您可以研究所谓的`systemd timer`，它是 **cron** 的模拟，但是具有更大的灵活性。🤸‍♂️

你能找到我🔎：

*   领英:[www.linkedin.com/in/sleeplesschallenger](http://www.linkedin.com/in/sleeplesschallenger)
*   GitHub:[https://github.com/SleeplessChallenger](https://github.com/SleeplessChallenger)
*   leet code:[https://leetcode.com/SleeplessChallenger/](https://leetcode.com/SleeplessChallenger/)
*   电报:@无眠挑战者