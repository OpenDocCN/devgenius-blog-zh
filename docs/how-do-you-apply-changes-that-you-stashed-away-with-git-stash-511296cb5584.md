# 你如何应用你用 git stash 藏起来的改变？

> 原文：<https://blog.devgenius.io/how-do-you-apply-changes-that-you-stashed-away-with-git-stash-511296cb5584?source=collection_archive---------3----------------------->

![](img/0970088b39a61900a72079fe04d958f8.png)

[Yancy Min](https://unsplash.com/@yancymin) 在 [Unsplash](https://unsplash.com/photos/842ofHC6MaI) 上拍照。

如果您在这里，您可能已经用`git stash`保存了一些本地更改。

不确定`git stash`是什么？让我们参考一下[官方 Git 文档](https://git-scm.com/docs/git-stash):

> 当你想记录工作目录和索引的当前状态，但是想回到一个干净的工作目录时，使用`git stash`。该命令保存您的本地修改并恢复工作目录以匹配`HEAD`提交。

很好。所以我们知道我们想要保持当前工作目录的变化，而`git stash`可以帮助我们。

不过，在这一点上，你可能想知道一旦你真正进入`git stash`，那些变化发生了什么。

如果你不知道下一步该做什么，工作目录的旧状态似乎已经消失了。但是不要担心:您仍然可以访问它们。

首先，要查看您隐藏的修改，您可以运行`git stash list`。

想检查他们吗？运行`git stash show`。

最后，也可以说是最重要的:`git stash apply`。这样，您实际上将在当前提交的基础上应用隐藏的更改。这是非常有用的，让你的修改进入你正在做的任何事情，而不必做一些手动的代码更改来恢复它们。

如果你想要一个真正深入的答案，那么我推荐这个由 torek 提供的[特别优秀的 StackOverflow 答案。不过，正如他们提到的那样:“**幸运的是**，使用 `git stash`你实际上不需要理解所有这些”——这就是你可以使用`git stash apply`的地方。不过，知道大致发生了什么总是一个好主意！](https://stackoverflow.com/a/20412685)

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入媒体

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)