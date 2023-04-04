# 我在 Datadog 的第一周学到了什么

> 原文：<https://blog.devgenius.io/what-i-learned-about-performance-optimization-during-my-first-week-at-datadog-88a90bafa66e?source=collection_archive---------3----------------------->

## 代码分析中的 4 个重要指标

在过去的一个月里，我有幸不仅搬到了我最喜欢的城市巴黎，还加入了我梦想中的公司，成为了一名高级软件工程师。

他们说第一印象很重要，我在 Datadog 的头几周几乎没有什么可以改善的。我加入了剖析团队，在公司的[连续剖析器](https://www.datadoghq.com/product/code-profiling/)上工作。这只是我在 Datadog 的第四周，但我已经学到了很多！

以前在 Slack，我是他们前端观察团队的创始成员。我学到了很多关于分布式跟踪和[如何利用跟踪来实现可观察性](https://towardsdatascience.com/how-i-used-tracing-and-data-science-to-learn-redux-correlates-with-terrible-app-performance-4fe976ef8a06)。然而，描摹只能给你提供一半的情况。通过跟踪，您可以获得应用程序性能的高级视图。例如，您可以跟踪从一个页面到另一个页面需要多长时间，并根据前端时间、网络时间和后端时间来分解这个转换。这使您能够轻松识别性能瓶颈是由前端、网络还是后端问题引起的。跟踪不会告诉您有问题的方法或代码行(除非您通过自定义跨度明确地度量这些方法或代码行)。这就是侧写的用武之地。

在加入 Datadog 之前，我在性能优化方面的知识主要局限于前端。虽然确实有很多方法可以优化前端性能——延迟加载代码、记忆道具、重写组件逻辑以限制渲染——后端也需要很快才能获得良好的用户体验。成为剖析团队的一员为优化后端代码的重要性打开了一个全新的世界。

后端分析有 4 个关注点:

1.  CPU 性能分析—CPU 内核在特定时间段内(例如，1 分钟内)完成的工作量。
2.  内存分析——分配了多少堆，分配了多少活动对象。
3.  锁分析——持有锁或等待释放锁所花费的时间。
4.  I/O 分析—从网络(套接字 I/O)和磁盘(文件 I/O)读取和写入所花费的时间。

Flamegraph 是一种流行的可视化策略，用于显示分析信息。下面的火焰图显示了完成一个请求时代码在 CPU 利用率上花费的时间。

![](img/2879c3cb77f15f89d86d217ea033be21.png)

[火焰图——CPU 时间超过约 1 分钟](https://www.datadoghq.com/knowledge-center/distributed-tracing/flame-graph/)

当查看概要文件时，这些指标中的每一个都可以通过方法来分解。因此，您可以看到基于方法的 CPU 时间、内存分配、锁压力和 I/O 时间。 [Datadog Profiling](https://docs.datadoghq.com/getting_started/profiler/) 工具可以根据包/库、类、代码行以及某些语言的线程来分解这些指标。

![](img/e6ee5846c75bb53c8ff3bb8f2e644d90.png)

[Profiler doc](https://docs.datadoghq.com/getting_started/profiler/) —按方法+行划分的 CPU 时间

CPU 性能分析本身可以进一步分为几个感兴趣的领域。它们包括但不限于:

1.  编译正则表达式完成了多少工作。
2.  垃圾收集完成了多少。
3.  在原始值装箱上做了多少工作。
4.  锁争用阻塞了多少 CPU 时间。

这 4 个性能关注领域中的每一个都可以通过少量的代码更改来优化。例如，通过将代码改为只编译表达式一次，通常可以减少正则表达式编译的 CPU 时间。

在 Python 中，此代码将导致重复编译相同的正则表达式:

```
for s in strings:
    re.match("[a-z]", s)
```

虽然这个版本将避免这种不必要的工作:

```
pattern = re.compile(“[a-z]”)
for s in strings:
    pattern.match(s)
```

等待锁会导致应用程序中的高延迟。通过使用更细粒度的锁，或者更改代码逻辑以在更短的时间内持有锁，可以缓解高锁压力。

原始值装箱特定于一些语言，如 Java。Java 具有自动装箱功能，可以将原语(如`int`)转换成相应的包装类(如`Integer`)。原语到对象的转换会严重影响性能。我发现[这篇文章](https://www.theserverside.com/blog/Coffee-Talk-Java-News-Stories-and-Opinions/Performance-cost-of-Java-autoboxing-and-unboxing-of-primitive-types)有助于理解原始值装箱的基础知识。

通过减少应用程序中的堆分配数量，可以减少垃圾收集对 CPU 的占用。在 Python 中，一个优化是用`.format()`替换用于字符串连接的`"+="`。这是因为在 Python 中，字符串是不可变的。每次向字符串添加元素时，Python 都会创建一个新的字符串，并将其分配到一个新的地址。

跟踪与分析相结合是一个非常强大的性能观察工具。

如果您通过一些`trace_id`将您的跟踪链接到您的概要文件，您可以直接从跟踪的跨度信息移动到概要分析数据。这让您可以找到与性能问题相关的特定代码行，而不是猜测问题来自哪里。[更多信息](https://docs.datadoghq.com/tracing/profiler/connect_traces_and_profiles/)关于如何用 Datadog 实现这一点。

优化 CPU 和内存不仅对用户有好处，对公司也有好处。如果您的服务是基于 CPU 或内存消耗来定价的，那么降低它们就意味着降低成本！