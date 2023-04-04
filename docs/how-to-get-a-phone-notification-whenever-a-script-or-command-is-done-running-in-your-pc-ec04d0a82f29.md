# 如何在您的电脑上运行完脚本或命令时获得电话通知

> 原文：<https://blog.devgenius.io/how-to-get-a-phone-notification-whenever-a-script-or-command-is-done-running-in-your-pc-ec04d0a82f29?source=collection_archive---------1----------------------->

## 我们可以告诉我们的系统在任何任务完成后直接发送通知到我们的手机。免费使用我们都已经安装的应用程序。

![](img/e9bc7f417712387a744e6e182451932a.png)

照片由 [Jonas Leupe](https://unsplash.com/@jonasleupe?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

我们可以使用脚本做任何事情，这些是很棒的问题解决者。但是在生活中，没有什么是完美的。无论何时运行脚本，都必须等待给定任务完成。如果命令简短，您就不必等待太久就能得到结果。另一方面，如果这些脚本需要很长时间，那就是一个问题。大多数时候，如果你也在忙着做某件事，你就会错过一个任务完成的时间，让你浪费宝贵的时间去等待已经做了很久的事情。

# 介绍

我想告诉你如何在你的手机或电脑屏幕上获得通知，让你知道你的命令刚刚完成。如果您需要运行更多的脚本，这是一种节省时间的便捷方式。没有人愿意编译一个程序，然后只看着终端就等着它完成。

# 要求

*   电报帐户
*   Python 3
*   饭桶
*   Ntfy

# 安装要求

# 计算机编程语言

先从安装 **python 3** 开始。Windows 和 Mac OS 用户可以去[python.org](https://www.python.org/)按照那里的指示操作。对于 Linux，只需运行:

```
apt-get install python 3
```

# Git 窗口

在 Windows 上安装 Git 有几种方法。最官方的版本可以在 Git 网站上下载。只需进入[https://git-scm.com/download/win](https://git-scm.com/download/win)，下载就会自动开始。注意，这是一个名为 Git for Windows 的项目，与 Git 本身是分开的；欲了解更多信息，请访问[https://gitforwindows.org](https://gitforwindows.org)。

# Git Mac OS

在 Mac 上安装 Git。最简单的可能是安装 **Xcode 命令行工具**。在 Mavericks (10.9)或更高版本上，你可以通过第一次尝试从终端运行`git`来实现。

```
$ git --version
```

如果您还没有安装它，它会提示您安装它。

# Git Linux

```
sudo apt install git-all
```

# Ntfy

在 windows 中安装 **ntfy** 的步骤与在 Linux 中不同。在这里我将解释如何在 windows 中安装它。如果您是 Linux 用户，请跳过这些步骤。

因为我们已经安装了 Git，所以转到[https://github.com/dschep/ntfy](https://github.com/dschep/ntfy)，打开 Power shell 并运行以下命令来克隆存储库。

```
$ git clone [https://github.com/dschep/ntfy.git](https://github.com/dschep/ntfy.git)
$ cd ntfy
$ python setup.py
```

# Ntfy Linux 和 Mac OS

要安装 ntfy run:

```
$ pip install ntfy
$ pip install ntfy[telegram]# Enable shell integration
$ echo 'eval "$(ntfy shell-integration)"' >> ~/.bashrc
```

确保所有东西都以正确的方式安装运行

```
ntfy send test
```

# 相关文章

[](/make-your-pc-notify-your-phone-whenever-there-is-movement-around-it-c6c4ccd734b7) [## 让您的电脑通知您的手机，无论何时周围有动静

### 不用花钱，只需使用你的电脑就能让你的家更安全

blog.devgenius.io](/make-your-pc-notify-your-phone-whenever-there-is-movement-around-it-c6c4ccd734b7) [](https://medium.com/geekculture/building-a-drowsiness-detector-using-machine-learning-9f64f4052e3d) [## 使用机器学习构建睡意检测器

### 这种困倦检测方法快速、高效且易于实现。

medium.com](https://medium.com/geekculture/building-a-drowsiness-detector-using-machine-learning-9f64f4052e3d) 

# 创建一个新的电报机器人

在这里，您将需要制作一个电报机器人，它将充当通知。要创建一个新的电报机器人，发送命令 **/newbot** 。你必须给它一个名字和用户名。完成后，您将获得一个访问令牌。复制电报机器人的访问令牌。我们以后会需要它。

# 将您的电脑连接到 Telegram

打开终端并运行

```
ntfy -b telegram send “hello world”
```

它会要求您提供一个令牌，粘贴我们从 Telegram 获得的以前的访问令牌，就这样。为了测试你的通知，再次发送相同的命令，你将从你的机器人那里得到一个 **hello world** 的电报。

# 电报自动发送

在测试完所有东西，并且让**电报机器人**按预期工作之后，我们可以设置**终端**在执行每个命令时向电报发送一个通知。通知将包括:

*   **执行命令的名称**
*   **运行和结束时间**
*   **命令的结果**

为了完成这项工作，请打开您的**。bashrc** 文件，并在最后添加以下代码行:

```
source ~/.local/share/ntfy/auto-ntfy-done.shsource ~/.local/share/ntfy/bash-preexec.sh
```

保存它，关闭文件，并导航到 **/root/。local/share/ntfy/auto-ntfy-done . sh .**打开 **auto-ntfy-done.sh** ，在第 7 行修改以下内容

```
AUTO_NTFY_DONE_OPTS='-b default'
```

到

```
AUTO_NTFY_DONE_OPTS='-b telegram'
```

现在每次在终端中执行一个命令，你都会收到一个通知发送到电报。这对你的口味来说太多了？您可以将脚本设置为仅在某些命令运行时向您发送通知，例如运行时间超过 10 秒的命令。就是这样。我们完了。

# 结论

如果你是我的老追随者，你已经知道我热爱自动化。我创建的许多机器人都可以用这种神奇的技术进行监控。它会告诉你你的机器人是否因为任何原因崩溃。

该设置可用于将长文本或脚本发送到您的手机，作为从 **PC** 到**手机**的**复制/粘贴**。我个人经常使用这个功能，这也是我想出这种设置的真正原因。我总是发现自己试图向手机发送链接和文本。现在我可以没有任何问题地做它。

尽情享受吧！