# 如何在 git 上合并两个仓库？

> 原文：<https://blog.devgenius.io/how-to-merge-two-repositories-on-git-b0ed5e3b4448?source=collection_archive---------0----------------------->

![](img/c0b2484f24ce293fb3d45708b4c54001.png)

卢克·切瑟在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

您可能想知道，为什么有人想要将两个独立的 git 存储库合并成一个。几周前，我遇到了一个问题。我和我的团队在做一个项目，不知何故，我一个人在做一个回购，而我的队友在做一个单独的回购。现在，我准备将他们的回购克隆到我的机器上，并重写或复制/粘贴我编写的代码，直到我突然想到，我可能可以在我的机器上合并这两个存储库。所以经过几分钟的研究，我找到了一个解决方案，如果你们遇到类似的问题，这可能会对你们有所帮助。

如果您在一个单独的 repo 中处理项目的组件，同时在一个不同的 repo 中处理同一个项目的模板文件，并最终想要合并这两个文件，这可能会有一点帮助。

(确保你没有将两个“截然不同”的项目合并成一个混合项目，这是非常愚蠢的)

![](img/8ffa2a139c9b059920c6b41f20b68e47.png)

考虑下面这个简单的例子。git 上有两个存储库，*比如 repo1 和 repo 2，*每个都有一个 *readme.md 文件。*然后两个合并这两个，你可以按照这些步骤:

***第一步*** :克隆其中一个库(比如 repo1)。

`git clone <*https/ssh-link-for-repo1>*`

***步骤 2*** :在这个克隆中创建另一个远程，它指向 repo 2——我们的第二个存储库。

`git remote add <your-custom-remote-name> <https/ssh-for-repo2>`

现在您将拥有以下遥控器

```
origin <repo-1-link> (fetch)
origin <repo-1-link> (push)
your-custom-remote-name <repo-2-link> (fetch)
your-custom-remote-name <repo-2-link> (push)
```

运行:`git remote -v`检查你的遥控器

***第三步:*** 将 repo2 中的内容提取到该遥控器中

`git fetch <your-custom-remote-name>`

***第四步:*** 从远程到本地分支

请注意，第二次回购的内容在远程 still 中，我们获取了它，但还不能访问它。所以我们把它复制到一个新的本地分支。

`git checkout -b <new-branch-name> <your-custom-remote-name>/master`

如果有必要的话，检查文件，看看你是否已经正确地提取了正确的数据。

***第五步:*** 与主合并。

请注意，最后一个命令让您签出主分支并签入新分支。所以，先用`git checkout master`从那到主人结账

然后运行:`git merge <new-branch-name>`

现在它可能会返回以下错误

`Fatal: refusing to merge unrelated histories`

由于我们试图合并两个独立的存储库，git 检查合并和提交历史之间的相互关系，并阻塞合并过程。

您可以在最后一个命令中使用下面的标志来超越这个限制

`git merge <new-branch-name> --allow-unrelated-histories`

***现在要考虑的事情:***

如果这些存储库中有同名文件，可能会出现合并冲突。如何解决这个问题取决于您，是手动删除还是使用 IDE 的智能来识别和保留某些版本。

我希望这有所帮助。