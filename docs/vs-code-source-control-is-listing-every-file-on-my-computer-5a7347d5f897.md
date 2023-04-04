# VS 源代码控制列出了我电脑上的每个文件

> 原文：<https://blog.devgenius.io/vs-code-source-control-is-listing-every-file-on-my-computer-5a7347d5f897?source=collection_archive---------0----------------------->

# 两个可能的原因

如果这条消息也出现在你的 VS 代码中，我会支持你的。当我开始在 flutter 中编写我的第一个应用程序时，我收到了这个警告。我决定使用 VS 代码，因为 youtube 上几乎所有的教程都是用 VS 代码完成的。在用 dart 和 flutter 编程时，我被许多新术语弄得不知所措，我想我希望能够坚持使用最常见的 IDE，因为庞大的用户群意味着许多教程，这有助于将挫折感降至最低。

但是由于没有使用 VS 代码的经验，我不知道该如何解释这样的警告，也无法想象哪里出错了。我忽略了这个问题，直到我需要使用分支，因为我想开发新的功能，可以打破其余的代码。

![](img/0e9b83df224bae3298160827eb12c951.png)

照片由[达莉亚·克拉普莱克](https://unsplash.com/@daria_kraplak?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 首要原因—停产问题

我首先在谷歌上搜索了这条警告信息。就像你做的那样。我找到了一个 StackOverflow 问题，正好问了这个问题。[https://stack overflow . com/questions/60160453/vs-code-the-git-repository-has-too-many-changes-only-a-subset-of-git-f](https://stackoverflow.com/questions/60160453/vs-code-the-git-repository-has-too-many-active-changes-only-a-subset-of-git-f)

也许你在看到这篇文章之前就在 SO 上访问过这个问题。我读了这个问题的答案，它让我比以前更加困惑。

最受欢迎的回答是关于下线问题(EOL)。您可以通过运行`ls-files --eol`来检查是否是停产问题。如果是，请执行以下步骤:

1.  键入 git config-global core . auto crlf false
2.  重新克隆您的存储库
3.  检查 VSCode 上的问题是否仍然存在

这是跨平台文件的一个问题，正如在 flutter 中看到的。基本上，Unix 和 Mac OSX(OSX 之前使用 CR)客户端使用 LF 行尾，而 Windows 客户端使用 CRLF 行尾。在某些情况下，这些结尾被改变(转换)导致 Git 看到改变的文件(但实际上没有改变)。这篇文章没有更详细地看它(我用第二个原因解决了我的问题)，如果你感兴趣，我在文章的底部列出了一些讨论和资源。[1][2][3]

# 第二个原因:VS 源代码控制列出了我电脑上的所有文件

我决定寻找其他原因，因为我没有足够的信心使用这三个步骤。我再次转到 VS 代码，在左边的面板上我看到了源代码控制。我瞥了一眼修改过的文件。这对我来说毫无意义。我检测到了 Python 文件和 anaconda 文件，甚至一些中学的文件。这很奇怪。然而，我意识到有了这些信息，我可以尝试用谷歌搜索我的问题。我搜索了“VS 源码控制正在列出我电脑上的每一个文件”。我找到了一篇帮助我度过难关的文章。我想让这些信息便于您操作！

*   在您的终端中进入您遇到问题的项目文件夹。
*   写`git rev-parse --show-toplevel`

此命令搜索。git 文件夹，它位于文件目录树的较高位置，它将返回第一个遇到的 git 存储库的位置。这意味着你要么会收到一个`fatal`未找到或一个`path`。如果你得到了一条道路，你就是一个快乐的人，因为本教程的其余部分将帮助你解决你最初遇到的问题。

*   `fatal: not a git repository (or any of the parent directories): .git`(这意味着本教程不会对你有任何帮助)
*   `/Users/[username]/../[arbitrary]`(或任何其他出现的路径)

这个路径`/Users/[username]/../[arbitrary]`是一个文件夹的位置(为了简单起见，我们称它为 A ),它有一个. git 文件夹。在这个文件夹 A 中，可能还有其他文件夹，这些文件夹有子文件夹，这些文件夹也有子文件夹，以此类推。您正在处理的项目，也是您收到这个愚蠢警告的原因，就在其中一个子文件夹中。VS 代码搜索任何。git 文件夹，并列出属于。

## 为了解决这个问题，我们需要:

1.  通过使用`cd ..`转到我们在`/Users/[username]/../[arbitrary]`之前收到的这个地址，直到我们到达那里
2.  然后，我们应该通过键入`git log --stat`来检查对这个存储库做了什么提交，并通过使用`git remote -v`来检查它是否连接到任何远程存储库
3.  删除`yes | rm -r .git`中的类型是否安全
4.  然后我们去我们的项目文件夹`cd [somepath]/../..`
5.  写上`git rev-parse --show-toplevel`这次我们要接收`fatal: not a git repository (or any of the parent directories): .git`
6.  如果是这样，我们想使用命令`git init`创建一个. git 文件夹
7.  暂存所有文件`git add -A`
8.  提交文件`git commit -m "finally can commit"`

**大功告成。恭喜你！！**

![](img/956aa62737bd735643bde29d251a9c6b.png)

照片由[莎伦·麦卡琴](https://unsplash.com/@sharonmccutcheon?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 为什么我会有这个问题？

老实说，我认为我的问题的原因是我在编程之初缺乏经验。我假设我不小心把`git init`写在了一个文件夹中，不知道如何删除它，也不知道如何在创建后搜索它，我把它放在了那里。回想起来，我本应该更努力去清除这场事故。两年后，这个`.git`文件夹扰乱了我的工作流程，因为我使用了 VS 代码，我终于意识到我有了这个不必要的和潜在的有害组织。令人高兴的是，我为自己解决了这个问题，我希望你也能解决你的错误。

## 有用的资源

**ML 项目的 Git 基础知识**

[](https://towardsdatascience.com/learn-git-basics-for-your-ml-project-46ecf03865b) [## 学习 ML 项目的 Git 基础知识

### 开始使用 Git，并通过使用模拟真实软件和机器学习工程师的工作方式…

towardsdatascience.com](https://towardsdatascience.com/learn-git-basics-for-your-ml-project-46ecf03865b) 

**ML 项目的 Git 分支**

[](https://towardsdatascience.com/learn-git-branches-with-your-ml-project-7f58bdf1ae80) [## 用您的 ML 项目学习 Git 分支

### MLE 项目的版本控制至关重要。这是它的工作原理。

towardsdatascience.com](https://towardsdatascience.com/learn-git-branches-with-your-ml-project-7f58bdf1ae80) 

## 这篇文章的灵感

[](https://dev.to/eliastooloee/help-vs-code-source-control-is-listing-every-file-on-my-computer-how-can-i-just-commit-the-changes-from-my-current-project-39m5) [## 救命啊！VS 源码控制列出了我电脑上的每个文件！我怎么能只是提交…

### 像许多开发人员一样，我有一个坏习惯，就是沉迷于代码，忘记将我的更改提交给 Github…

开发到](https://dev.to/eliastooloee/help-vs-code-source-control-is-listing-every-file-on-my-computer-how-can-i-just-commit-the-changes-from-my-current-project-39m5) 

[1][https://rehansaeed.com/gitattributes-best-practices/](https://rehansaeed.com/gitattributes-best-practices/)

[2][https://stack overflow . com/questions/3206843/how-line-ending-conversions-work-with-git-core-auto crlf-between-different-operat/14039909 # 14039909](https://stackoverflow.com/questions/3206843/how-line-ending-conversions-work-with-git-core-autocrlf-between-different-operat/14039909#14039909)

[3][https://stack overflow . com/questions/5787937/git-status-shows-files-as-changed-even-but-contents-is-the-same/35204436 # 35204436](https://stackoverflow.com/questions/5787937/git-status-shows-files-as-changed-even-though-contents-are-the-same/35204436#35204436)