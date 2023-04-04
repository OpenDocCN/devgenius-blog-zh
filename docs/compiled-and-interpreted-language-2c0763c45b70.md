# 编译和解释语言

> 原文：<https://blog.devgenius.io/compiled-and-interpreted-language-2c0763c45b70?source=collection_archive---------14----------------------->

> 编译和解释语言可能很容易理解，但对初学者来说却很困惑。

![](img/326fd31f068629b293938fe301b942e9.png)

照片由来自 [Pexels](https://www.pexels.com/photo/green-and-yellow-printed-textile-330771/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的 [Markus Spiske](https://www.pexels.com/@markusspiske?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 拍摄

如果你正在阅读这篇文章，并试图弄清楚编译语言和解释语言之间的区别，你可能是一名软件工程师或开发人员。

这个话题并没有那么难，但是知道和理解这两件事仍然是值得的。谁知道呢，你可能会在下一次面试中被问到这个话题。

# 介绍

你有没有想过你的程序代码或脚本是如何运行的，然后它达到你想要的任何目的？

举个例子，如果你用 Python 编程，计算机怎么知道如何阅读你写的 Python 代码？或者，如果你正在用 Golang 编写代码，计算机如何理解你编写的 Golang 脚本并将其翻译成目的？

我们将讨论编译语言和解释语言的含义。

# 编译编程语言解释

c 语言**compiled programming language 是一种语言，在你的机器能够读取代码并运行它之前，需要先通过编译器进行编译。最终编译的代码通常是机器可读的二进制代码。**

编译器只是一个软件程序，它将人类可读的代码转换成二进制代码。

**Golang** 、 **Rust** 、 **C** 和 **C++** 是编译型编程语言的流行例子。不同的编程语言需要不同类型的编译器。

# 关于编译语言的更多信息…

任何编译的编程语言通常都非常快，因为你必须先编译它。

但是，请记住，编译器编译您的源代码所需的时间会因源代码语言和其他因素而异。

同样值得注意的是，通常当您编译或构建代码时，您的结果二进制代码通常是平台相关的。

这意味着，如果你在 Windows 机器上编译它，它通常不能在 Linux 或 MACOSX 上工作，除非你构建它在多个平台上工作。

![](img/21c342e16778bec1008808e9d0fc6ced.png)

照片由 [**马体芒果**发自](https://www.pexels.com/@mati?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)[Pexels](https://www.pexels.com/photo/person-wearing-black-hoodie-5952647/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

# 解释编程语言

**解释编程语言是在机器可以运行之前，由解释器解释的编程语言。**

解释语言依赖于解释器，解释器将读取人类可读的代码并逐行执行代码。

此外，解释器是读取代码的软件程序，与编译器不同，解释器不产生任何二进制代码。

**Python** 、 **Javascript** 、 **Ruby** 和 **PHP** 都是解释型语言的好例子。

# 关于解释语言的更多信息…

与编译语言不同，解释语言通常比编译语言慢。当你运行你的解释语言脚本时，比如 Python，解释器必须一行一行地读取、分析和执行每一行。

然而，解释语言的一个好处是，它们不依赖于平台。你可以在任何你想要的平台上运行你的脚本，只要你有适合那个平台的解释器。

# 关于主题的更多信息…

我们今天拥有的每一种编程语言，不管是编译的还是解释的，都应该属于这两者之一。

然而，也有被认为是编译和解释的语言。例如，Java 被认为既是编译语言又是解释语言。对于 Java，代码在 JVM 中运行之前首先被编译，JVM 通常是一个解释器。

C#也可以被认为是两者兼而有之，但是很多人都这么认为，但是我认为 C#是一种编译语言。

# 两者的比较

我不会为了展示哪一种更好而进行激烈的比较，因为对我来说，任何语言都是伟大的编程语言，如果你正确地使用它并抓住它的长处。

不要试图用一种编程语言去完成一些它不应该做的事情。

例如，使用 **Golang** 创建一个前端应用程序。我知道这是可以做到的，但努力这样做是浪费时间。或者，如果你使用 **JavaScript** 来做数据分析工作。它就是合不上。

# 结尾…

总而言之，在机器运行代码之前，编译语言需要先编译成二进制代码。另一方面，解释语言不需要编译，相反，解释器将逐行读取和分析代码并执行它。

下一篇文章再见！