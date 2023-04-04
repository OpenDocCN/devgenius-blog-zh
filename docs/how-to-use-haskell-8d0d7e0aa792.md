# 如何使用 Haskell

> 原文：<https://blog.devgenius.io/how-to-use-haskell-8d0d7e0aa792?source=collection_archive---------4----------------------->

![](img/5143029201f460836e8077ce82aa3056.png)

# 目的

由于我对函数式编程语言感兴趣，我正在学习 Haskell。有很多函数式语言，但我选择了 Haskell，因为这是函数式编程非常严格和独特的特性(注意:Haskell 可以写成过程式语言，但一般来说，这种语言的用例更多的是函数式编程，而不是过程式编程)。

# 哈斯克尔是什么？

Haskell 是 1985 年发明的一种编程语言，以数学家和逻辑学家 Haskell Curry 的名字命名。Haskell 是一种具有不变性的纯函数式语言。换句话说，如果你在 Haskell 的任何变量中放入任何值，这些变量不能被任何值覆盖。所以，如果你想替换数组中的一些值，你需要创建一个新的副本来保持不变性。尽管我写了几篇关于 redux toolkit 的文章，并且在这些文章中提到过，但是这种不变性在 redux 中也很重要。基本上你不能改变任何值(多亏了 immer，你并不真正关心 redux toolkit 中的不变性，但这个主题不是本文的范围)。为什么不变性很重要？因为如果编程语言有严格的不变性，任何副作用都不会发生(严格来说，你可以控制副作用)。也就是说，这种不变性防止了严重的错误。Haskell 还有其他特性。

# 特性(除了不变性)

*   懒惰的

这有点像 react 中的懒惰统治，它在需要值的时候加载。由于 Haskell 有一个默认的懒惰特性，这有助于提高性能

*   类型推理

由于这种语言具有很强的类型推断能力，所以您不需要真正关心类型。当然你仍然需要关心类型，但是 Haskell 推导出类型并帮助它。所以如果某些变量明显有类型，这种语言会自动推断出类型并在编译时加上。你不必浪费你的时间在那些公认的类型上。

*   静态类型化

这不是 Haskell 的唯一特性，但知道这一点很重要。这个特性在 Java、C 和任何其他语言中都有使用，它可以防止严重的 bug 在编译时检测错误。然而，正如我已经提到的，Haskell 也有很强的类型推断特性，所以你可以节省时间，编译后不会有任何错误。

为什么有用？例如，你不需要对一个 Haskell 程序进行完美或非常严格的测试，因为 Haskell 在编译过程中不需要任何测试就能发现很多错误。这并不意味着您不再需要编写任何测试，但至少您可以节省时间来编写测试，这完全归功于 Haskell 的特性。

此外，由于这种语言非常抽象，所以学习其他语言也很有帮助，因为如果你熟悉高度抽象的语言，你的抽象技能就会提高。

Haskell 的缺点如何

*   学习限制

由于 Haskell 是一种纯函数式编程语言，其语法与其他过程式语言和非纯函数式语言有着本质的区别。所以，学起来稍微有点难。此外，正如我已经提到的，这种语言需要你有抽象的能力，所以很难熟悉这种语言，特别是对于初学者。

*   控制副作用

正如我已经提到的，基本上这种语言不接受任何副作用，因为这是一种纯粹的函数式语言。这是防止严重错误的一个非常有用的特性，但是我们需要输入或输出等副作用的时候呢？在这种情况下，你需要像 I/O 一样使用 monad。Monad 是一个稍微有点难的概念，所以我在本文中不涉及，但是你需要使用一些 Monad 来处理任何副作用。

*   不太受欢迎

尽管这是一种古老的语言，而且有一些不错的社区，但这种语言还是有点次要。这就是为什么尽管 Haskell 中有很多库，但库的数量比其他任何流行语言(如 Java)都少。这不是一个严重的问题，但你有时会觉得很难找到一个合适的解决方案。

关于 Haskell 已经说得够多了，让我们深入一些例子。

# 示例(Hello world)

1.  设置您的环境(有多种方式来设置，但在这种情况下，我使用堆栈。)

堆栈安装(mac 或 linux)

```
$ curl -sSL [https://get.haskellstack.org/](https://get.haskellstack.org/) | sh
```

堆栈安装(windows)

```
Set-ExecutionPolicy Bypass -Scope Process -Force;[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072;Invoke-Command -ScriptBlock ([ScriptBlock]::Create((Invoke-WebRequest [https://www.haskell.org/ghcup/sh/bootstrap-haskell.ps1](https://www.haskell.org/ghcup/sh/bootstrap-haskell.ps1) -UseBasicParsing))) -ArgumentList $true
```

因为我在本文末尾放了一个官方参考文件的链接，所以如果你需要不同的方法或帮助，请查看。

这个栈意味着像 rbenv(ruby)、pyenv(python)、nodebrew(node.js)这样的包管理器。所以你需要安装 GHC，它是 Haskell 世界中最流行的编译器之一。

```
stack setup
```

如果您成功安装了 GHC，您可以看到下面的信息时，您键入以下命令。

```
stack ghc -- --version //result
The Glorious Glasgow Haskell Compilation System, version 9.0.2
```

1.  创建一个 hello.hs 文件，并编写如下代码。这个主函数非常重要，因为你总是需要它。如果你编译某个程序，这个函数总是首先被执行。符号“=”可以像其他语言一样表示变量。然而，函数的语法和 Haskell 中的=是一样的，所以这很简单，但是如果你不熟悉任何函数式语言，你需要稍微注意一下。这个 putStrLn 是一个函数。这种语言分为两种不同的函数，一种是纯函数，另一种是非纯函数，它有一些副作用，叫做 I/O 动作。正如我已经提到的，当你想使用一些副作用，如输入或输出，你需要有这个 IO()。这个 putStrLn 得到一些字符串并返回一些动作。

```
main :: IO ()
main = putStrLn "Hello, world!"
```

1.  使用以下命令运行 hello.hs 文件。当您运行此命令时，您将有三个不同的编译文件“hello”“hello . hi”“hello . o”作为二进制文件。

```
stack ghc hello.hs
```

1.  运行下面的命令编译你的文件，你可以看到你写的字。

```
./hello
Hello, world!
```

# 结论

正如我展示的一个非常简单的例子，您可能不知道 Haskell 的强大之处。然而，如果你处理一些递归值，比如计算斐波那契数，你可以只写一行。如果你使用任何其他的过程语言比如 Java 或者 JavaScript，你需要写 10 行以上，因为我们需要使用 for 循环。所以如果你熟悉这门语言。

# 参考

官方文件(哈斯克尔):[https://www.haskell.org/](https://www.haskell.org/)

官方文档(Stack):[https://docs . haskellstack . org/en/stable/install _ and _ upgrade/# windows](https://docs.haskellstack.org/en/stable/install_and_upgrade/#windows)

什么是 Haskell，谁应该使用它？:【https://www.47deg.com/blog/what-is-haskell/ 

什么是 Haskell 编程语言？:[https://www . geeks forgeeks . org/what-is-haskell-programming-language/](https://www.geeksforgeeks.org/what-is-haskell-programming-language/)

純粋関数型言語 Haskellを使ってみた: [https://qiita.com/k-iida/items/8023f55dec2d3c3c73f7](https://qiita.com/k-iida/items/8023f55dec2d3c3c73f7)

Haskellらしさって？ 「型」と「関数」の基本を解説！(第二言語としてのHaskell): [https://eh-career.com/engineerhub/entry/2017/08/25/110000/?PK=F85EE5&isLoginAfter](https://eh-career.com/engineerhub/entry/2017/08/25/110000/?PK=F85EE5&isLoginAfter)

感谢您的阅读！！