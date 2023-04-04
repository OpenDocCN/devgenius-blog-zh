# 一个关于后期初始化选项的奇怪故事——第一部分

> 原文：<https://blog.devgenius.io/a-curious-tale-of-late-initialisation-options-part-i-44da6ca94068?source=collection_archive---------15----------------------->

## 使用 Kotlin 的后期初始化选项导航内存泄漏和线程安全

![](img/cbf353f396d9cf57eaaf44c2014df24c.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上 [Towfiqu barbhuiya](https://unsplash.com/@towfiqu999999?utm_source=medium&utm_medium=referral) 拍摄的照片

我真的很喜欢科特林中的修饰语。这是一个很小的词，却给我们提供了如此多的方便。简而言之，`lateinit`修饰符使我们能够告诉 Kotlin 一个属性将在以后被初始化。因此，在利用这种属性时，我们不需要担心空检查。

` ＄{ item . name } '在第 20 行被直接访问，而不检查` item '是否为空。

多方便啊？

但是等等，有个小问题。我想清除`onDetach()`方法中的`item`属性。我可以简单地在这个方法上添加一个`this.item = null`行来实现这个，对吗？不对！Kotlin 编译器不允许这样做。您将得到类似于以下内容的错误:

```
Null can not be a value of a non-null type Item
```

使用`lateinit`修饰符编写这段代码会导致内存泄漏，因为无论何时调用`onDetach()`都不会清除`item`属性。

## 捕捉:依赖注入

我会让事情变得更复杂。让我们假设我们已经在我们的项目中设置了 DI，并且我们想要使用一个`DefaultBehaviour`的**单例**实例，以便每当我需要一个`Behaviour`时总是被注入。为了简洁，我将使用这个准系统`DependencyInjection`对象作为我的 DI 框架。

对“DependencyInjection.behavior”的每次调用都将返回相同的实例。

您会注意到，在长时间运行的应用程序中，`DefaultBehaviour`中的`item`会在第一个`onAttach(item)`被调用后保留在内存中，即使在`onDetach()`被调用后也不会被释放。

## 解决方案 1:自定义延迟初始化

我真的希望能够使用`item`属性而不检查它是否是`null`。也许有一种方法我可以欺骗 Kotlin 编译器并模仿一个`lazy-lateinit`？下面是更新后的代码:

这么🎉。我也在清理第 12 行的“项目”。第 13 行将为我证实这一点。

在这种方法中，我创建了一个可空属性`_item`，它保存在`onAttach(item)`方法中接收到的`Item`。然后，我将我的`item`属性设为非空的**`val`，并使用可空的`_item`进行惰性初始化。**

```
private val item by *lazy* **{** *requireNotNull*(_item) **}**
```

**如果我不使`item`成为[惰性属性](https://kotlinlang.org/docs/delegated-properties.html#lazy-properties)，每当创建这个对象时，我的应用程序就会崩溃。原因是，我还没有调用`onAttach(item)`方法，所以后台属性`_item`还没有设置。**

**`by lazy {...}`延迟了`item`属性的初始化，直到我第一次访问它，希望那时我已经调用了初始化后台`_item`属性的`onAttach(item)`方法。另外，`item`属性实际上是非空的，因为`requireNotNull(_item)`将总是返回一个非空值。**

**让我们运行程序:**

**手指交叉…🤞🏾**

**当它编译的时候，我已经知道我期望看到什么了。控制台输出应在第一行显示“正在连接，机器人手臂”,然后在第二行显示“正在分离 null ”,因为支持属性`_item`已被设置为 null。让我们看看输出结果是什么。**

```
Attaching, Robot arm
Detaching Item(name=Robot arm)
```

**等等。什么？**

**为什么不是`null`？我将后台属性`_item`设置为`null`。`item`不也应该有效地变成 null 吗？**

**不，文件上说:**

> **…对 get()的第一次调用执行传递给 lazy()的 lambda 并记住结果。对 get()的后续调用只是返回记忆的结果。— [懒惰属性](https://kotlinlang.org/docs/delegated-properties.html#lazy-properties)。**

**啊，它记得结果！显然，这次“阅读文件”的人是对的。**

**所以我们又回到了起点。我们的 DI 图保存了我们的单例实例，该实例保存了延迟初始化的`item`，在第一个`onAttach(item)`被调用后，它将永远保留在内存中。**

## **解决方案二:接受失败，使用可选属性**

**我想我必须接受`item`必须是可选属性，我才能正确释放该属性。以下是我更新的代码:**

**“item”属性现在可为空。这意味着我需要在访问第 16 行的‘item . name’时使用安全调用。**

**在我们投入精力庆祝找到解决方案之前，让我们先快速地看看结果。祈求好运🤞🏾。**

```
Attaching, Robot arm
Detaching null
```

**哇，这就是我们想要的。`onDetach()`方法现在工作正常🎉。**

**唯一让我恼火的是，每当我想访问项目名称时，我总是不得不使用安全调用`item?.name`。我想知道为什么我需要这样做，当我确信只要我调用了`onAttach(Item)`，那么`item`的值就永远不会是`null`。**

**但这总是真的吗？🤔。**

**让我们把事情复杂化一点。我们将在[第二部](https://bendaniel10.medium.com/a-curious-tale-of-late-initialisation-options-part-ii-339a85e08961)继续。**