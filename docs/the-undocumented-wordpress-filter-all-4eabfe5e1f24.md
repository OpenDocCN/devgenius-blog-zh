# 未记录的 WordPress 过滤器—全部

> 原文：<https://blog.devgenius.io/the-undocumented-wordpress-filter-all-4eabfe5e1f24?source=collection_archive---------11----------------------->

![](img/d8c5b9f15eab51179ee69627185a1931.png)

照片由[韦德·奥斯丁·艾利斯](https://unsplash.com/@wadeaustinellis?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

我的上一周在 WordPress core 上疯狂的开发，因为我试图做一些[代码清理](https://medium.com/dev-genius/clean-php-code-57da4cf75537?sk=6df5744748856b27e5aa6d74805a1cc5)。其中一部分是最后安装 xDebug，并使它与 WordPress 开发环境一起工作。

一旦我开始工作，我就做了任何有自尊的开发人员都会做的事情。

我玩它。

我在请求的最开始——index.php 的第一行——设置了一个断点，然后单击“单步执行”,直到我的手指累了。如果你从来没有这样做过，让我说…

不要。

它长得可怕。

我知道，但我显然也有点讨厌自己，所以我继续前进。直到我意识到我实际上看到了什么，并产生了好奇心。

我一直在浏览过滤方法，因为这是 WordPress，我当然会这么做，但是我注意到了一些我以前从来没有过的东西。

# WordPress 有一个全部过滤器

我检查了[钩大书](https://adambrown.info/p/wp_hooks/hook)以及[官方名单](https://codex.wordpress.org/Plugin_API/Filter_Reference)，它不在任何地方列出。所以我写了一个快速和肮脏的插件，只是为了确保我没有疯。

果然，当我加载我的页面时，我有一个编号为 6000 的文件`all-log.txt`。

它现在正式引起了我的兴趣，所以我重写了我的插件以获得更多的信息。例如，有多少个参数(如果有的话)被传递给 all 过滤器？

很快我就知道它在向过滤器传递参数，并且传递了不同数量的参数。所以我进一步调查。到底通过了什么*？*

*我再次重写了插件。*

*这给了我一条我真正需要知道的信息:*它传递了一切。**

*几乎所有的东西。包括钩子的名称——作为第一个参数。*

# *什么是好的？*

*嗯，我还不知道。我刚发现它在那里。*

*在我的脑海中，我可以看到使用它来获得更好的性能。例如，跟踪过滤器之间的时间。你也可以用它来跟踪哪些过滤器对你可用，而不必搜索插件——因为这是为所有的*钩子运行的，而不仅仅是 WordPress 核心钩子。**

*无论哪种方式，找到一个我已经使用了十多年的软件的未记录的功能总是让我微笑，我想我会分享。*

*如果你对这篇文章感兴趣，你会爱上我的 Twitter 上的技术意识流。 [*头那边，给我跟着*](https://twitter.com/n00bJackleCity) *。你不会失望的。**