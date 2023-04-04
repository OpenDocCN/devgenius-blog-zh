# 使用 Git 别名加速您的开发和 Git 工作流

> 原文：<https://blog.devgenius.io/speed-up-your-git-workflow-with-git-aliases-4a725e2930d9?source=collection_archive---------4----------------------->

![](img/b76db393d1085cba961ae665219a8b7b.png)

艾伦·坚硬人在 [Unsplash](https://unsplash.com/photos/Y-hl3rx5fr8) 上的照片。

对于许多开发人员来说，您一遍又一遍地运行相同的命令——一天多次变成一周、一个月、一年和一个职业生涯的多次，这会占用您可以用来做其他任务的宝贵时间。

在小范围内，这些击键并不重要，但在长时间内，它们肯定很重要。弄清楚如何减少打字量是一件值得做的事情——就像他们过去常说的那样(或许现在仍然如此？)，“及时一针省九针。”

# Git 命令

如果您正在使用版本控制软件(您可能正在使用),并且您正在使用 Git(您可能正在使用),那么您可能非常熟悉如下命令:

*   git 拉
*   git 检验
*   git 状态
*   git 提交
*   git 推送

…以及更多。除了肌肉记忆之外，你甚至可以非常快速地把这些打出来。

话虽如此，只是因为你真的擅长并快速地打出这些，并不意味着速度的提高不能实现，也不值得。事实上，对这一点的追求是如此的令人鼓舞，以至于 Git 本身有内置的功能来支持限制击键。

# Git 别名

输入 Git 别名。这些基本上是您最喜欢的 Git 命令的快捷方式。您可以设置它们，Git 将能够记住它们以便将来执行。

假设您想为`git pull`输入一个别名。您想使它更简单，所以您可以在您的终端中运行以下命令:

```
git config --global alias.pu pull
```

现在，你可以运行`git pu`，效果就像你运行`git pull`一样！

更一般地说，您的别名命令是这样的格式:

```
git config --global alias.yourAliasHere whatGitCommandYouWantToAlias
```

这里还有一些例子:

## git 检验

您可以运行以下命令来创建别名:

```
git config --global alias.co checkout
```

这种用法可能如下所示:

```
git co origin master
```

这与跑步是一样的:

```
git checkout origin master
```

你已经可以看到一些保存的按键。

## git 分支

您可以运行以下命令来创建别名:

```
git config --global alias.br branch
```

然后你可以运行:

```
git br
```

这与跑步是一样的:

```
git branch
```

## git 提交

您可以运行以下命令来创建别名:

```
git config --global alias.com commit
```

然后你可以运行:

```
git com
```

这与跑步是一样的:

```
git commit
```

## git 状态

您可以运行以下命令来创建别名:

```
git config --global alias.st status
```

然后你可以运行:

```
git st
```

这与跑步是一样的:

```
git status
```

# 在哪里检查您的 Git 别名

如果您忘记了命令别名，您可以随时使用您的终端并在`/.gitconfig`处读取文件，以查看:

```
[alias]
        co = checkout
            br = branch
            com = commit
            st = status
```

您总是可以直接编辑该文件，而不是运行上述命令；这可能比一遍又一遍地运行这些命令更好，这也是您保存这些命令的全部原因！

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入媒体

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)