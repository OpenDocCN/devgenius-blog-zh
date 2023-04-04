# 一个关于后期初始化选项的奇怪故事——第三部分

> 原文：<https://blog.devgenius.io/a-curious-tale-of-late-initialisation-options-part-iii-e92d9f2abd79?source=collection_archive---------19----------------------->

## 使用 Kotlin 的后期初始化选项导航内存泄漏和线程安全

![](img/cc1286ae303f15eb0e46345d1112388a.png)

照片由 [GuerrillaBuzz Crypto PR](http://guerrillabuzz.com/) 在 [Unsplash](https://unsplash.com/@theshubhamdhage?utm_source=medium&utm_medium=referral)

在本系列的最后一部分，我将重构`Behaviour`接口及其实现，并将更改和错误修复合并到我们在第二部分中编写的[代码中。](https://bendaniel10.medium.com/a-curious-tale-of-late-initialisation-options-part-ii-339a85e08961)

## 解决方案:API 返工、错误修复

重要的事情先来。让我们更新一下`Behaviour`接口，并添加分离单个`Item`的功能。

实现也将被更新，以从项目中删除行为。但在此之前，让我们重新思考我们的方法。

本系列从找出最安全和最不痛苦的方法开始，将在`onAttach(item)`方法中收到的`item`保留在`Behaviour`的单例实例上。我们选择将该属性保留为可空属性，并在分离行为时清除它。

如果我们不需要来保留`item`作为一个属性呢？我们是否可以将收到的`item`下行传递给任何需要知道它在做什么的方法？我想这个会有用的。

其次，我们需要将一个附加的`item`关联到具体的`apiCallThread`。这样我们可以确保无论何时`Behaviour`从`item`上拆下，正确的`apiCallThread`都会被中断。我认为我们可以使用一个`ConcurrentHashMap<Int, Thread>()`来处理这个映射；这个 Map 实现支持并发的写和读。

这是更新的解决方案:

现在我们唯一的财产就是`apiCallThreadsMap`。这是帮助我们将附加的`item`关联到特定的`apiCallThread`的图。

请注意，`item`正在向下游传递给任何需要它的人。这反映在新的`echo(item)`方法签名和`then: (item: Item) -> Unit` lamda 中。这样做的一个好处是，我们不需要以安全的方式访问`item.name`，因为它不再是可空类型。

让我们使用破坏代码的最后一个片段来测试更新后的代码。

机械臂故意不分离，进一步阅读:)

这是输出:

```
Calling API for Robot arm from Thread-0
Calling API for Robot face from Thread-1
Detaching Item(name=Robot face) from main, interrupted Thread-1
Interrupted, fun ...echo(Item)... not called from Thread-1
Attaching, Robot arm from Thread-0
```

忠太🎉。现在效果很好。开发人员可以随时附加和分离单个项目。此外，不会覆盖任何线程。

让我们用 3 个项目再次尝试代码，并在不同的时间间隔分离所有项目。

现在，我们甚至在拆下之前连接的一个`item`后再连接一个。代码应该运行良好(剧透，确实如此)。

```
Calling API for Robot arm from Thread-0
Calling API for Robot face from Thread-1
Detaching Item(name=Robot face) from main, interrupted Thread-1
Detaching Item(name=Robot arm) from main, interrupted Thread-0
Detaching Item(name=Robot wing) from main, interrupted Thread-2
Calling API for Robot wing from Thread-2
Interrupted, fun ...echo(Item)... not called from Thread-0
Interrupted, fun ...echo(Item)... not called from Thread-1
Interrupted, fun ...echo(Item)... not called from Thread-2
```

咻。

我发现在设计 API 时应该考虑多少事情是很有趣的。我用了一个简单的例子来说明这一点，我们已经看到了为了适应许多重要的情况，代码被调整得多么健壮。

我从这次经历中得到的主要收获是:

*   仅在绝对必要时使用属性
*   任何形式的并发都可能暴露一段代码中丑陋的部分，在编写任何 API 时要记住这一点
*   不要忘记清理资源。了解您在内部使用的 API 的生命周期，并在将它们设置为 null 之前清理它们的实例
*   阅读文档

谢谢你跟随我经历这个思考过程。我真的希望你能从中吸取一些东西。

✌之二🏽。