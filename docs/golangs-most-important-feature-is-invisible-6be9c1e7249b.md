# Golang 最重要的特点是隐形

> 原文：<https://blog.devgenius.io/golangs-most-important-feature-is-invisible-6be9c1e7249b?source=collection_archive---------1----------------------->

![](img/c6b0de136a968498df75e2da33f7dff8.png)

照片由[夏羽·加尼翁](https://unsplash.com/@metriics?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/threads?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

**TLDR**:Go 标准运行时允许异步编码，同时给开发者一个简单的同步接口。

我从事围棋专业工作已经有几年了，有一件事让我一再感到惊讶，那就是我认为围棋最重要的特性得到的关注是如此之少。

当被问及 Go 最重要的特性时，许多人会列出 Go 的简单性、C 互操作性、编译速度和并发性。所有这些都是重要的特征，但我认为最重要的特征几乎是看不见的，可能非常微妙。

让我们从一个展示该特性的代码示例开始，看看您自己能否猜出来？

你看到了吗？那里没有很多代码。老实说，这段代码演示了我所说的特性，但是因为它是标准 Go 运行时的一部分，所以它是不可见的。为了更好地突出这个特性，让我们看一个用 Java 编写的类似代码的例子。

长得挺像的。。。但事实并非如此。忽略代码较长，一个好的 IDE 会抵消这些权衡。为了让这段代码像 Go 代码一样运行，你需要做很多额外的工作。你猜到了吗？

我所说的特性是 Go runtimes 对阻塞 goroutines 的处理。当我第一次开始使用 Go 时，我认为 goroutines 是 Java 绿色线程的一个更有效的变体，但当我编写看起来像传统的阻塞 I/O 的代码时，我从来没有想到它实际上更类似于异步 I/O。很明显，在进行阻塞调用时，Go 能够更有效地调度工作。为了从 Java 中获得这种性能，你需要添加线程池、期货或其他一些异步库。

但是不要相信我关于性能的话，让我们进行一个快速的性能测试。这些结果是使用 Apache Bench 运行的，并且是在具有充足预热时间的同一台机器上运行的。

Apache Bench 在 Go Web 服务器上运行

Apache Bench 在 Java Web 服务器上运行

长话短说，Java 版本达到每秒 21K 请求，而 Go 版本达到每秒 36K 请求。

当然，这是一个合成的例子，但与我经常使用的模式相差不远。一个网络请求进来，它被处理，路由到一些代码，这些代码工作，通常是另一个基于网络或磁盘的请求，然后结果被返回。在 Go 中，网络和磁盘上的所有阻塞都得到了非常高效的处理。在 Java 和大多数其他语言中，您也可以获得这种好处，但是它通常需要更多的代码、计划和测试。围棋几乎是免费的。这是非常强大的。

在像 C 这样的语言中，你可以使用 libevent 这样的库来获得这种类型的行为。在 Java 中，你可以使用线程池和期货。在 python 中，你会依赖 async/await。但是在 Go 中你可以免费得到它，你可以像写同步代码一样写代码，在任何时候发生阻塞，goroutine 都可以被挂起，其他准备好工作的 Go routine 可以被调度。

所以，作为一个做很多这种模式工作的服务器工程师，我认为这是 Go 最强也是经常被误解的特性！

你怎么想呢?

要更深入地了解 Java & Go 并发校验:

[](https://medium.com/@genchilu/javas-thread-model-and-golang-goroutine-f1325ca2df0c) [## Java 的线程模型和 Golang Goroutine

### Golang 最重要的特性之一是处理高并发的能力。而 goroutine 是基础…

medium.com](https://medium.com/@genchilu/javas-thread-model-and-golang-goroutine-f1325ca2df0c)