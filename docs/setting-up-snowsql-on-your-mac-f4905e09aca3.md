# 在 Mac 上设置 SnowSQL

> 原文：<https://blog.devgenius.io/setting-up-snowsql-on-your-mac-f4905e09aca3?source=collection_archive---------6----------------------->

## 在 MacOS 上安装和运行 SnowSQL 的快速指南

![](img/c4679e6e1f3687d63c3f908cb18eaac0.png)

照片由[亚伦·伯顿](https://unsplash.com/@aaronburden?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

我最近开始学习雪花的“实践基础”课程，并在此过程中获得了一些徽章。我发现这些课程真的很有价值，而且我认为它们组合得非常好。论坛对于故障诊断也非常有用，通常几乎对遇到的每个问题都有解决方案。

然而，在数据应用课程中，有一个部分确实让我慢了下来，我不得不在讨论帖子之外搜索完整的答案。幸运的是，答案并不难找到，尽管它们并不存在于一个单一的地方。所以，我想我应该整理一下我的发现，给那些可能和我有同样问题的人。

在我们继续之前，有一点需要注意，基础课程的实践要求学习者在 AWS 上创建一个试用的雪花账户，所以代码片段倾向于反映这一点。如果您有个人雪花帐户，您的 AWS 区域可能与我的不同，或者您的帐户可能完全链接到另一个云提供商。我不能肯定地说，这里概述的过程将链接其他提供商托管的雪花账户，尽管我怀疑你只需要在相关的账户设置中进行替换。

## 安装 SnowSQL

安装应用程序非常简单。你可以点击[这里](https://developers.snowflake.com/snowsql/)来下载你的操作系统的最新版本。然而，我在这里只讨论 MacOS 的安装，所以我希望你选择“SnowSQL for Mac”。下载完成后，运行安装文件。

## 测试安装

这里我假设您熟悉命令行界面(CLI ),比如终端，因为我现在需要您打开它。

安装成功完成后，会邀请您测试一下是否一切正常。为此，您需要您的帐户详细信息、用户名和密码。如果您不记得自己的帐户名称，最简单的方法是在雪花中打开一个工作表并运行

```
SELECT current_account();
```

获得所有详细信息后，在终端中输入以下命令:

```
% snowsql -a youraccountname -u yourusername
```

现在，如果一切都是 ticketyboo，你将被要求输入密码，一旦你做到了这一点，你就会笑着离开。

对我来说，这并没有发生；相反，我收到了

`zsh: command not found: snowsql`

解决这个问题相当简单。这里发生的情况是，应用程序的路径不在路径列表中，所以我们的系统不知道在哪里找到它。所以，让我们更新路径信息。

## 更新路径文件

首先，导航到根目录，然后跳转到`etc`文件夹。为此，您可以运行下面的命令`% cd /etc`。检查你是否在你应该运行的地方`pwd`。现在，我们需要更改的文件叫做`paths`，但是在我们全部进入之前，检查文件中的内容可能是个好主意。使用这个命令将内容打印到控制台:`% cat paths.`希望您会看到如下内容:

```
/usr/local/bin
/usr/bin
/bin
/usr/sbin
/sbin
```

是的——没有通往 SnowSQL 的道路。没有戏剧，我们只需要添加它。

接下来，我们将使用 Nano 打开`paths`文件，并将`/Applications/SnowSQL.app/Contents/MacOS`添加到路径列表中。对我来说，这是通往 SnowSQL 的道路，但对你来说可能略有不同。一旦有了它，运行以下命令:

```
% sudo nano paths
```

你会被要求输入密码，所以输入密码。完成后，您应该能够看到我们之前打印到控制台的路径列表。您现在需要做的就是使用箭头键导航到底部并添加您的路径字符串。

要保存文件，按`^X`，然后按`Y`，再按`ENTER`。如果您随后再次使用`cat`功能将`paths`打印到控制台，您现在应该会看到如下内容:

```
/usr/local/bin
/usr/bin
/bin
/usr/sbin
/sbin
/Applications/SnowSQL.app/Contents/MacOS/
```

我只想提一下，这部分的大部分内容摘自我在这里找到的一篇文章。

## 更新配置文件

到目前为止，一切顺利——我是这么想的。完成这一步后，我再次尝试连接，但失败了。在这种情况下，我需要在我的帐户名称中包含地区和云提供商。根据您所在的地区或帐户类型，这对您来说可能不是问题，但对我来说，这是必要的一步。这意味着登录时，我的凭据看起来像这样(您所在的地区可能会有所不同):

```
% snowsql -a myaccountname.ca-central-1.aws -u myusername
```

一旦我这样做了，我被要求输入密码(耶！).

然而，在我收到两条拒绝许可的消息后，我的喜悦是短暂的。错误涉及两个日志文件:`Users/snowsql_rt.log`和`Users/snowsql_rt.log_bootstrap`。幸运的是，数据应用讨论帖子对此有一个解决方案。在这里，我将再次使用 Nano 向您展示如何实现这一修复。

我们再次需要对一个文件进行一些更改，尽管这次是安装附带的`config`文件。要找到它，请导航到您的主目录中的 SnowSQL 文件夹:`% cd ~/.snowsql`。在那里，列出目录内容，希望您会看到如下内容:

```
1.2.24  autoupgrade config  downloadlck history
```

如果你看不到`config`文件，你很可能在错误的地方——要么是这个，要么是某个地方出了可怕的问题。

就像我们对`paths`文件所做的那样，我们将再次使用 Nano 打开并编辑`config`文件，如下所示:

```
% sudo nano config
```

现在，您需要向下滚动一点，找到分配日志路径的位置。一旦你找到它们，你想要应用以下设置(至少，这是我用的):

```
log_file = ~/.snowsql/snowsql_rt.log
log_bootstrap_file = ~/.snowsql/log_bootstrap
```

完成后，按`^X`保存，然后按`Y`，再按`ENTER`。

## 连接到雪花

一旦我做了这些改变，一切都变得非常好！提醒一下，这是我以前登录时用的:

```
% snowsql -a myaccountname.ca-central-1.aws -u myusername
```

很可能有另一个更简单的解决方案，我还没有遇到过，但这个对我很有效。希望这对你也有用🙂

如果你喜欢这篇文章，并且想保持更新，那么请考虑在 Medium 上关注我。这将确保你不会错过新的内容。如果你更喜欢的话，你也可以在 LinkedIn 和 Twitter 上关注我😉