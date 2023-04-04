# 理解 JavaScript 中的同步和异步代码

> 原文：<https://blog.devgenius.io/understanding-synchronous-and-asynchronous-code-in-javascript-46734bf29914?source=collection_archive---------1----------------------->

## 了解同步和异步 JavaScript

![](img/344fca75c1b78bb207a9d7feb9d19655.png)

照片由[福蒂斯·福托普洛斯](https://unsplash.com/@ffstop?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 介绍

JavaScript 是一种**单线程**编程语言。这意味着它有一个调用堆栈和一个内存堆。正如预期的那样，它按顺序执行代码，并且必须先执行完一段代码，然后才能进入下一段。然而，由于 V8 引擎，JavaScript 可以是异步的，这意味着我们可以一次在代码中执行多个任务。因此，在本文中，我决定向您展示 JavaScript 中同步和异步代码的区别。

![](img/72de222714d379fc0d0b279002bc25d9.png)

照片由[欧文·史密斯](https://unsplash.com/@mr_vero?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 同步 JavaScript

JavaScript 中的同步代码将从文件的顶部开始，一直执行到底部，每一行都按顺序执行，直到它到达底部，它将停止。例如，如果一个函数需要一段时间来执行或必须等待某个东西，它会在此期间冻结所有东西，因为 JavaScript 中的同步代码一次只能执行一个任务，它会等到某个特定的语句执行完，然后移动到下一个语句。这种情况的一个很好的例子是窗口报警功能`**alert("Hello World")**` **:**

JavaScript 中的同步代码。

在点击 **OK** 并解除警告之前，您根本无法与网页互动。

# 异步 JavaScript

我们上面提到 JavaScript 是单线程语言，那么我们如何用 JavaScript 获得异步代码呢？

JavaScript 引擎有一个 web API 在后台处理任务。调用堆栈识别 web API 的功能，并将它们交给浏览器处理。一旦这些任务被浏览器完成，它们就返回并作为回调被推送到堆栈上。这有助于同时执行多项功能。因此，异步代码从文件的顶部到底部开始执行，但在执行过程中，它会遇到一些异步函数，在这些函数中，它会将异步代码与其余代码分开执行。让我们看一个简单的例子:

JavaScript 的异步代码。

在上面的例子中，第一行代码将开始执行，因为它首先在堆栈上，所以“first”被打印出来。然后它将移动到下一行，javascript 引擎将注意到一个异步函数**setTimeout****需要 1000 毫秒来执行，因此引擎将把它推给 WebAPI 来异步完成。调用堆栈继续运行，而不关心交给 Web APIs 的代码，并打印出`**console.log("three")**`。**

**我们代码的输出。**

**由于`**setTimeout**`没有结束，它返回`**undefined**` **。****

# **结论**

**同步和异步编程是令许多初级和中级开发人员困惑的概念。这也是 JavaScript 中非常常见的一种模式。所以，在你开始使用语言和框架来充分发挥潜力之前，你需要对它有一个深刻的理解。**