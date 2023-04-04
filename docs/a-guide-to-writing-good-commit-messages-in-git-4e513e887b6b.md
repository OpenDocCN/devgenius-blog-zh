# 在 Git 中编写良好提交消息的指南

> 原文：<https://blog.devgenius.io/a-guide-to-writing-good-commit-messages-in-git-4e513e887b6b?source=collection_archive---------5----------------------->

![](img/d821e99931d63c26ab5a79b6ab307cda.png)

来自 Pexels 的 Tim Gouw

你在你的磁盘上有一个名为“版本 1”的存档文件的时间应该是遥远的。我打赌你会使用一些像 GitHub 这样的版本控制工具。因此，每当您想要将代码添加到项目中时，都必须编写提交。您是否曾经想过您的提交消息是否是好的？也许你不喜欢写它们，想知道为什么每次想推代码的时候都要这么做。毕竟变化还不够充分吗？

本文将帮助您创建一个好的提交消息。它会给你(对我来说)一个很好的模式，让你第一眼就能看到这个提交消息中发生了什么。

# 为什么提交消息很重要？

那么，改变足够了吗？在我看来(我也希望其他人如此)，答案是否定的！

但是为什么呢？

提交消息有很多用途。这是在项目中与其他开发人员交流的第一种方式。变更定义了你如何实现某事，但是提交消息解释了你为什么要这么做。它必须给出足够的上下文，以避免贡献者想知道为什么一些代码是这样写的。有了适当的解释，你就不会花宝贵的时间问自己为什么一个开发者会推出这段代码。在某些情况下，比如开源项目，如果你不给他们最起码的信息，比如一条精心编写的提交消息，忙碌的维护人员甚至不会阅读你的代码。

彼得·赫特勒写道:

> *任何软件项目都是一个协作项目。它至少有两个开发者，最初的开发者和几周或几个月后思路早已离开车站时的最初开发者。* [*【彼得·赫特勒(打开新窗口)*](http://who-t.blogspot.com/2009/12/on-commit-messages.html)

总而言之，他会告诉我们:

“承诺信息不仅是给你的同事的，也是给你未来的彼得·赫特勒的”

一个项目是为了长期维护而设计的。有时候，在一段代码被写出来几年后，你可能会问自己为什么它会被写成那样。这个星期，我在 6 个月前编写的一个函数中遇到了这种情况。有了一个写得很好的提交消息，就更容易回到您写这个变更时的思想状态。

关注你的提交信息也可以简化你的日常生活。一些工具因为有了有意义的提交历史而变得更加有趣，比如一些 git 命令 **log** ， **rebase** ， **cherry-pick** ，…当你度假回来时，一个简单的**git log——漂亮的**会给你很多关于你的同事在你离开时做了什么的信息。编写好的提交消息迫使你分割你的变更，使得用 **git log -p** 检查变得更容易。

说到审查，你有没有因为开发人员做的一些改变而挣扎着改变你的代码？如果提交消息具有真正的意义，那么知道为什么要进行更改就变得更容易了。这使得重定基数的结果更容易预测。另一个例子是 git 责备命令，顺便说一下，它不是用来取笑你的同事，而是给你特定代码行的上下文。当你想重构一段代码时，它会非常有用。

最后但同样重要的是，一个结构良好的提交历史允许在项目的两个版本之间生成变更日志，以通知用户发生了什么变化。

这是开始花时间编写好的提交消息的大量理由。

# 如何编写一个好的提交消息

既然你已经确信提交消息是重要的，那么让我们来谈谈如何写出好的消息。

例如，这些提交消息不一致，这使得它们难以阅读。

```
fixex bug #72 
did some perfomance stuff
Remove built files
Add tests #81
hope it work
forgot files
```

> *风格。标记语法、换行、语法、大小写、标点符号。把这些东西拼出来，去掉猜测，尽可能的简单。最终的结果将是一个非常一致的日志，不仅读起来很愉快，而且确实会定期被阅读。内容。提交消息(如果有)的正文应该包含什么样的信息？它不应该包含什么？元数据。应该如何发布跟踪 id、拉动式请求编号等。被引用？* [*Chris Beams —如何编写 Git 提交消息(打开新窗口)*](https://chris.beams.io/posts/git-commit/)

在上面的例子中，我们看到一些提交是混乱的，因为这个意思分散在不止一个提交中。为了防止这种情况，Chris 说必须存在一个满足 7 条规则的提交模式来编写一个好的提交消息

1.  用一个空行将主题和正文分开
2.  将主题行限制在 50 个字符以内
3.  大写主题行
4.  不要以句号结束主题行
5.  在主题行中使用祈使语气
6.  在 72 个字符处换行
7.  用身体来解释什么和为什么以及如何

我个人应用这些规则的子集:

1.  将主题行限制在 48 个字符以内
2.  大写主题行
3.  在主题行中使用祈使语气

不仅角色限制迫使你简洁，这是一个伟大的技能，而且它与很多工具非常契合。

我倾向于认为完美是好的敌人。这就是为什么我没有讨论提交消息体。我认为我们在总结方面有足够的工作要做。但我也想补充一条规则:

1.  添加票证或任务 ID

大多数时候，我们在代码审查过程中使用工具进行讨论，如 GitHub 或 Azure DevOps。关于变更的所有信息至少会在那里描述。我添加了这条关于跟踪 ID 的规则，这样就很容易找到相关的拉/合并请求。编写提交主体比添加跟踪 id 更好，因为如果您更改您的 repository manager 解决方案，您将丢失所有这些宝贵的上下文信息，并且您的跟踪 id 将毫无用处。

下面是一个应用了这些规则的提交历史示例

```
Make core independent from the git client (#171)
Upgrade Docker image version (#167)
Add maven preset (#172)
Add a generic preset using configuration file (#160)
Improve error messages for preset system (#161)
Publish Canary version on master push (#141)
```

有一个一致的风格使它更容易阅读。您可以弄清楚变更的上下文，如果可能的话，您还可以看到应用的票证。最重要的是在您的团队中找到共同的规则，以确保有一个结构良好的提交历史。对于这一点，有一些惯例可以帮助你找到这些，但我们稍后将回到这个主题。

如果你很难找到你应该写什么信息，也许你应该把你的承诺分成更小的部分。Peter Hutterer 在他的文章中列举了几个例子，在这些例子中，很难找到一个好的提交消息，因为您的提交补丁可能不符合逻辑。

# 提交约定

如前所述，您应该定义一个提交约定(可能是为您的团队或者您的整个开发部门)。存在大量开源提交约定，它们可能是很好的灵感来源。它们还附带了一个完整的生态系统，包括帮助您编写提交消息、生成 changelog、创建发布等等的工具。很多事情可能会花费大量的时间。有很好的文档记录，所以您不必编写关于您自己的提交消息约定的文档。

我们将讨论其中的两个[常规提交(打开新窗口)](https://www.conventionalcommits.org/en/v1.0.0/)和 [gitmoji(打开新窗口)](https://gitmoji.carloscuesta.me/)。

# 常规提交

这是一个受角度提交消息指南启发的规范。它有一些有趣的规则，比如:

*   提交必须以类型(feat，fix)为前缀
*   可能会提供一个范围来指出代码库的特定部分(对于 mono repo 来说确实很有趣)
*   重大变更必须包含在页脚部分

以下是必须使用此规范构造提交消息的方式:

```
[optional scope]: <description>[optional body][optional footer(s)]
```

要了解更多信息，您可以查看他们的[规格(打开新窗口)](https://www.conventionalcommits.org/en/v1.0.0/#specification)

# 吉特莫吉

我喜欢表情符号，每个人都喜欢它，❤，gitmoji 致力于使用表情符号对提交进行分类。我认为可视化提交原因非常合适，所以我认为这个约定对每个人都有好处。

我在之前的 gitmoji-changelog 提交示例中没有完全诚实。现在你应该已经猜到了，有一个缺失的部分。

```
:recycle: Make core independent from the git client (#171)
:whale: Upgrade Docker image version (#167)
:sparkles: Add maven preset (#172)
:sparkles: Add a generic preset using configuration file (#160)
:recycle: Improve error messages for preset system (#161)
:construction_worker: Publish Canary version on master push (#141)
```

这些文本别名在 Slack、Discord 等工具中被广泛使用……大多数存储库管理器工具，如 GitHub 或 GitLab、AzureDevops 解释它们并在它们的 UI 中很好地显示它们:

```
♻️ Make core independent from the git client (#171)
🐳 Upgrade Docker image version (#167)
✨ Add maven preset (#172)
✨ Add a generic preset using configuration file (#160)
♻️ Improve error messages for preset system (#161)
👷‍♂️ Publish Canary version on master push (#141)
```

我喜欢这个约定，因为我一眼就知道提交了什么样的更改。它附带了一个 cli 叫做 [gitmoji-cli(打开新窗口)](https://github.com/carloscuesta/gitmoji-cli)帮助你写提交消息，我为它做了一个 changelog 生成器。

他们还有更多。请记住，这些是指导方针，您可以将它们混合在一起，以满足您的需求。

```
**Want to Connect?**Say Hello on: [LinkedIn](https://www.linkedin.com/in/sascha-peter-bajonczak-32a17a2a/), [GitHub](https://github.com/sbajonczak), and [Blog](https://blog.bajonczak.com/).
```