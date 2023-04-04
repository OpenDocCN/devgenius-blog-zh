# Git 历史📜

> 原文：<https://blog.devgenius.io/git-history-cd85b0956dc?source=collection_archive---------3----------------------->

![](img/5f19915226bcddc00581987dc61425e1.png)

[乔·塞拉斯](https://unsplash.com/@joaosilas?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍照

版本控制系统的优点是它记录变更。这些记录允许我们检索提交之类的数据，查看谁贡献了什么，找出哪里引入了错误，并恢复有问题的更改。但是，如果我们不能驾驭它，所有这些历史都将是无用的。这就是 git log 命令的用武之地。

`git log`是一个实用工具，用于回顾和读取存储库中发生的所有事情的历史。这个命令帮助您查看过去的提交，这有助于了解谁在存储库中做了什么。多个选项可以与一个`git log`一起使用，使历史更加具体。我们可以使用`git log`命令以不同的方式列出、过滤、查看提交历史。

通过在终端中键入以下命令，可以执行一个简单的日志命令:

```
git log
```

这将在一个交互式终端中列出所有提交历史，我们可以在其中查看和导航。

```
commit b617e8c6f326d62448d7a3822d6a20346ebe1c28
Author: Milan Brankovic <user@email.com>
Date:   Fri Oct 25 17:15:09 2019 +0200

    Commit #3

commit 70c191e98844c0b3d6d26b83c7142569eb5d58c4
Author: Milan Brankovic <user@email.com>
Date:   Fri Oct 25 11:18:40 2019 +0200

    Commit #2

commit ff589ff46d272d9dd90eb6d33d618ad78f72de6f
Author: Milan Brankovic <user@email.com>
Date:   Fri Oct 25 11:14:34 2019 +0200

    Commit #1

commit 1d47128a13818b8fc0c3d919d8d62d129744a168
Author: Milan Brankovic <user@email.com>
Date:   Fri Oct 25 11:11:12 2019 +0200

    Commit #0
```

如您所见，不仅显示了带有提交标识符的提交历史。这些提交以*倒序顺序显示*(最后一次提交将显示在顶部)。它还包括各种其他信息，这些信息在不止一个人使用存储库时非常有用。提交的运行日志包含以下信息:

*   **提交哈希**，这是一个由 SHA1(安全哈希算法)算法生成的 40 字符校验和数据。这是一个唯一的号码。
*   **提交作者元数据**:提交修改的作者的信息，如作者姓名和电子邮件。
*   **提交日期元数据**:这个部分显示了提交的日期和时间以及日期和时区。拥有日期和时区非常重要，因为许多人在不同的地理位置从事同一个项目。
*   **提交标题/消息**:提交消息中给出的提交概述。

默认日志对于快速查看存储库中刚刚发生的事情非常有用。但是它会占用很多空间，而且你一次只能看到少量的提交。

# 使用 Git 日志

在接下来的章节中，我们将学习如何最好地利用`git log`命令。

## 导航日志

Git 使用非终端分页器来分页查看提交历史。您可以使用以下命令导航它:

*   若要向下滚动一行，请使用 j 或↓
*   若要向上滚动一行，请使用 k 或↑
*   要向下滚动一页，请使用空格键或 Page Down 按钮
*   要向上滚动一页，请使用 b 或 Page Up 按钮
*   要退出日志，请使用 q

## 如何查看浓缩日志？

如果我们只需要用作者提供的注释列出提交 id 的唯一部分，我们可以使用`--oneline`选项，它将只打印关于每个提交的单行。这对于获得项目的高层次概述非常有用。

典型的`git log --oneline`输出如下所示:

```
b617e8c Commit #3
70c191e Commit #2
ff589ff Commit #1
1d47128 Commit #0
```

正如您所看到的，以前可见的一些细节现在被隐藏了，视图只包含一行代码。这更容易理解，因为现在每个提交都显示在一行中，从提交 id 开始，后跟提交消息。

所以，通常我们可以说`--oneline`标志导致`git log`显示:

*   每行一次提交
*   SHA 的前七个字符
*   提交消息

## 如何查看日志大小？

Git 中的日志大小是通知您日志大小的一个选项。在输入命令后，它输出一个带有`log-size <number>`的附加行:

```
git log --log-size
```

通过运行这个命令，您可以看到控制台中打印了一个名为 log size and number 的附加行。这个数字是各种工具所需的以字节为单位的提交消息长度。通过读取这个数字，它们可以预先分配精确的空间来保存提交消息。

## 如何查看有限数量的提交？

我们可以通过使用`git log`命令来限制输出提交的数量。如果您对更少的提交感兴趣，这个命令将消除复杂性。

要限制 git 日志的输出，请包含`-<n>`选项。如果我们只想看到最后五次提交，那么我们可以在`git log`命令中传递参数`-5`。

```
git log -5
git log -5 --oneline
```

## 如何跳过有限次数的提交？

skip 命令将消除顶部的`<n>`提交。

```
git log --skip 5
git log --skip 5 --oneline
```

## 如何查看提交范围？

特定提交历史指的是从一个提交到另一个提交所定义的历史。由于每个提交都有自己的标识符，我们可以很容易地使用它来查看历史。要查看特定的历史记录，请键入以下命令:

```
git log <since commit>..<until commit>
```

结果不包括 id 的`<since>`部分，但包括`<until>`部分。因此，`<since>`表示您希望看到的最后一次提交，而`<until>`表示您希望看到的最后一次提交。

如果您想在特定历史中包含`<since>`，您可以通过提供插入符号(`^`)来实现。

```
git log <since commit>^..<until commit>
```

## 如何按分支查看日志？

我们可以使用与目录限制类似的语法，只为一个分支构建日志。我们只需要指定想要查看的分支。

```
git log <branch>
```

默认情况下，用`git log`命令打印并列出合并提交。这些提交可能会增加日志的容量，导致分心。如果我们不想列出或打印，那么我们可以使用`--no-merges`选项，它不会显示合并提交。

```
git log --no-merges
```

如果默认行为已经用 config 改变了，我们想要列出并打印合并提交，我们也可以使用`--merges`选项来列出合并提交。

```
git log --merges
```

## 如何通过提交 ID 查看历史？

```
git log <commit>
```

您使用的提交哈希将被用作开始哈希。之前提交的所有内容将照常显示。

此外，您可能希望看到特定提交的更改。这可以通过以下方式实现:

```
git show <commit>
```

这将显示与特定提交的`git log`命令中相同的信息，并以该提交中完成的差异进行修饰。

## 如何查看提交的统计数据？

我们可能需要打印关于提交的详细信息。我们将使用`--stat`选项。

```
git log --stat <commit>
```

`--stat`标志使`git log`显示

*   修改后的文件
*   已添加或删除的行数
*   已更改记录总数的摘要行
*   添加或删除的行

## 如何查看文件的历史？

Log 命令提供了查看特定文件提交历史的选项。这个选项是`<file>`。

```
git log <file>
```

在大多数情况下，更有趣的信息是每次提交后到底发生了什么变化。运行以下命令以显示特定文件的提交，每个更改都有差异:

```
git log -p <file>
```

`git log`补丁命令显示被修改的文件。它还显示添加、删除和更新的行的位置。添加的代码颜色为绿色，删除的代码为红色。此外，添加的代码行以`+`加号开始，删除的代码行以`-`减号开始。

`--patch`标志使`git log`显示

*   已被修改的文件
*   已添加或删除的行的位置
*   已经做出的具体改变

## 如何按作者过滤提交？

在某些情况下，我们可能需要根据作者的名字过滤提交。我们将使用`--author`并提供作者的名字来过滤并只显示给定的作者。

```
git log --author="user_name"
```

该命令采用一个正则表达式，并返回与该模式匹配的作者提交的列表。您也可以使用确切的名称来代替模式。

## 如何通过评论消息过滤提交？

```
git log --grep="<pattern>"
```

您将看到包含指定搜索词的所有日志消息。这与针对作者的过滤功能完全相同。

## 如何按内容过滤提交？

我们可以根据提交内容过滤提交。如果我们想要搜索和过滤特定的变更，这将非常有用。我们将使用`-S`选项并提供过滤条件。请记住，这可能需要一些时间，因为它将搜索所有没有为快速搜索建立索引的提交。

```
git log -S"<pattern>"
```

你看，`git log -S`让你在你的回购历史中找到一个字符串的所有实例。这对于寻找被删除的代码来说太棒了。

## 如何按日期/间隔过滤提交？

我们可以按日期和时间过滤输出。我们必须通过`--after`或`--before`参数来指定日期。两个参数都接受各种日期格式。也可以用`--since`代替`--after`，用`--until`代替`--before`。

```
git log --after="12-1-2018"
```

在此示例中，日志将包含 2018 年 12 月 1 日之后创建的所有提交。`--after`和`--before`可以组合起来得到提交的范围。

我们还可以通过类似*【昨天】**【1 周前】**【21 天前】*等适用的引用语句。它将以如下方式运行:

```
git log --after="21 days ago"
```

## 如何查看与提交相关联的引用？

很多时候，知道哪个分支或标签与某个提交相关联是很有用的。`--decorate`标志使`git log`显示指向每个提交的所有引用(例如，分支、标签等)。

分支、标签、`HEAD`和提交历史几乎是 git 存储库中包含的所有信息，因此这为您提供了存储库逻辑结构的更完整视图。

## 如何按作者对提交进行分组？

如果我们想根据作者的名字检查提交，我们需要按作者对提交进行分组。我们可以使用`shortlog`命令来列出按作者姓名分组的提交注释。

```
git shortlog
```

`git shortlog`命令是`git log`的特殊版本，用于创建发布公告。它按作者对每个提交进行分组，并显示每个提交消息的第一行。

命令可以与`-c`选项一起使用，按提交者而不是作者分组。你可能想知道*作者*和*提交者*的区别。作者是最初编写作品的人，而提交者是最后应用作品的人。

## 如何显示作者提交数量？

如果我们对作者的提交数量感兴趣，我们需要向`shortlog`命令提供`-s`选项。这将为每个作者提供提交数量。

```
git shortlog -s
```

我们可以改进前面的例子，按照作者的提交数量对他们进行排序。我们将把`-n`添加到前面的例子中，最终命令如下所示。

```
git shortlog -n -s
```

## 如何查看日志的图形表示？

`--graph`标志使您能够以图表形式查看您的`git log`。为了让事情变得有趣，您可以将这个命令与您从上面学到的`--oneline`选项结合起来。

```
git log --graph
git log --graph --oneline
```

使用这个命令的好处之一是，它使您能够了解提交是如何合并的以及 git 历史是如何创建的。

还有许多其他选项可以与`--graph`结合使用。其中有几个是`--decorate`和`--all`。

```
* d46c1886 (HEAD -> feature, origin/develop, origin/HEAD, develop) Commit message
*   bb7e2976 (tag: 0.0.49, origin/master) Merged in release_0.0.49 (pull request #xxx)
|\  
| * 2839739c Release 0.0.49
| *   f8b310a9 Merged in feature_xx (pull request #xxx)
| |\  
| | *   bec90d06 Merged develop into feature_xx
| | |\  
| | |/  
| |/|   
| * |   65e31e68 Merged in feature_yy (pull request #xxx)
| |\ \  
| | * \   234d6ec7 Merged develop into feature_yy
| | |\ \
```

星号表示提交在哪个分支上，所以上图告诉我们,`bec90d06`提交在`feature_xx`分支上，其余的在不同的分支上。

虽然这对于简单的存储库来说是一个不错的选择，但是对于分支严重的项目，您可能更适合使用功能更全面的可视化工具，如`gitk`或 Sourcetree。

# 图形输出格式

如果您不喜欢默认的`git log —-graph`格式，您可以使用`git config`的别名功能创建一个快捷方式，以所需的方式显示图形/日志输出。以下是如何做到这一点的一些示例:

```
git config --global alias.lg "log  --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s  %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit  --date=relative"git config --global alias.lg1 "log --color --graph --pretty=format:’%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset’ --abbrev-commit”git config --global alias.lg2 "log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)' --all"git config --global alias.lg3 "log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold cyan)%aD%C(reset) %C(bold green)(%ar)%C(reset)%C(bold yellow)%d%C(reset)%n''          %C(white)%s%C(reset) %C(dim white)- %an%C(reset)' --all"git config -- global alias.history "log --graph --pretty=format:’%C(yellow)%h%C(cyan)%d%Creset %s %C(white)- %an, %ar%Creset’"git config --global alias.hist "log --graph --date-order --date=short
--pretty=format:'%C(auto)%h%d %C(reset)%s %C(bold blue)%ce %C(reset)%C(green)%cr (%cd)'"#quick look at all repo
git config --global alias.loggsa "log --color --date-order --graph --oneline --decorate --simplify-by-decoration --all"#quick look at active branch (or refs pointed)
git config --global alias.loggs "log --color --date-order --graph --oneline --decorate --simplify-by-decoration"#extend look at all repo
git config --global alias.logga "log --color --date-order --graph --oneline --decorate --all"#extend look at active branch
git config --global alias.logg "log --color --date-order --graph --oneline --decorate"#Look with date
git config --global alias.logda "log --color --date-order --date=local --graph --format=\"%C(auto)%h%Creset %C(blue bold)%ad%Creset %C(auto)%d%Creset %s\" --all"git config --global alias.logd "log --color --date-order --date=local --graph --format=\"%C(auto)%h%Creset %C(blue bold)%ad%Creset %C(auto)%d%Creset %s\"#Look with relative date
git config --global alias.logdra "log --color --date-order --graph --format=\"%C(auto)%h%Creset %C(blue bold)%ar%Creset %C(auto)%d%Creset %s\" --all"git config --global alias.logdr "log --color --date-order --graph --format=\"%C(auto)%h%Creset %C(blue bold)%ar%Creset %C(auto)%d%Creset %s\""git config --global alias.loga "log --graph --color --decorate --all"
```

您也可以按照自己喜欢的方式配置`git log`命令。

# 摘要

`git log`是一个强大的实用工具，用于回顾和读取存储库中发生的所有事情的历史。这个命令帮助您查看过去的提交，这有助于了解谁在存储库中做了什么。这是一个很小的工具，可以使研究历史变得容易得多。