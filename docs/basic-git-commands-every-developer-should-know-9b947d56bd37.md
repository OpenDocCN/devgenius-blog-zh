# 每个开发人员都应该知道的基本 Git 命令

> 原文：<https://blog.devgenius.io/basic-git-commands-every-developer-should-know-9b947d56bd37?source=collection_archive---------10----------------------->

![](img/df8bb19c9494be510dbf608e126fa633.png)

基本 Git 命令

嘿，各位，作为开发人员，我们大部分时间都在和 git 打交道。作为初学者，使用终端/命令来使用 git 可能会有点吓人。这完全没问题。git 中有大约 160 个命令，学习所有这些命令对于开发人员来说是一场噩梦。我将讨论您将在日常工作流程中使用的 10–15 个基本 git 命令。

**以下是您在日常工作中需要的 Git 命令:**

**1。git 配置**

这些命令分别设置提交时使用的作者姓名和电子邮件地址。

**怎么用？**

*   在查看版本历史记录时设置可识别的名称。

```
**git config — global user.name “username”**
```

*   设置将与每个历史标记关联的电子邮件地址。

```
**git config — global user.email “email”**
```

**2。git init**

这是用于将现有目录初始化为 git 存储库的第一个命令。

**怎么用？**

```
**git init**
```

**3。** **git 添加**

此命令将文件添加到临时区域。

**怎么用？**

该命令有三种用法:

*   添加特定文件

```
**git add [file_name]**
```

*   添加特定目录

```
**git add [directory_name]**
```

*   添加所有未转移的文件

```
**git add .**
```

**4。git 提交**

该命令用于保存本地存储库中的更改。每次提交都有一个惟一的 hashcode 供您参考。它允许您为每次提交编写一条消息，这被认为是使用 git 时的最佳实践。

**怎么用？**

```
**git commit -m “your commit message goes here”**
```

**5。git 状态**

该命令用于检查您的存储库的当前状态。

**怎么用？**

```
**git status**
```

**6。git 分支**

该命令用于执行与分支相关的任务，如列表、创建和删除。

**怎么用？**

*   获取存储库所有分支的列表。

```
**git branch -a**
```

*   创建新的分支。

```
**git branch** **[branch name]**
```

*   删除分支。

```
**git branch -d [branch name]**
```

7。git 检验

该命令用于在分支之间进行切换。

**怎么用？**

*   切换到现有分支。

```
**Git checkout [branch name]**
```

*   切换到新的分支。

```
**Git checkout -b [branch name]**
```

**8。git 合并**

该命令用于将一个分支整合或合并到另一个分支中。

**怎么用？**

```
**git merge [branch]**
```

**9。git 遥控器**

该命令用于将本地存储库连接到远程存储库。

**怎么用？**

```
**git remote <command> <remote_repository_name> <remote_repository_URL>

# or if you don't want to set a name:
git remote <command>  <remote_repository_URL>**
```

**注意—** 您可以使用以下命令代替命令:

*   增加
*   去除
*   重新命名
*   设置头
*   设置 url
*   set-URL-添加或-删除
*   设置分支
*   获取 URL
*   减少
*   列出附加到项目的所有 URL。

```
**git remote -v**
```

**10。git 克隆**

此命令用于创建现有远程存储库的本地副本。

**怎么用？**

```
**git clone [URL]**
```

**11。git pull**

该命令用于获取对远程存储库所做的最新更改。

**怎么用？**

```
**git pull branch_name**
```

**12。git 推送**

该命令将本地所做的更改推送到远程存储库。

**怎么用？**

```
**git push origin [branch-name]**
```

**13。git stash**

存储在本地回购中所做但未提交的更改。当您不想提交所做的更改，而是希望将它们存储在本地时，可以使用此命令。

**怎么用？**

*   保存已修改和暂存的更改。

```
**git stash -u**
```

*   列出隐藏文件更改的堆栈顺序。

```
**git stash list**
```

*   取回临时存储的文件。

```
**git stash pop**
```

*   删除临时存储、修改和跟踪的文件。

```
**git stash clear**
```

**14。git 日志**

该命令用于按时间顺序显示存储库的提交历史。

**怎么用？**

```
**git log**
```

**15。git rm**

此命令从您的工作目录中删除文件，并分阶段删除。

**怎么用？**

```
**git rm [file]**
```

16。git rebase

该命令用于在指定分支之前应用当前分支的任何提交。如果您需要重写项目的历史，可以使用它。

**怎么用？**

```
**git rebase [branch-name]**
```

17。git 回复

该命令用于恢复一些现有的提交。

**怎么用？**

```
**git revert <insert bad commit hashcode here>**
```

18。git 复位

这个命令是关于更新您的分支和移动 tip，以便在分支中添加或删除提交。

**怎么用？**

*   该命令用于删除提交。

```
**git reset [hashcodeId]**
```

***注-*** *哈希码 Id 可以从 git 日志中看到*

*   用于在保留工作目录中的更改的同时取消文件的暂存。

```
**git reset [file]**
```

19。git 恢复

该命令是关于从索引或另一个提交中恢复工作树中的文件。此命令不会更新您的分支。该命令还可用于从另一次提交中恢复索引中的文件。

**怎么用？**

```
**git restore -SW**
```

**20。git 差异**

该命令是一种广泛使用的跟踪更改的工具。

**怎么用？**

*   此命令用于查看已更改内容的差异，但不用于暂存。

```
**git diff**
```

*   此命令用于查看暂存内容与未提交内容之间的差异。

```
**git diff --staged**
```

*   该命令用于查看两个分支之间的差异。

```
**git diff Branch A...Branch B**
```

就这些，谢谢你的阅读。我希望这篇博客能帮助你理解基本的 git 命令。你可以把这个博客收藏起来，以备将来需要时参考。

如果你有任何问题或反馈，请在下面的评论区告诉我。我们还可以通过[**Twitter**](https://twitter.com/ApoorvD72358398) 和 [**LinkedIn**](https://www.linkedin.com/in/apoorv-dubey-8a3a31191/) 了解更多我的科技之旅。