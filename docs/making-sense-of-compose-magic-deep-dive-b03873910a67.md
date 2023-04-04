# 理解作曲魔术(深入探讨)

> 原文：<https://blog.devgenius.io/making-sense-of-compose-magic-deep-dive-b03873910a67?source=collection_archive---------5----------------------->

![](img/9a67b6bce4572ff8fb136294583c5b07.png)

如果您更改一个状态值，compose 如何自动更新 UI 几乎是不可思议的，但是它是如何做到的呢？让我们找出答案。

# 想法

在 jetpack compose 中，如果你想让一些值存活下来 [**重组**](https://developer.android.com/jetpack/compose/mental-model#recomposition) 你就把它传入，记住在`mutableStateOf()`中换行

`ex — val enabled = remember{ mutableStateOf(false)}`但是它是如何工作的呢？

***让我们深潜***

如你所见，`mutableStateOf()`定义了两个参数，第一个是`value:T`，它是它所保存的对象，第二个是`policy:SnapShotMutationPolicy` ，我们通常不关心它，因为它有一个默认值，但我们一会儿会深入讨论它。

合成运行时跟踪当前合成中的`**MutableState**` 对象，这意味着如果您更改由`**MutableState**`保存的值，合成运行时将注意到该更改，并调度一个 [**重组**](https://developer.android.com/jetpack/compose/mental-model#recomposition) ，这是第二个参数`policy`出现的地方，合成使用它来检查值是否更改。

***让我们深潜***

如你所见`stucuralEqualityPolicy()`返回`SnapshotMutationPolicy<T>`它有一个`equivalent()`方法，该方法接收值`a`和`b`，其中一个是我们正在设置的新值，另一个是由`MutableState<T>`保存的旧值，这些值有望由 Compose 运行时提供。

`equivalent()`仅检查`a == b`是否`true`编写将调度重组，否则不调度。这也意味着你的**数据类型**的`equals()`方法应该是一致的(*如果你使用数据类，这不是一个问题，虽然*)如果出于某种原因，你没有使用**数据类**，你可以通过实现那个接口覆盖 equals 或者提供你自己的`SanpShotMutationPolicy`。

这是关于重组，但只有重组不起作用，改变`State<T>`对象导致重组，但没有提供新值，我们的`State<T>`将再次初始化为旧值，就像什么都没发生一样。

# 让我们记住一些事情

原来**组件**可以有自己的内存，所以在重新编译时，它们可以选择保存在内存中的旧值并使用它。

要让 compose 跟踪您的值并保存在 **composables** 内存中，您需要使用`State<T>`和`remember{}`。

`ex — val enabled = remember{ mutableStateOf(false)}`

***让我们深潜***

`remember()`将计算作为 lambda，然后使用`currentComposer`引用 Composer 的实例。这个接口是 Compose Kotlin 编译器插件的目标，由代码生成助手使用。您不应该直接调用它，因为运行库假定调用是由编译器生成的，因此不包含太多的验证逻辑。`cache()`是 Composer 的扩展功能。它在组合的组合数据中存储一个值。

既然这就是记忆值的来源，`rememberedValue()`函数返回一个保存在 composable 上的缓存中的值，这里它们主要检查我们是否已经有了那个值，如果没有，那么只进行计算，值得到更新，否则返回旧值。

**更多好奇**

想多深潜？阅读以下文章

[](https://medium.com/androiddevelopers/understanding-jetpack-compose-part-1-of-2-ca316fe39050) [## 了解 Jetpack 撰写—第 1 部分，共 2 部分

### 使用 Compose 构建更好的 UI

medium.com](https://medium.com/androiddevelopers/understanding-jetpack-compose-part-1-of-2-ca316fe39050) [](https://medium.com/androiddevelopers/under-the-hood-of-jetpack-compose-part-2-of-2-37b2c20c6cdd) [## 在 Jetpack Compose 的引擎盖下—第 2 部分，共 2 部分

### 在作曲的引擎盖下

medium.com](https://medium.com/androiddevelopers/under-the-hood-of-jetpack-compose-part-2-of-2-37b2c20c6cdd) 

**谢谢**