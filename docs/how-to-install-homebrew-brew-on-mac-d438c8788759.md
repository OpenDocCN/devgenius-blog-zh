# 如何在 Mac 上安装家酿(brew)

> 原文：<https://blog.devgenius.io/how-to-install-homebrew-brew-on-mac-d438c8788759?source=collection_archive---------1----------------------->

![](img/dad1e7772f43083839e42da2352c2f9e.png)

简而言之，Homebrew 是一个非常有用的工具，它允许你安装你的 Mac(或 Linux)系统没有的软件包，但是你需要它来满足你的各种项目需求。

它将这些包安装到它们自己的目录中，然后将它们的文件符号链接到`/usr/local`。

例如，如果您想安装命令行系统监视器工具 htop(一个非常有用的工具)，您所要做的就是运行:

```
$ brew install htop 
```

事不宜迟，让我们进入安装步骤。

# 如何安装自制软件

1.  转到[https://brew.sh/](https://brew.sh/)
2.  在主页上找到终端命令，并将其复制到您的剪贴板上(为了您的方便，我在 8/3/22 之前复制了它，但它可能会在将来更新)

```
/bin/bash -c "$(curl -fsSL [https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh](https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh))"
```

3.您将看到以下内容，请按键盘上的`RETURN`或`ENTER`按钮继续:

```
==> Checking for `sudo` access (which may request your password)...
Password:
==> This script will install:
/opt/homebrew/bin/brew
/opt/homebrew/share/doc/homebrew
/opt/homebrew/share/man/man1/brew.1
/opt/homebrew/share/zsh/site-functions/_brew
/opt/homebrew/etc/bash_completion.d/brew
/opt/homebrew
==> The following new directories will be created:
/opt/homebrew/bin
/opt/homebrew/etc
/opt/homebrew/include
/opt/homebrew/lib
/opt/homebrew/sbin
/opt/homebrew/share
/opt/homebrew/var
/opt/homebrew/opt
/opt/homebrew/share/zsh
/opt/homebrew/share/zsh/site-functions
/opt/homebrew/var/homebrew
/opt/homebrew/var/homebrew/linked
/opt/homebrew/Cellar
/opt/homebrew/Caskroom
/opt/homebrew/FrameworksPress RETURN/ENTER to continue or any other key to abort:
```

4.根据您的网速，完成安装需要一些时间，但您最终有望看到以下信息，表明您的安装已成功完成:

```
Warning: /opt/homebrew/bin is not in your PATH.
  Instructions on how to configure your shell for Homebrew
  can be found in the 'Next steps' section below.
==> Installation successful!==> Homebrew has enabled anonymous aggregate formulae and cask analytics.
Read the analytics documentation (and how to opt-out) here:
  [https://docs.brew.sh/Analytics](https://docs.brew.sh/Analytics)
No analytics data has been sent yet (nor will any be during this install run).==> Homebrew is run entirely by unpaid volunteers. Please consider donating:
  [https://github.com/Homebrew/brew#donations](https://github.com/Homebrew/brew#donations)==> Next steps:
- Run these two commands in your terminal to add Homebrew to your PATH:
    echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> /Users/yourname/.zprofile
    eval "$(/opt/homebrew/bin/brew shellenv)"
- Run brew help to get started
- Further documentation:
    [https://docs.brew.sh](https://docs.brew.sh)
```

现在，您应该按照“后续步骤”将 Homebrew 添加到您的路径中，这样您就可以在终端的任何地方运行`brew`。如果你不这样做，每当你试图运行`brew`时，你就会看到这个:

```
$ brew install something
zsh: command not found: brew
```

5.运行这个:

```
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> /Users/<willshowyouruserdirectory>/.zprofile
```

6.运行这个:

```
eval "$(/opt/homebrew/bin/brew shellenv)"
```

**大功告成！现在你可以运行`brew install <yourPackage>`了，它会工作的！**

快乐酿造！

如果你觉得这篇文章很有帮助或者只是喜欢阅读它，可以考虑[注册成为一名媒体会员](https://tremaineeto.medium.com/membership)。每月 5 美元，你可以无限制地阅读媒体上关于软件、技术等主题的报道。如果你用我的链接注册，我会得到一小笔佣金。

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入 Medium—Tremaine Eto

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)