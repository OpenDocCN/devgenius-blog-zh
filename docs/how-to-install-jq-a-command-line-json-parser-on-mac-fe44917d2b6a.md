# 如何在 Mac 上安装命令行 JSON 解析器 jq

> 原文：<https://blog.devgenius.io/how-to-install-jq-a-command-line-json-parser-on-mac-fe44917d2b6a?source=collection_archive---------4----------------------->

![](img/22a38b3943e8979197fdabba56db672f.png)

[x](https://unsplash.com/@xaze) 在 [Unsplash](https://unsplash.com/photos/xtgONQzGgOE) 上的原始照片；jq 的 logoTremaine Eto 的插图。

如果你安装了自制软件，在 Mac 上安装`jq`会非常容易。

但是首先，不确定`jq`是什么，为什么它有用？jq 的网站描述得比我更好:

> jq 就像是 JSON 数据的`sed`——你可以用它来分割、过滤、映射和转换结构化数据，就像`sed`、`awk`、`grep`和朋友们让你玩文本一样容易。
> 
> jq 是用可移植的 C 语言编写的，它没有运行时依赖性。你可以下载一个单独的二进制文件，并期望它能够工作。
> 
> jq 可以轻而易举地将您拥有的数据格式转换成您想要的格式，并且这样做的程序通常比您预期的更短更简单。

不确定你是否安装了自制软件？在终端中运行以下命令进行检查:

```
$ brew
```

如果您安装了它，您将看到如下输出:

```
$ brew
Example usage:
  brew search TEXT|/REGEX/
  brew info [FORMULA|CASK...]
  brew install FORMULA|CASK...
  brew update
  brew upgrade [FORMULA|CASK...]
  brew uninstall FORMULA|CASK...
  brew list [FORMULA|CASK...]Troubleshooting:
  brew config
  brew doctor
  brew install --verbose --debug FORMULA|CASKContributing:
  brew create URL [--no-fetch]
  brew edit [FORMULA|CASK...]Further help:
  brew commands
  brew help [COMMAND]
  man brew
  [https://docs.brew.sh](https://docs.brew.sh)
```

如果没有，您将会看到类似这样的内容:

```
zsh: command not found: brew
```

如果你没有，那么你会想下载自制软件。幸运的是，我有一篇关于这个的文章:

[](/how-to-install-homebrew-brew-on-mac-d438c8788759) [## 如何在 Mac 上安装家酿(brew)

### 如何安装一个非常流行的包下载管理器？

blog.devgenius.io](/how-to-install-homebrew-brew-on-mac-d438c8788759) 

一旦你确认你已经安装了自制软件，那么你所要做的就是运行下面的命令来安装`jq`:

```
$ brew install jq
```

一旦输出告诉您安装成功，您可以通过运行`jq`来确认。您将看到如下输出:

```
$ jq
jq - commandline JSON processor [version 1.6]Usage:  jq [options] <jq filter> [file...]
        jq [options] --args <jq filter> [strings...]
        jq [options] --jsonargs <jq filter> [JSON_TEXTS...]jq is a tool for processing JSON inputs, applying the given filter to
its JSON text inputs and producing the filter's results as JSON on
standard output.The simplest filter is ., which copies jq's input to its output
unmodified (except for formatting, but note that IEEE754 is used
for number representation internally, with all that that implies).For more advanced filters see the jq(1) manpage ("man jq")
and/or [https://stedolan.github.io/jq](https://stedolan.github.io/jq)Example:$ echo '{"foo": 0}' | jq .
        {
                "foo": 0
        }For a listing of options, use jq --help.
```

说完，`jq`就装了！

如果你觉得这篇文章很有帮助或者只是喜欢阅读它，可以考虑[注册成为一名媒体会员](https://tremaineeto.medium.com/membership)。每月 5 美元，你可以无限制地阅读媒体上关于软件、技术等主题的报道。如果你用我的链接注册，我会得到一小笔佣金。

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入 Medium—Tremaine Eto

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)