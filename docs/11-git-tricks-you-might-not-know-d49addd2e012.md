# 你可能不知道的 11 个技巧

> 原文：<https://blog.devgenius.io/11-git-tricks-you-might-not-know-d49addd2e012?source=collection_archive---------1----------------------->

## GIT |版本控制|技巧

## 以下是你还不知道的 11 个 Git 技巧

![](img/4deff8a92d96bc1af14bdcd6ad2406d5.png)

照片由[克里斯蒂娜·莫里洛](https://www.pexels.com/@divinetechygirl?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从 [Pexels](https://www.pexels.com/photo/woman-sitting-in-front-of-laptop-1181287/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 拍摄

**不是**，不是`***git reflog***`。**不**，不是`***git commit --ammend***`。

*还有哪些你应该知道的 Git 招数？*你可以找到你的历史。您可以获取拉取请求。您可以在不重置的情况下撤消提交。所有这些都是通过 Git CLI 实现的。

让我们开始吧。

# 如何为自己的下一次单口相声争取一个好的历史？

***用*** [***单口相声***](https://github.com/tj/git-extras/blob/master/Commands.md#git-standup) ***。*** 从某些开发者那里寻找提交。您可以按年龄和姓名进行筛选。

使用 git standup 获取您的提交历史记录

要查找昨天的提交，请使用`git standup`。这将返回您最近提交的内容——非常适合每日站立。更多信息请使用`git help standup`。

# 如何把 PR 拉到你当地？

***对于 GitLab 使用*** `[***git mr***](https://github.com/tj/git-extras/blob/master/Commands.md#git-mr)` ***，对于 GitHub 使用*** `[***git pr***](https://github.com/tj/git-extras/blob/master/Commands.md#git-pr)` ***。*** 这对代码评审来说很棒。此命令不适用于 BitBucket。更容易地获取 PR、检查变更和审查代码。

**如何使用*命令？*** 运行`[git pr <PR-Number>](https://github.com/tj/git-extras/blob/master/Commands.md#git-mr)`

从 GitHub 获取 PR

# 如何给`.gitignore`添加一个文件？

*你知道如何添加到* `*.gitignore*` *文件吗？* ***用*** `[***git ignore***](https://github.com/tj/git-extras/blob/master/Commands.md#git-ignore) ***<filename>***` ***。***

将文件添加到。gitignore 而不改变。基特尼奥勒

# 如何重置最后 N 次提交？

***使用*** [***git 撤消***](https://github.com/tj/git-extras/blob/master/Commands.md#git-undo) ***。*** 撤销上一次提交。您不需要重置为以前的。你根本不需要重置。使用`git undo 4`重置最后 4 次提交。

使用 git-extras 中的 git undo 重置提交

# 怎么上你最近的分公司？

[*要不要上一个分公司？*](https://medium.com/r?url=https%3A%2F%2Fstackoverflow.com%2Fquestions%2F7206801%2Fis-there-any-way-to-git-checkout-previous-branch%2F7207542)

```
git checkout -
# or
git switch -
```

# 你想要一个详细的 Git 日志吗？

*用* `*git log*` *还需要更多细节吗？*你可以使用`[git log --stat](https://git-scm.com/docs/git-log#Documentation/git-log.txt---statltwidthgtltname-widthgtltcountgt)`命令。该命令显示了许多插入和删除操作。

*你只需要看文件吗？*可以用`[git whatchanged](https://git-scm.com/docs/git-whatchanged)`。

# 如何打印出好的 Git 图？

*需要看 Git 图吗？*使用`[git show-tree](https://github.com/tj/git-extras/blob/master/Commands.md#git-show-tree)`。

如果不想使用 git-extras，可以创建自己的树。

第一行是自定义树命令

# 如何从远程分支机构丢弃和拉取？

您想要放弃本地更改吗？你想用力拉吗？

下面是如何用 git-extras 的`[sync](https://github.com/tj/git-extras/blob/master/Commands.md#git-sync)`来做这件事。

如何用普通的 Git 命令做同样的事情？您需要对上游分支进行硬复位。自变量`[@{u}](https://stackoverflow.com/questions/19474577/what-does-the-argument-u-mean-in-git)`是上游分支。

# 如何从您不在的分支中提取更改？

*如何一边拉* `*develop*` *一边上* `*feature*` *分支？你可以这样做。*

`git checkout dev`
`git pull`


这是更好的方法。
`git fetch origin develop:develop`

# 如何删除文件和相关提交？

可以用`[git obliterate](https://github.com/tj/git-extras/blob/master/Commands.md#git-obliterate)`。这将从存储库和相关提交中删除该文件。

# 如何修复特定的提交？

你知道如何修改提交。这将改变存储库中的最后一次提交。

您可以使用`git commit --fixup <SHA>`来修复特定的提交。然后使用`git rebase -i --autosquash <SHA>`来压缩这些修正。

这里有一个别名来加快速度。

# 如何找到修改最多的文件？

***用*** [***git 努力***](https://github.com/tj/git-extras/blob/master/Commands.md#git-effort) ***。*** 该命令给出了文件变动的良好度量。活动天数将显示修改花费了多少时间。

使用 git 努力找到大多数搅动的文件

大部分提到的命令都需要 [**git-extras**](https://github.com/tj/git-extras/blob/master/Installation.md) 。Git extras 使 Git 变得更好、更少威胁、更快。