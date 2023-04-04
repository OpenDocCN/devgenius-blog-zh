# GIT 简介(第二部分)

> 原文：<https://blog.devgenius.io/introduction-to-git-part-ii-ed439abcc6ed?source=collection_archive---------18----------------------->

![](img/051ed45e28317de769551d3b9b230659.png)

在本文中，我将带您浏览我的 **Git** 教程文章的第二部分。点击 https://blog.devgenius.io/introduction-to-git-785f23d6beaf[看我文章的第一部分。](/introduction-to-git-785f23d6beaf)

#git rebase，git push，git pull，git reset，remote repo 等。

让我们开始吧…

```
git branch
# git branch <Medium> ..(creates a new branch called medium)
```

**Git 分支**:****'**Git branch '命令列出了您的存储库中的所有分支。为了能够创建一个新的分支，只需在 git branch 命令中添加一个参数来创建一个新的分支，如上所示。**

```
git checkout <Medium>
git checkout -b <Medium>
```

****Git checkout** :上面的命令将从一个分支检验到另一个分支，在我们的例子中是‘Medium’**

****Git checkout -b:** 这个创建并签入到新创建的分支中，放下-b 标志来签出一个现有的分支。**

```
git merge <branch> 
# e.g git merge Medium
```

****Git merge:** 这将两个分支的内容合并到当前分支中。**

# **远程存储库**

**远程存储库是基于 web 的项目版本，或者只是托管在互联网上某个地方的一个 repo。**

**现在，当您想要在本地和远程 repo 之间添加或创建连接时。运行下面的命令。**

```
git remote add <name> <url>
# e.g git remote add origin https://github.com/Emediong-Samuel/demo-repo.gitgit pull <remote>
```

****Git 远程添加**:这将创建一个到远程存储库的新连接。添加遥控器后，您可以将其用作其他命令的快捷方式。**

****Git pull:** 获取当前分支的指定远程副本，并立即将其合并到本地副本中。**

```
git push <remote> <branch> # e.g git push -u origin main
```

****Git Push:**Git Push 命令会将您的代码推送到您的远程 repo，也就是分支，以及所有必要的提交和对象。如果不存在，它还会在远程回购中创建一个命名分支。**

**为了全面了解远程仓库的来龙去脉，点击[https://docs.github.com/en](https://docs.github.com/en)查看 Github 的文档页面。**

# **GIT 重置**

**这是更有趣的地方。在提交不完整的代码或文件时，我们会在某个时候犯错误，并且可能想要撤销该操作，添加更多的细节或纠正错误，然后再次提交。**

**然而，这可以通过 git reset 命令来完成。**

```
git reset --soft HEAD~1
# resets head by 1
git reset --hard HEAD~1
# be clear about the differences before using.
```

***注意:在处理“git reset”时，确保使用正确的命令，以实现您想要的目标。***

****Git reset — soft HEAD~1** :该命令将从当前分支中移除最后一次提交，但文件更改仍将保留在您的工作树中。此外，这些更改将保留在您的索引中，因此使用 git commit 将创建一个与您之前“删除”的提交具有完全相同的更改的提交。**

****Git reset- -head HEAD~1** :当你使用`--hard`的时候，所有的修改都会从你的本地 Git 库，(暂存区)和工作目录中移除。因此，在使用`--hard`之前，一定要确定和清楚你是否也想从工作目录中删除变更。**

# **撤消远程提交**

**无论出于何种原因，如果您想要撤销已经推送到远程存储库的提交。您应该使用下面的 git 命令。**

```
git revert
```

**revert 命令将在本地撤销它，并将更改推送到远程分支。在这样做之前，使用`git reflog`获取提交散列。**

```
git reflog
```

**然后还原它。假设您的提交散列是 1234567E，您将执行以下操作:**

```
git revert 1234567E
```

**然后将更改推送到您的远程分支。**

***就是这样伙计们，感谢你们坚持到最后。祝你在学习旅途中一切顺利。***

***干杯！***