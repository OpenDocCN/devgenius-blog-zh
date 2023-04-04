# 如何在超级终端中打开高亮显示或选择时复制文本

> 原文：<https://blog.devgenius.io/how-to-turn-on-copy-text-on-highlight-or-select-in-hyper-terminal-fe163d9a126b?source=collection_archive---------2----------------------->

![](img/a6ed6f153e3830e1559824a590720a07.png)

如果你使用 [Hyper](https://hyper.is/) ，那么你可能有兴趣开启的一个功能是，当你在终端窗口中高亮显示文本时，可以复制文本。

这是我这些年来刚刚习惯的东西，所以当我重新安装 Hyper 的时候，我不能，这是不和谐的。我会突出显示文本，期望它被传播到我的剪贴板，然后粘贴它，却发现我最后复制粘贴的内容。

按下`command + C`以确认我确实复制了的额外动作减慢了我的速度，所以我开始研究如何在复制文本的同时简单地高亮显示它——就像我习惯的那样。在本文中，我将介绍如何在 Hyper 安装中实现这一功能。

# Hyper 有一个配置文件

Hyper 很酷有很多原因，其中之一是它的超级可定制性。

这些配置可以在系统中位于`~/.hyper.js`路径的文件中找到。你可以用 Sublime 或 Atom 之类的文本编辑器打开它，或者你可以直接进入 Hyper，用 Vim 打开它:

`$ vim ~/.hyper.js`

# 做出改变

一旦打开，你会看到一堆配置；在另一篇文章中，我可以回顾一些您可能想要更改的兴趣。

但是，对于本文，让我们直入主题:您将想要搜索文件中的`copyOnSelect`。在 Vim 中，您可以键入以下内容并按 enter 键:

`/copyOnSelect`

一旦出现，将值(默认为`false`)设置为`true`。然后，保存文件。在 Vim 中，您可以键入以下内容并按 enter 键:

`:wq`

现在，试着复制/突出显示一些东西，然后粘贴。应该能行！如果没有，那么你可以打开一个新的超级标签或窗口，然后它应该工作。

现在你可以选择复制了，你可以开始你的生产力竞赛了！

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入媒体

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)