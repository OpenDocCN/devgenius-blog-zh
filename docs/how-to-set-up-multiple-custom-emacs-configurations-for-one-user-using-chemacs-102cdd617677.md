# 如何使用 Chemacs 为一个用户设置多个自定义 Emacs 配置

> 原文：<https://blog.devgenius.io/how-to-set-up-multiple-custom-emacs-configurations-for-one-user-using-chemacs-102cdd617677?source=collection_archive---------1----------------------->

![](img/a1a03f4a2335ec1c46406802e4ab365f.png)

凯文·Ku 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

[Emacs 是一个庞大而古老的文本编辑器/IDE/操作系统/宗教](https://www.gnu.org/software/emacs/)，拥有极其专注的用户群，已经持续发展了 *44 年*。

由于模块化和可定制性是 Emacs 环境的主要吸引力，您可以想象几十年来已经创建和实现了多少插件。几乎所有你能想象到的在电脑上做的事情都可以在 Emacs 中完成。

随着人们越来越习惯使用 Emacs，他们的配置文件变得越来越详细和复杂。没有两个用户会有相同的设置，在大多数情况下，每个人的构建看起来一点也不像。

甚至还有非常受欢迎的定制配置，就像普通 emacs 上的大规模超集，并完全重塑其行为和外观，如 [Spacemacs](https://www.spacemacs.org/) 、 [Doom Emacs](https://github.com/hlissner/doom-emacs) 和 [Prelude](https://github.com/bbatsov/prelude) 。

如果你像我一样，总是沉迷于尝试新的玩具，这有损于你的生产力，那么尝试这些不同的配置可能会对你有吸引力。问题是 Emacs 是一个非常大的软件，每个安装都有一个单独的配置，其文件被绑定到用户的主目录。您基本上必须手动重命名不同的文件以切换到较新的配置，或者在每次您想重新开始或尝试新事物时完全覆盖它们。管理和备份所有这些文件是一件非常痛苦的事情。

[这就是 Chemacs 的用武之地](https://github.com/plexus/chemacs)！Chemacs 允许您在一个用户帐户上设置多个配置，没有任何冲突或手动移动设置！例如，我将默认的 Spacemacs 配置、Doom 配置和定制的普通 EmacsPlus 配置都放在一个目录中，我可以运行它们中的任何一个，而不会破坏任何东西。

下面是您需要做的所有工作*(注意:这些步骤将在 MacOS 或 Linux 上工作，而不是 Windows。有关 windows 说明，请参见 Chemacs 的自述文件。)*

1.  在你的电脑上安装 Emacs。有很多方法可以做到这一点，但我使用家酿安装 Emacs Plus，因为它的可选功能有助于与 Spacemacs 兼容。

```
brew tap d12frosted/emacs-plus
brew install emacs-plus
```

2.在您的个人文件夹中移动或重命名默认的`.emacs`文件。

3.将 chemacs 克隆到您的机器上。我将它放在我的主文件夹中的一个新的“emacs”目录中，在那里我保存了所有的 Emacs 配置。

```
git clone [https://github.com/plexus/chemacs.git](https://github.com/plexus/chemacs.git)
```

4.运行 chemacs 的安装脚本。emacs 文件到您的主目录，以便 emacs 自动读取。

```
cd chemacs
./install.sh
```

5.现在在您的主目录中有一个`.emacs-profiles.el` Emacs-Lisp 文件，它将允许您指向您的各种配置。

第一个参数是您想要的配置名，例如`“MediumEmacs”`，您可以通过在命令行中运行`emacs --with-profile MediumEmacs`来调用它。

下一个是`user-emacs-directory`，它指向你的配置文件所在的路径，例如`"/Users/Frank/Emacs/MediumEmacs.d"`。

所以我的中型 Emacs 产品线。el 文件应该是这样的:

```
("MediumEmacs" . ((user-emacs-directory . "/Users/Frank/Emacs/MediumEmacs.d")
```

MediumEmacs.d 文件夹包含我的 init.el 文件和我可能需要的任何其他配置文件。

如果你正在使用一个大规模的配置，它有自己的目录，像 Spacemacs 和它的`spacemacs.d`设置，你需要在这里把它指定为一个环境变量。这个目录也可以移动到 chemacs 的任何地方。就像用户配置的那样。

我的默认 emacs 概要文件(不需要运行`--with-profile`标志)设置为:

```
(("default" . ((user-emacs-directory . "/Users/Frank/Emacs/Spacemacs/.emacs.d")
  (env . (("SPACEMACSDIR" . "/Users/Frank/Emacs/Spacemacs/.spacemacs.d")))))
```

使用 Chemacs，我可以向该文件添加任意数量的配置，并且它们都可以从命令行轻松启动，而不会以任何方式相互干扰！

希望这能帮助任何 Emacs 新手开始尝试其他奇特的 Emacs 配置，而不会破坏他们心爱的定制版本！

[](https://www.gnu.org/software/emacs/) [## GNU Emacs

### Emacs 27.1 有各种各样的新特性，包括:内置支持任意大小的整数文本整形…

www.gnu.org](https://www.gnu.org/software/emacs/) [](https://github.com/plexus/chemacs) [## 神经丛/化学

### Emacs 配置文件切换器。在 GitHub 上创建一个帐户，为 plexus/chemacs 的发展做出贡献。

github.com](https://github.com/plexus/chemacs) [](https://www.spacemacs.org/) [## 专注于邪恶的 emacs 高级套件

### Emacs 高级套装专注于邪恶

专注于 Evilwww.spacEmacs.org 的 emacs 高级套件](https://www.spacemacs.org/) [](https://github.com/hlissner/doom-emacs) [## 赫里斯纳/杜姆-埃马克

### 这是一个古老的故事。一个顽固的，壳居住，和矫情 vimmer-羡慕现代的特点…

github.com](https://github.com/hlissner/doom-emacs)