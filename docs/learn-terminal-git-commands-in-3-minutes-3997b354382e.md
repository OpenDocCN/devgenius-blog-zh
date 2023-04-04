# 在 3 分钟内学会终端 GIT 命令。

> 原文：<https://blog.devgenius.io/learn-terminal-git-commands-in-3-minutes-3997b354382e?source=collection_archive---------14----------------------->

## 学习基本的 git 终端命令和 GitHub。

![](img/3896c911fc81de3b92b4e37a129a3ee3.png)

照片由[卢克·切瑟](https://unsplash.com/@lukechesser?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在本帖中，我们将在 3 分钟内了解 Git——什么是 Git 及其终端命令。所以先说什么是 Git。

# GIT 是什么？

2005 年，Linus Torwalds 创建了一个 GIT。

> Git 是一个免费的开源分布式版本控制系统，旨在快速高效地处理从小到大的项目。
> 
> Git 是[易学的](https://git-scm.com/doc)并且有[微小的足迹和闪电般的性能](https://git-scm.com/about/small-and-fast)。它比 Subversion、CVS、Perforce 和 ClearCase 等配置管理工具具有更好的特性，如[廉价的本地分支](https://git-scm.com/about/branching-and-merging)、方便的[中转区](https://git-scm.com/about/staging-area)和[多工作流](https://git-scm.com/about/distributed)。

# 装置

## 苹果个人计算机

`$ brew install git`

## Linux 操作系统

`$ yum -y install git`

# 找到 Git 的版本

`$ git --version`

# 从头开始创建存储库

1.  转到项目的文件夹(您希望将其作为存储库)
2.  运行`$ git init`

或者

1.  跑`$ git init [project-name]`

它将创建具有特定名称的新本地存储库。

# 克隆现有的 git 仓库

1.  转到您想要存储库文件夹的文件夹。
2.  运行`$ git clone [url]`

# 更改

## 检查 GIT 仓库的状态

`$ git status`

该命令列出所有新文件或修改过的文件

## 将文件添加到 GIT 存储库

`$ git add [file-name]`

运筹学

如果您想添加多个文件

`$ git add .`

## 提交文件(永久存储在 GIT 中)

`$ git commit -m "Message"`

消息是对变更进行评论的好方法。

# 分支

## 创建分支

`$ git branch [new_branch]`

## 改变(开关)分支

`$ git checkout [branch_name]`

## 创建分支并切换到新分支

`$ git checkout -b [new_branch_name]`

## 列出所有分支

`$ git branch`

## 将分支合并到当前分支

`$ git merge [branch_name]`

## 删除特定分支

`$ git branch -d [branch_name]`

# 同步更改

## 将本地存储库上传到 GitHub

`$ git push -u origin master`

## 从 GitHub 下载存储库到本地机器

`$ git pull origin master`

今天我们从 [python 速成教程](https://geni.us/ew5ZE7B)一书中学习了 Python 的基础知识(第 8 章和第 9 章)。我这次 100 天机器学习挑战的目标是从零开始学习[机器学习](https://medium.com/javarevisited/my-favorite-data-science-and-machine-learning-courses-from-coursera-udemy-and-pluralsight-eafc73acc73f)并帮助其他想要开始机器学习之旅的人。很多概念我都知道，但我是从零开始，帮助机器学习社区的初学者，修正概念。

**感谢阅读！**

如果你喜欢我的工作并想支持我，我会非常感谢你在我的社交媒体频道上关注我:

*   支持我的最好方式就是在 [**中**](https://medium.com/@durgeshsamariya) 关注我。
*   订阅我的新 [**YouTube 频道**](https://www.youtube.com/c/themlphdstudent) 。
*   在我的 [**邮箱列表**](http://eepurl.com/hampwT) 报名。

以防你错过我的 python 系列。

*   第 0 天:[挑战介绍](https://medium.com/@durgeshsamariya/100-days-of-machine-learning-code-a9074e1c42c3)
*   第 1 天: [Python 基础知识— 1](https://medium.com/the-innovation/python-basics-variables-data-types-and-list-59cea3dfe10f)
*   第 2 天: [Python 基础知识— 2](https://towardsdatascience.com/python-basics-2-working-with-list-tuples-dictionaries-871c6c01bb51)
*   第 3 天: [Python 基础知识— 3](https://medium.com/towards-artificial-intelligence/python-basics-3-97a8e69066e7)
*   第 4 天: [Python 基础— 4](https://medium.com/javarevisited/python-basics-4-functions-working-with-functions-classes-working-with-class-inheritance-70e0338c1b2e)

我希望你会喜欢我的其他文章。

*   [八月月度阅读清单](https://medium.com/@durgeshsamariya/august-2020-monthly-machine-learning-reading-list-by-durgesh-samariya-20028aa1d5cc)
*   [集群资源](https://medium.com/towards-artificial-intelligence/a-curated-list-of-clustering-resources-fe355e0e058e)
*   [离群点检测资源](https://medium.com/dev-genius/curated-list-of-outlier-detection-resources-35ed27d0e46e)