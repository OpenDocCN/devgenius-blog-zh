# 如何使用管道技术编写更好的 JavaScript

> 原文：<https://blog.devgenius.io/how-to-write-better-javascript-using-plumbing-techniques-68aa78be817c?source=collection_archive---------2----------------------->

![](img/52aeaf30f24c96732824a1f532c9226b.png)

由 [Samuel Sianipar](https://unsplash.com/@samthewam24?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

浪费思想是一件可怕的事情，最快的方法之一就是不要让它足够开放。尽管在我的开发生涯中，我大部分时间都在用 JavaScript 写作，并且我热爱这种语言，但我从来不认为自己是一名 JavaScript 开发人员。在需要的时候，我用 Java、Python 或 C#写东西从来没有问题，我也喜欢探索我不了解的语言，并努力发挥它们的优势。

这正是 Elixir 的情况——我以前公司的一位同事说服我参加关于 Elixir 的在线课程，我非常喜欢它——它有很酷的语言功能，如列表理解、高级模式匹配，但我最有共鸣的肯定是管道操作符。不幸的是，我在工作中没有太多机会用 Elixir 编写，但是我找到了使用我在用 JavaScript 编写时学到的东西的方法。请继续阅读，寻找答案！

![](img/4a67f81baa78afb2a5abb640525c8e4b.png)

照片由 [Abhi Bakshi](https://unsplash.com/@potofgold07?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

我想让你们熟悉的第一个概念是 **function currying** (令人惊讶的是，这个名字不是来自一个流行的印度菜肴，而是来自一位美国数学家和逻辑学家 [Haskell Curry](https://en.wikipedia.org/wiki/Haskell_Curry) )。我一会儿会解释它有什么用，因为现在我们需要知道的是，用固定数量的参数来处理一个函数使得在不止一次调用中传递参数成为可能。似乎很复杂？也许这个例子会使它变得清楚:

当然，我们需要自己实现`curry`函数，或者——就像上面的例子中一样——使用一个现有的实现，在本例中，它是函数库`ramda`的一部分，我将在下面的例子中使用它。

![](img/e6fc54c29d36f9f6625df42795ec1ed8.png)

Gabriel Barletta 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

第二个有用的概念是**函数组合**。这是怎么回事？根据数学定义，如果有一个函数`f(x)`和另一个`g(x)`，就有可能将它们组合成一个函数`h(x) = g(f(x))`。假设我们需要给一个数加 2，然后将结果提高到 2 的幂，再从得到的数中减去 3。

代码并不复杂，但是乍看之下并不是那么明显；可读性不是很大。此外，如果我们需要添加一个类似的计算，但参数略有不同(如添加 5，提高到 3 的幂，等等)，会发生什么？)?除了不可读之外，代码也不太可重用。我们能做些什么来改善它？让我们从提取所有的原子动作开始。

这将允许我们以一种更加可重用的方式编写代码:

这无疑是更可重用的，但仍然不是非常可读——我们只对输入做了三处修改，如果我们有更多的修改，会发生什么呢？让我们试着让它更容易理解:

这里发生了什么？我们采用了三个函数，比如`f(x)`、`g(x)`和`h(x)`，我们创建了一个全新的函数，它是三个函数的组合:
、`k(x) = h(g(f(x)))`。执行的顺序与数学定义中的完全一样——从内向外。我们从最后一个我们想要构建的函数开始，然后向第一个函数前进。虽然有些人喜欢这种方法，但我认为它违反直觉——在我看来，它需要额外的思维周期来调整。幸运的是，有一个简单的方法可以让它变得更简单:

我们现在看到的更自然一些——执行的顺序和自然的阅读顺序是一样的——我们一看就知道到底发生了什么。为什么是管道？这仅仅是因为我们通过连续的函数来传输数据以获得最终结果。还能进一步简化吗？让我们暂时回到原子函数。

这里发生了两件明显的事情。首先，我们颠倒了每个函数中参数的顺序(实际上对于`add`并不需要——因为加法的交换性——但是这样所有的函数都是一致的)，其次，我们简化了所有的函数。为什么让我们看看这会如何影响我们的计算:

首先清楚可见的是，这个函数现在可以用简单的英语阅读——我们加上 2，然后提高到 2 的幂，再减去 3！这主要归功于阿谀奉承。让我们来看看`power`功能:

如您所见，currified `power`现在可以同时接受两个参数，或者——如果只提供第一个参数(现在是指数),则返回一个接受基数并对其进行之前指定的幂的函数。

那有什么用？嗯，在我看来——首先，它增加了可读性、可组合性和可维护性。可读性的提高是毋庸置疑的——如果管道函数命名正确(或语义正确),代码应该像普通英语一样。可组合性的好处也很明显——如果需要将一个数提高到 5 的幂，然后加上 7，就不需要从头开始编写整个代码。可维护性也是如此——如果需要添加三个而不是两个，那么如何更改管道中的值就一目了然了，然而在编辑`Math.pow(myNumber + 2, 2) — 3`时很容易出错。

当然，这些例子非常简单，乍看之下好处可能并不明显，但拥有复杂得多的原子函数并不少见——例如在图像处理中(例如，本文提到的对图像数据的所有操作:[https://medium . com/JIT-team/how-to-create-a-musical-instrument-with-no-notes-using-JavaScript-EC 6a 83333 aa 4](https://medium.com/jit-team/how-to-create-a-musical-instrument-with-no-notes-using-javascript-ec6a83333aa4)将构成一个优秀的管道)。

我希望你会发现这些有用——如果是这样(或者不是！)，请在评论里告诉我！

**我指导软件开发人员**。请在 [MentorCruise](https://mentorcruise.com/mentor/piotrjaworski/) 上给我留言进行长期指导，或者在[共同导师](https://www.codementor.io/@piotrjaworski)上留言进行个别指导。