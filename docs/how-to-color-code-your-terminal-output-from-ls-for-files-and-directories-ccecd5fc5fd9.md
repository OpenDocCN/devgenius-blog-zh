# 如何对文件和目录的“ls”终端输出进行颜色编码

> 原文：<https://blog.devgenius.io/how-to-color-code-your-terminal-output-from-ls-for-files-and-directories-ccecd5fc5fd9?source=collection_archive---------7----------------------->

![](img/b099cff500079fbc77651a88740ad5fb.png)

Dmitry Chernyshov 在 [Unsplash](https://unsplash.com/s/photos/coding?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

如果你经常使用命令行，比如 MacOS 中的 Terminal，那么你肯定使用过`ls`命令，它允许你查看给定目录中的文件和文件夹列表。

为了强调这一点，IBM 文档将`ls`描述如下:

> **ls** 命令将每个指定的*目录*参数的内容或每个指定的*文件*参数的名称，以及您通过标志要求的任何其他信息写入标准输出。如果没有指定*文件*或*目录*参数，则 **ls** 命令显示当前目录的内容。

`ls`非常普遍和强大，可以说你会经常运行它。从长远来看，这里的任何优化都可能是非常好的。

对于许多终端来说，`ls`命令的输出都是相同的颜色，这几乎是您已经设置的默认文本颜色。

这没什么大不了的，但是说实话，这种便利性还可以更好:例如，如果我们可以在文件和目录之间进行颜色编码会怎么样？从人类可读性的角度来看，这不是更清楚了吗？

幸运的是，有一种方法可以做到这一点。

# 简短的解释

```
ls -g
```

这大概是最短的了，对吧？试试看。实际上，只需向您通常的`ls`命令提供`-g`标志，您就可以看到不同类型的颜色。

# 冗长的解释

冗长的解释来自`ls`的手册页。本质上，`ls -g`实际上就是`ls -l`，除了它“禁止显示所有者和符号链接信息”，根据 [IBM 文档](https://www.ibm.com/docs/en/power6?topic=commands-ls-command)。

试试看，你会看到更多的信息:它显示了每个文件的模式、链接数量、所有者、组、大小(字节)和最后修改时间。如果该文件是一个特殊的文件，大小字段包含主要和次要设备号”，IBM 文档[还说。](https://www.ibm.com/docs/en/power6?topic=commands-ls-command)

从本质上来说，就是把它浓缩并保持颜色不变。

# 不要总是打`ls -g`

你可以通过不总是键入讨厌的`-g`来节省击键次数。

为此，用你最喜欢的文本编辑器(如`vim`)打开以下文件之一:

*   `~/.zshrc`
*   `~/.bashrc`

要知道是哪一个，可以运行类似`echo $SHELL`这样的命令让你知道如何选择。

在那里，添加下面一行:

`alias ls=’ls -G’`

这就是说，以后每次你使用`ls`时，它都会把它解释为`ls -G`。你会永远有颜色。

重要的是，如果您想在当前窗口中立即看到这些更改，您必须运行`source ~/.zshrc`或`source ~/.bashrc`。这是另一篇文章的主题，但是要知道您必须这样做才能看到变化，而不需要为您的终端打开一个新的选项卡或窗口。

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入媒体

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)