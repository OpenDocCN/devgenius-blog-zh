# Golang 的背景，意义。

> 原文：<https://blog.devgenius.io/golang-context-the-significance-1611fc248d25?source=collection_archive---------6----------------------->

> “没有上下文的内容就是噪音。”

![](img/b16ba8bfe88d70b98931bc81a4302e99.png)

演职员表:[@ mighty hummbird](https://unsplash.com/@mightyhummingbird)

在惯用的 go 中使用`context`是不可避免的，尤其是当你使用 Go 并发时。正如“语境”的定义所述:- *形成事件、陈述或想法的背景的环境，根据这些环境可以完全理解*；简单地说，`context`包可以用来让函数或 [goroutines](https://go.dev/tour/concurrency/1) 根据系统中的某些事件来动作。

下面这篇文章介绍了为什么上下文包很重要，以及如何在生产系统中使用它来提高性能和功能，并揭开`ctx`变量的神秘面纱，不要在看到以`context.Context`作为第一个参数的函数时就使用`context.Background()`。让我们来了解一下 Golang 中的上下文包。

# 说真的，我们为什么需要上下文包？

每当我们想要在应用程序中传递“上下文”或公共范围数据时，我们就使用上下文包。归结起来就是以下几个概念。

*   **听取消。** *场景*:上下文取消的一个经典例子是服务的正常关闭。当系统关闭时，它应该取消所有后台进程，并在后台处理正在进行的任务。
*   **通知超时和截止时间。**
    *场景*:我们构建的系统请求一个服务，它有一个有限的超时(比如说 5 秒)，所以如果服务超时，我们需要取消我们的 goroutines，不要浪费计算资源。
*   **传递杂项键值。** *场景*:当您创建一个中间件来解析令牌并转发经过身份验证的用户信息时，您可以使用请求`context`并为其附加键值。

上下文接口定义如下，[检查源代码](https://github.com/golang/go/blob/master/src/context/context.go)，当您开始一个实现时，您可以选择创建一个基本上下文，并将其传递给 [go 例程](https://go.dev/tour/concurrency/1)。

# 上下文接口

你总是使用`context.Background()`或`context.Todo()`创建一个基本上下文，当你在官方 go 源代码中检查实现时，你可以看到这些上下文实际上是一个整数`emptyCtx`实现了`context interface`。

由`Background()`返回的上下文通常被用作整个上下文调用链的根上下文，作为一个抽象，它实际上什么也不做。

从技术上讲，当你查看[源](https://github.com/golang/go/blob/17083a2fdf4475c3f11a3e6a0ef8cb595a5fc4d6/src/context/context.go#L199-L202)时，`context.Background()`和`context.Todo()`都是一样的，都是`emptyCtx.`，那么我们为什么需要`context.Todo()`？当您不确定如何处理上下文，但需要编写惯用的 go 时，它用于重构和代码分析。

> 代码应该使用上下文。当不清楚使用哪个上下文或者上下文不可用时(因为周围的函数还没有被扩展以接受上下文参数)，TODO。

# **1。听听取消。**

您可以在整个程序中使用`context.Context`来处理取消。当您在应用程序中传递一个主上下文时，侦听关闭事件并在系统中触发优雅的关闭级联。

在上面的例子中，你可以看到`context.WithCancel`是通过`longBackgroundWorker`函数传递的，我们使用一个 ticker 来模拟一个正在运行的进程。

当`Ctrl+C`或中断被触发时，程序将[通知](https://gist.github.com/sivsivsree/d731e58c30379f11a7d70a4ef163d9bd#file-ctx_listen-go-L17)或`exit chan`，并将通过将值传递给完成中的通道来取消上下文。

> 取消不止一次是可以的，但是第一次之后，就没用了。

## 可以用在哪里？

正常关机场景如上所述，另一个场景是，当程序在分块和流式传输过程中中断时，您正在将一个大文件流式传输到另一个系统，您可以保留一个检查点并通知另一端发生了中断，稍后从您离开的地方继续…您明白了吗？

# 2.通知超时和截止日期。

超时允许程序继续运行，为最终用户提供了更好的体验。超时/截止时间是针对损坏或失败的依赖关系的一种有用的防御措施。

`context.Timeout`和`context.Deadline`在概念上类似。设置一个`context.Timeout`需要持续的时间- `time.Duration.`

```
ctx, cancel := context.WithTimeout(ctx, 3*time.Second)
```

`context.Deadline`是类似的，除了你指定一个截止日期`time`当你想要上下文取消，而不是一个持续时间。

```
notAfter := time.Date(2023, time.May, 12, 45, 0, 0, 0, time.UTC)
ctx, cancel := context.WithDeadline(ctx, notAfter)
```

以下代码是如何使用超时的示例。它将每隔[一秒](https://gist.github.com/sivsivsree/c53c6db8a57f704cc1d3ce8d967d58bc#file-ctx_timeouts-go-L22)调用一次提供的 URL，并在提供的持续时间内等待获得结果，如[第 41 行](https://gist.github.com/sivsivsree/c53c6db8a57f704cc1d3ce8d967d58bc#file-ctx_timeouts-go-L41)所示。

请求中使用超时的示例

## 为什么要使用超时/截止时间

1.  **返回一个错误**:当一些事情发生时，比如网络请求、数据库连接，我们不希望我们的程序挂起并无限期等待，而是尝试一段时间并提供一个错误。
2.  **回退值**:同样，如果事情出了问题，并且期望一个比错误值更大的值，我们可以返回一个回退值，比如上次缓存、默认值等等。
3.  **启动重试**:我们可以在指定的超时后再次尝试错误的依赖关系。

> 在这么多要担心的事情中，暂停真的是一件好事。

# 3.**传递杂项键值。**

使用上下文的另一个好处是。程序中的上下文是通过上下文传递键值的能力。以下方法可用于通过上下文传递和检索值。

```
// Add value to context
context.WithValue(ctx context.Context, key string, value interface) // Get the value from context by key
context.Value(key string) interface {}
```

> 将所有数据放在一个上下文中并跨函数使用，而不是传入参数，这看起来很诱人，但这可能会导致代码可读性下降、不必要的依赖以及代码库维护方面的挑战。

**何时通过上下文传递值。**

只有在需要传播时，您才应该绝对地跨程序层通过上下文传递值。

在上面的代码示例中，在中间件函数中使用它来传递`auth`或相关信息。同样，当你测试程序并在函数间传递`traceId`或上下文信息(主要是元信息)时。

# 结论

Golang `context` package 是设计程序的一个有价值的工具集。使用`context.TODO()`和`context.Background()`来创建空的上下文，并通过用`WithTimeouts`、`WithDeadline`或`WithCancel`来控制程序的流程，使其更加强大。

整个上下文包是这样包装的，

*   在整个呼叫链中存储和传递信息的能力。
*   控制取消的能力，如果任何事情超出或出乎意料。

如果您是新手，或者对上下文的工作方式感到困惑，我认为这篇文章比以前更加清晰。

谢了。