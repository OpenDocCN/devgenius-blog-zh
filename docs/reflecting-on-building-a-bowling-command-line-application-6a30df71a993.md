# 关于构建保龄球命令行应用程序的思考

> 原文：<https://blog.devgenius.io/reflecting-on-building-a-bowling-command-line-application-6a30df71a993?source=collection_archive---------6----------------------->

## 今天我会用不同的方式来写它

![](img/c18559f4ade46d0e0b55368abd3f4c44.png)

照片由[艾拉·克里斯滕森](https://unsplash.com/@ellabella124?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

对于我的第一个编程项目(不考虑一些糟糕的网站)😜)我写了一个保龄球命令行应用(CLI)。我使用 [JavaScript](https://www.javascript.com/) 作为语言， [Node.js](https://nodejs.org/en/) 作为运行时环境， [MySQL](https://www.mysql.com/) 作为数据库来存储关于玩家分数的信息。

该应用程序使一个或多个玩家能够在完成标准的十瓶保龄球游戏时跟踪他们的分数。游戏结束后，玩家会得到感谢，并显示一个高分列表。

CLI 本身在起作用

一场简单的保龄球比赛包含了如此多的逻辑，令人惊讶。备件和罢工给你额外的点，这意味着你的下一个或两个卷基本上分别被计算一倍。更不用说第 10 局也是最后一局，正常的规则似乎被推翻了——这一局中的任何补球或好球都不能获得加分，但可以让你再掷 3 次。这意味着最后一帧中的`XXX`或`9/X`或`XX3`是完全合法的分数。

正如你开始看到的，在保龄球运动中有很多边缘案例。因此，随着我的逻辑变得越来越复杂，我开始编写测试(用 [Jest](https://jestjs.io/) 测试框架),我害怕改变我的代码，因为害怕破坏其他东西。

我很高兴我做到了这一点，也体验到了在构建软件的同时编写测试的有用性。在我的测试文件中，我写了很多集成风格的测试(除了别的之外),其中我基本上断言了输出到[标准输出](https://thoughtbot.com/blog/input-output-redirection-in-the-shell#standard-output)的字符串。这种方法现在让我觉得非常脆弱——如果我改变这个字符串的格式，我所有的测试都会失败😭。了解了[测试金字塔](https://martinfowler.com/bliki/TestPyramid.html)，如果我今天重新编写软件，我会更专注于将这些转换成单元测试，也许会使用[表格驱动方法](https://medium.com/fsmk-engineering/table-driven-tests-in-javascript-c0c9305110ce)来测试所有不同的排列。因此，我将测试更小的代码片段，并且不依赖于输出字符串的格式。

上面是在我的代码中发现的一个相当常见的断言——这里断言连续滚动 10、6 和 4 后的输出。如果我今天重写这篇文章，我会把它转换成一个单元测试。

我做了很多不明智的决定，只是因为我不知道更好的方法(毕竟这是我的第一个编程项目)。例如，我只在游戏完成后连接数据库。这让我觉得有点奇怪——我会在应用程序初始化时创建一个连接，然后传递这个连接(或者像 JavaScript 一样传递文件)。如果我感觉真的很棒，我可以创建一个[连接池](https://github.com/mysqljs/mysql#pooling-connections)，尽管这种优化对于一个每游戏只有两次数据库查询的应用程序来说似乎有点大材小用。

然而，在不明智的决策中，最有可能的是对数据库凭证进行硬编码。但不仅如此，还要将它们提交到源代码控制中😱。呀，我今天绝对不会做这种事。但我认为这就是改进和变得更好的好处——你会犯错误，但也可以合理化为什么这也是个坏主意。

在第 10 帧也是最后一帧，计分逻辑变得相当复杂

总之，我对 CLI 的结果非常满意，我喜欢实现保龄球游戏中的所有逻辑。我很高兴我能够在职业生涯的早期就体验到测试的效用，因为我确实在项目团队中工作过，而这并不是我的工作重点。回顾我几年前写的代码也是一次有趣的经历——这是我通常从不做的事情，并且是一次有趣的思想实验，思考我如何用我现在拥有的知识重写软件。

*原载于*[*https://www . madeleinesmith . uk*](https://www.madeleinesmith.uk/portfolio/bowling-command-line-application/)*。*