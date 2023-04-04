# 关于 JavaScript 提升你应该知道的 4 件事

> 原文：<https://blog.devgenius.io/4-things-you-should-know-about-javascript-hoisting-8b53803a1ed0?source=collection_archive---------3----------------------->

## 用实例理解 JavaScript 提升

![](img/c6436a8f34b6ea7bc8b24e09dd31bf9f.png)

费尔南多·埃尔南德斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 什么是吊装？

提升是 JavaScript 的默认行为，它在执行程序代码之前将变量声明移动到作用域的顶部。由于 JavaScript 中提升的概念，我们可以在声明函数之前调用它，或者在声明变量之前使用它，因为所有的函数和变量声明都放在它们作用域的顶部，不管它们在整个程序中的什么地方声明。

所以在这篇文章中，我决定给你一些有用的技巧，你应该知道在 JavaScript 中提升。

![](img/ab508a890661e98da8e822fdbd1e9a00.png)

[法托斯 Bytyqi](https://unsplash.com/@fatosi?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 1.JavaScript 声明被挂起

在 JavaScript 中，变量可以在使用后声明。换句话说，变量可以在声明之前使用。看看下面的例子:

在声明前使用变量。

在使用变量之前声明变量。

正如你在第一个例子中看到的，变量 x 被用在声明之前，这是由于提升的概念，将所有的声明移动到作用域的顶部，而不管它们在整个程序中的位置。

# 2.let 和 const 关键字

在 JavaScript 中，所有用 **let** 或 **const** 定义的变量都会被提升到顶部，但不会被 ***初始化*** 。这意味着变量在声明之前不能使用。

使用 **let** 关键字。

上面的例子将导致一个 **ReferenceError。同样的事情也适用于**常量**关键字。**

# 3.JavaScript 初始化不会被挂起

JavaScript 只提升声明，不提升初始化。这意味着它将只提升变量关键字和变量名，而不是分配给该变量的值。让我们看看下面的例子:

初始化不会被提升。

在上例中，y 将打印未定义的**。**因为提升，y 在使用之前已经声明了，但是因为初始化没有提升，y 的值没有定义。

# 4.在顶部声明你的变量

在顶部声明变量总是避免错误的好规则，因为这是 JavaScript 解释代码的方式。你也可以使用严格模式，它不允许在声明前使用变量。

# 结论

JavaScript 提升是您必须理解的重要概念之一。如果一个开发者不懂提升，程序可能会有 bug。这就是这篇文章，我希望你今天学到了一些新的东西。