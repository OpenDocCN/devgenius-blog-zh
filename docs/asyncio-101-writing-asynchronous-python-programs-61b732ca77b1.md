# Asyncio 101 |编写异步 Python 程序

> 原文：<https://blog.devgenius.io/asyncio-101-writing-asynchronous-python-programs-61b732ca77b1?source=collection_archive---------5----------------------->

异步代码是目前的一个大话题。这是一个相当宽泛的术语，有时可能意味着并发性、并行性，或者两者兼而有之。

> 关于并发性和并行性区别的详细讨论，请查看[这个 StackOverflow 线程](https://stackoverflow.com/questions/4844637/what-is-the-difference-between-concurrency-parallelism-and-asynchronous-methods#:~:text=Concurrency%20is%20having%20two%20tasks,on%20the%20same%201%20thread)。

出于本教程的目的，我们可以将异步代码看作是可以被分割成可以由计算机同时执行的逻辑片段/组件的代码。当一个任务暂停时(可能是在等待网络数据包)，另一个任务就开始了。他们重复这个过程，直到生命周期结束。它允许我们有效地使用系统资源，同时减少执行时间。

传统上，大多数 Python 代码是同步的，这意味着代码是按顺序执行的。计算机将开始一项任务，并等待该任务完成，然后再执行下一个命令。一个简单的例子:

这段代码的总执行时间应该约为 6 秒。计算机顺序或同步执行每项任务。

那么如何用 Python 实现异步代码呢？好问题。输入`asyncio`，添加到 Python 3.4 中的标准库中。
在过去，你必须使用`yield from`和一个生成器来创建协程。但是`asyncio`引入了`async/await`语法。它允许你做一些事情，比如:

太好了！时间缩短了 50%！好了，让我们来看看这段代码是做什么的。

我们首先从标准库中导入`asyncio`和`timer`。`asyncio`让我们在高层次上管理事件循环和协程，`timer`让我们监控执行时间。

接下来，如果你使用的是 Windows，你会想要告诉`asyncio`，这样它可以修改它的行为，否则你会得到`.Runtime Error: Event loop is closed`。

`wait_for_some_time` 是我们第一个异步函数。目前它没有做什么大的事情，但是 s *休眠*一会儿，并输出一条很好的消息。
`main`保存了整个程序流程，在这里你可以看到你通常会在`if __name__=="__main__"`块中看到的东西。我们实例化新的协程，并将它们传递给`asyncio.gather`。
请注意，我们用`async`标记了两个异步函数，因此 Python 知道它们是协程，而不是标准函数。

`asyncio.gather`在自己的*线程*中启动每个任务，然后等待所有任务完成。所以这里我们用它来分组任务并运行它们。我们还可以做更多的事情，快速浏览一下官方文档会有所帮助。

`asyncio.run`是 Python 中与异步程序交互的高级函数。它通常充当主程序循环的入口点。

在我的下一篇文章中，我将展示如何用 Python 创建异步 web 请求来加速网络操作。我们还将创建很酷的个人资料图片🙂。

更多资源:

 [## asyncio -异步 I/O - Python 3.10.4 文档

### asyncio 是一个使用 async/await 语法编写并发代码的库。asyncio 被用作…的基础

docs.python.org](https://docs.python.org/3/library/asyncio.html) [](https://medium.com/velotio-perspectives/an-introduction-to-asynchronous-programming-in-python-af0189a88bbb) [## Python 异步编程简介

### 介绍

到 Python Introductionmedium.com 中的异步编程](https://medium.com/velotio-perspectives/an-introduction-to-asynchronous-programming-in-python-af0189a88bbb) 

如果您对异步如何影响执行时间和伸缩性有疑问或想法，请回复这篇文章。

离别迷因😎

![](img/2bc0310537b2110ddb3f25e0667be604.png)