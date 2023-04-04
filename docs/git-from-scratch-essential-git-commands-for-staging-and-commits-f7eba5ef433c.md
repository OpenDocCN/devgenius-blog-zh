# 从头开始 Git:用于暂存和提交的基本 Git 命令

> 原文：<https://blog.devgenius.io/git-from-scratch-essential-git-commands-for-staging-and-commits-f7eba5ef433c?source=collection_archive---------10----------------------->

## 第 04 条

![](img/a7f019b4afa37b5b5872a4da228fca76.png)

从这里开始，从 **Git 从头开始**系列文章开始，我们将讨论基本的 Git 命令。本文将讨论一些用于暂存和提交的 Git 命令。您可以使用 Git bash 来尝试这些命令。如果对以下术语不熟悉，可以参考我的 [**Git 从头开始:基础 Git 术语篇**](https://medium.com/@senevirathnehu/git-from-scratch-basic-git-terminologies-2b8c663ea592) 。所以我们走吧...

## 1.Git 初始化

**$ git init**

Git init 命令用于创建一个新的空存储库，或者将现有的工作项目重新初始化为 Git 存储库。

在运行了 *$ git init* 命令之后。git 子目录，包含所有的元数据，它将在工作目录中创建。

因此，在创建 git 存储库之后，您可以使用以下命令将其与远程存储库连接:

**$ git 远程添加*remote repourl***

## 2.Git 添加

该命令用于向索引添加文件上下文。git add 命令将当前工作树内容更新到 staging，并为下一次提交做准备。

要添加单个文件:

**$ git 添加*文件名***

要添加所有文件:

**$ git 添加。**

## 3.Git 提交

**$ git 提交**

Git commit 命令记录对存储库的更改。

要提交消息:

**$ git commit-m " c*ommitMessage*"**

要更改提交消息，请执行以下操作:

**$ git 修正**

## 4.Git 克隆

**$ git 克隆 *repositoryURL***

我们使用这个命令将一个存储库克隆到一个新目录中。

要将存储库克隆到特定的本地文件夹，请执行以下操作:

**$ git 克隆*储存库 yURL* "f *旧路径* "**

要克隆分支:

**$ git clone-b*branch name repository URL***

## 5.Git 标签

此命令创建、列出、删除或验证标签。

要创建标签:

**$ git 标记*标记名***

要获取标签列表:

**$ git 标签**

## **6。Git Stash**

**$ git stash**

您可以使用 git stash 命令切换分支，而无需提交当前分支。

隐藏信息:

**$ git stash save*stash message***

要获取藏匿清单:

**$ git 藏匿清单**

要重新应用隐藏的更改:

**$ git stash apply**

或者你可以用

**$ git stash pop**

这两个操作的主要区别在于 stash pop 在应用 stash 后将其从堆栈中删除。

要删除最近的存储:

**$ git 藏匿掉落**

要删除所有可用的隐藏:

**$ git 藏匿清除**

结尾…

让我们来看看下一篇文章。在那里，我们将看到撤销更改的命令。

快乐学习！