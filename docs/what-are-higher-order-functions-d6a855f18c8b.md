# 什么是高阶函数？

> 原文：<https://blog.devgenius.io/what-are-higher-order-functions-d6a855f18c8b?source=collection_archive---------19----------------------->

你已经在用了吗？

![](img/9c8216c3b319a259bf25035fb1c9f2dd.png)

照片由 [Alessandro Bianchi](https://unsplash.com/@ale_s_bianchi?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/concept?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

尽管“高阶”这个术语听起来可能很奇特和复杂，但它实际上比你想象的要简单得多。事实上，你可能已经使用过高阶函数，甚至没有意识到这一点。

`higher-order function`是一个函数，要么接受一个或多个函数作为参数，要么返回一个函数。所有其他类型的功能都被认为是`first-order functions`。因为 JavaScript 接受高阶函数，这就是这种编程语言适合函数式编程的原因。

为了更好地理解这个概念，你首先需要了解一点关于函数式编程和`first class functions`的知识。顾名思义，函数式编程是一种用函数构建应用程序的范式。这是通过使用纯函数、递归和避免副作用、可变数据和共享状态来实现的。如果你想更全面地了解函数式编程，你可以在这里阅读我的帖子。

JavaScript 函数被视为一等函数，因为它将它们的函数视为一等公民。这意味着该语言支持将变量赋给函数，将变量存储在数据结构中，并将函数作为参数传递给其他函数。

高阶函数的一些例子是`.map()`和`.filter()`。这两个函数都接受一个函数作为参数。下面是一个使用`.map()`的简单例子:

。map()作为高阶函数

使用这些函数的好处是，它使理解程序和代码的意图变得更容易。高阶函数也比其他函数更容易重用，因为它们能够接受其他函数作为参数。这个想法不是创建大型复杂的函数，而是创建较小的函数来处理一部分逻辑，然后您可以将它们放在一个更高阶的函数中。

高阶函数的概念并不像人们想象的那样复杂。尽管如此，熟悉这个术语及其含义将有助于提高您的编程技能。更不用说，高阶函数是任何开发人员的武器库中的强大工具。