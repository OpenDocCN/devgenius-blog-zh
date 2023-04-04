# 与团队合作时最强大的 Git 命令

> 原文：<https://blog.devgenius.io/the-most-powerful-git-command-when-working-with-teams-4853a5941a6b?source=collection_archive---------30----------------------->

## Git 上游简介

![](img/ee3842b0e3ddaa149ec209043fcf8c9c.png)

卢克·切瑟在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

你正准备合作一个项目，你非常兴奋，因为这可能是你一直期待的突破。您之前参加了 GIT 的速成班，您已经准备好了，或者您认为是这样。如果这听起来像你，那么这篇文章绝对适合你。

## 什么是 GIT 上游，你为什么要关心它？

为了解释什么是 git upstream，我将使用一个类比:您正在与一个团队一起工作，并且您被分配从事一个特性的工作。是的，分叉项目很容易让您做出贡献，但是如果您没有将那些变更发送到上游(没有将它们发送回父分支)会怎么样呢？这就是 git upstream 的用武之地。

## 入门指南

PS:我假设你已经把你的回购分支并克隆到你的本地计算机上了。

首先，我们验证我们有一个远程设置。

**git remote-v**

***origin git @ bit bucket . org:my-user/some-project . git(fetch)***

***原点 git @ bit bucket . org:my-user/some-project . git(push)***

接下来，我们添加一个上游。

***git 远程添加上游 git @ bit bucket . org:my-user/some-project . git***

接下来，我们验证我们已经添加了上游。

***git remote -v***

***origin git @ bit bucket . org:my-user/some-project . git(fetch)***

***原点 git @ bit bucket . org:my-user/some-project . git(push)***

***上游 git @ bit bucket . org:some-gate keeper-maintainer/some-project . git(fetch)***

***上游 git @ bit bucket . org:some-gate keeper-maintainer/some-project . git(push)***

**注意**:我们总是必须确保我们的本地主分支总是与上游存储库同步。为此，运行

**T43【git 结账大师】T44**

***git 合并上游/主***

现在，我们可以创建我们的特征分支。

***git checkout -b【你的功能】***

## 发布我们的更改

祝贺您，您已经完成了我们的专题，可以发布您的专题了。我们可以通过简单地推送我们的代码来做到这一点。

***git 推原点 ft-login***

## **每日提示**

如果我们因为来自上游维护者的反馈而想要对我们的特性进行更改，有几种方法，但我个人更喜欢合并来自上游的更新并在分支上工作。

你已经学会了一个非常强大的 Git 技巧，现在你可以和团队合作了。如果你觉得这篇文章有帮助，请分享，不要忘记留下反馈，这样我可以更好地为你服务。

快乐编码: )