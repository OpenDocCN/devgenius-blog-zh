# 从头开始 Git:撤销和检查变更的基本 Git 命令

> 原文：<https://blog.devgenius.io/git-from-scratch-essential-git-commands-for-undoing-and-inspecting-changes-8c0bd0fe7570?source=collection_archive---------13----------------------->

## 第 05 条

在上一篇文章 [**从头开始 git:用于暂存和提交的基本 Git 命令**](https://medium.com/@senevirathnehu/git-from-scratch-essential-git-commands-for-staging-and-commits-f7eba5ef433c?source=your_stories_page-------------------------------------) 中，我讨论了一些用于暂存和提交的基本 Git 命令。因此，在本系列的第 5 篇文章中，我将讨论帮助撤销和检查 Git 中的更改的基本命令。如果对以下术语不熟悉，可以参考我的 [**Git 从头开始:基础 Git 术语篇**](https://medium.com/@senevirathnehu/git-from-scratch-basic-git-terminologies-2b8c663ea592) 。

首先，让我们看看撤销更改的命令。

![](img/e905919180e63900b8b385bf4c9987ea.png)

## 1.Git 检验

**$ git check out*branch name***

我们使用 checkout 在分支之间切换。

如果您想要创建一个新的分支并切换到它，您可以使用下面的命令

**$ git check out-b*branch name***

## 2.Git 删除

**$ git rm *文件名***

要删除一个文件或文件集合，我们使用 git rm。这个 rm 代表移除。

## 3.Git CherryPick

**$ git 精选 *commitId***

Git cherry-pick 用于将提交从一个分支应用到另一个分支。

## 4.Git 重置

***$ git 重置委员会 Id***

该命令将当前磁头复位到指定状态。重置后，以前的提交不会显示在日志中。它不会从日志中删除。您可以使用上面的命令再次撤消它。

接下来，我们将看到检查变更的命令。

![](img/8851850f451fde129285d8e0a6d4b4bf.png)

## 1.Git 日志

**$ git 日志**

该命令将显示所有提交日志。

**$ git 日志—在线**

该命令每行显示一个提交，包含提交 id 及其消息的前七个字符。

要退出 Git 日志，可以按 q。

## 2.Git 差异

**$ git diff**

Git diff 命令显示了提交之间的变化，提交和工作树等。

## 3.Git 状态

**$ git 状态**

这个命令将向您显示工作树的存储库和临时区域的状态。它向我们显示了跟踪的，未跟踪的文件和变化。

结尾…

希望在这个系列的最后一篇文章中与您见面。

快乐学习！