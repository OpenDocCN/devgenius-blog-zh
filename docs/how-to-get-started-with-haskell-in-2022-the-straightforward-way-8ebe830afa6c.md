# 如何在 2022 年开始使用 Haskell(直截了当的方法)

> 原文：<https://blog.devgenius.io/how-to-get-started-with-haskell-in-2022-the-straightforward-way-8ebe830afa6c?source=collection_archive---------2----------------------->

Haskell 是一种独特而美丽的语言，值得学习，不为别的，只为它引入的概念。他们将扩展你对编程的看法。

我从 2011 年开始断断续续地用 Haskell 编程，在过去的 2 年里，我一直很专业地构建一个[编译器](https://github.com/wasp-lang/wasp)。虽然在这段时间里 Haskell 变得对初学者更加友好，但我一直看到初学者被众多流行的构建工具、安装程序、介绍性教育资源等选项所淹没。[哈斯克尔的主页](https://www.haskell.org/)接到过去十年的电话，让他们把 UX 还给他们也于事无补。

这就是为什么我决定写这篇固执而实用的帖子，告诉你**如何在 2022 年以最标准/最普通的方式开始使用 Haskell**。而不是担心你现在没有能力做的决定(比如“什么是最好的构建工具 Ifd？?")，可以专注于享受学习 Haskell 的乐趣:)！

# TLDR /超级自以为是的总结

1.  对于设置，使用 [GHCup](https://www.haskell.org/ghcup/) 。安装 GHC，HLS 和 cabal。
2.  作为构建工具，使用 [cabal](https://cabal.readthedocs.io/) 。
3.  对于编辑器，使用带有 [Haskell 扩展](https://marketplace.visualstudio.com/items?itemName=haskell.haskell)的 VS 代码。或者，使用 emacs/vim/…。
4.  加入[r/哈斯克尔](https://www.reddit.com/r/haskell/)。随时寻求帮助！
5.  要学习 Haskell 的基础知识，请阅读 [LYAH](http://learnyouahaskell.com/) 的书，并[在 Haskell 中构建一个博客生成器](https://lhbg-book.link/)。专注于理解材料，而不是完全理解所有东西；你以后会再回来的。

# 1.设置:使用 GHCup 进行无缝安装

GHCup 是 Haskell 的通用安装程序。它将安装你需要在 Haskell 中编程的所有东西，并在将来帮助你管理这些安装(更新、切换版本等等)。它使用简单，在 Linux、macOS 和 Windows 上工作方式相同。它为您提供了一个单一的中心位置/方法来处理您的 Haskell 安装，这样您就不必处理特定于操作系统的问题。

要安装它，请遵循 [GHCup](https://www.haskell.org/ghcup/#) 的说明。然后，用它来安装 Haskell 工具链(也就是你需要用 Haskell 编程的东西)。

Haskell 工具链包括:

1.  GHC -> Haskell 编译器
2.  HLS -> Haskell 语言服务器->您的代码编辑器将使用它来为您提供编辑 Haskell 代码的良好体验
3.  cabal -> Haskell 构建工具->你将使用它来组织你的 Haskell 项目，构建它们，运行它们，定义依赖项，等等。
4.  堆叠->阴谋集团替代，你现在不需要，因为我们会选择阴谋集团作为我们的建造工具

# 2.构建工具:使用阴谋集团

Haskell 有两个流行的构建工具: [cabal](https://cabal.readthedocs.io/) 和 [Stack](https://docs.haskellstack.org/) 。两者都应用广泛，各有利弊。因此，初学者经常面临的一个艰难选择是使用哪一个。

前一段时间，cabal 有点难以使用(复杂，“依赖地狱”)。这就是 Stack 被创建的原因:一个用户友好的构建工具，它解决了 cabal 的一些常见问题。(有趣的是，Stack 使用 cabal 的核心库作为后端！)然而，随着 Stack 的发展，cabal 也在进步。它的许多问题已经解决，使它成为初学者的可行选择。

2022 年，我推荐`cabal`给初学者。我发现它在开始时更容易理解(没有解析器)，它与 GHCup 和生态系统的其余部分一起开箱即用，最近似乎得到了更好的维护。

# 3.编者:VS 代码是一个安全的赌注

HLS (Haskell 语言服务器)为你的编辑器带来了所有很酷的 IDE 特性。所以，只要你的编辑器有一个利用 HLS 的不错的 Haskell 语言扩展，你就是好的。

最安全的选择是使用 Visual Studio 代码——它有一个很棒的 Haskell 扩展[,通常可以开箱即用。很多 Haskell 程序员也使用 Emacs 和 Vim。我可以确定他们也很支持哈斯克尔。](https://marketplace.visualstudio.com/items?itemName=haskell.haskell)

# 4.社区:r/haskell 等

Haskell 社区是一个寻求帮助和了解生态系统新发展的好地方。我更喜欢, [r/haskell](https://www.reddit.com/r/haskell/) - >它跟踪所有最新的事件，没有问题是没有答案的。还有 [Haskell Discourse](https://discourse.haskell.org/) ，这里发生了很多讨论，包括更官方的讨论。许多 Haskellers 仍然活跃在 [IRC](https://wiki.haskell.org/IRC_channel) 上，但我发现它太复杂、太过时了，无法使用。

查看[*https://www.haskell.org/community*](https://www.haskell.org/community)*获得 Haskell 社区的完整列表。*

# 5.学习:不需要数学学位，随便拿本书就行

有一个流传很广的神话，你需要专门的数学知识(范畴论博士！)才能正确的用 Haskell 编程。从我的经验来看，这与事实相去甚远。它肯定是不需要的，我严重怀疑它有帮助，即使你有它。也许对于一些非常高级的 Haskell 来说是这样，但是对于初级/中级水平来说肯定不是。

相反，学习 Haskell 和学习其他语言是一样的->你需要理论和实践的健康结合。主要的区别是会有比你习惯的更多不寻常的/新的概念，这需要一些额外的努力。但是这些新概念也是学习 Haskell 如此有趣的原因！

我推荐从一本适合初学者的书开始， [LYAH](http://learnyouahaskell.com/) 。它有一个在线版本，你可以免费阅读，或者如果你喜欢实体书，你可以购买印刷版本。

如果你不喜欢 LYAH，可以考虑其他适合初学者的流行书籍(不过没有一本是免费的):

1.  [Haskell 编程从第一原理出发](https://haskellbook.com/)
2.  [用 Haskell 编程](https://www.manning.com/books/get-programming-with-haskell)
3.  用 Haskell 编程

无论你读哪本书，不要在那些让你困惑的概念上停留太久，尤其是在书的结尾。有些概念只是需要时间去理解；不要指望第一次就能完全掌握。无论您从第一次阅读中掌握了什么，都有可能足以开始您在 Haskell 中的第一个项目。你总是可以在以后回到那些复杂的概念，更好地理解它们。此外，不要羞于询问社区->有许多 Haskellers 很乐意支持你的学习！

一旦你第一次看完这本书，我建议你做一两个项目。你可以自己想出一个主意，或者你可以跟随其中一本指导你完成它的书。

例如:

1.  [通过构建博客生成器学习 Haskell](https://lhbg-book.link/)->免费，从零知识开始，甚至可以作为第一资源
2.  简单的 Haskell 手册 - >不是免费的，期望你已经知道了 Haskell 的基础知识

一旦你对项目有了更多的经验，我会推荐你重新阅读你的入门书籍。这一次，你可以跳过你已经知道的部分，把重点放在之前比较混乱的地方。你可能会更容易理解那些更难的概念。

另外，如果你正在寻找一点额外的动力，看看我的队友 Shayne 最近写的关于他和 Haskell 的旅程的博客文章。他从 2021 年末开始，已经取得了巨大的进步！

祝 Haskell 好运！如果你对我或 Wasp 团队的其他人有 Haskell 的问题，请在 `*“martin” ++ “@” ++ concat [”wasp”, “-”, “lang”] <> “.dev”*` *给我写信，或在*[*Wasp-lang Discord server*](https://discord.gg/rzdnErX)*中写信给#haskell channel。*