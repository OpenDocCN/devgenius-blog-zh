# 学习 RxSwift(第二部分)

> 原文：<https://blog.devgenius.io/learn-rxswift-part2-3d99583417bf?source=collection_archive---------1----------------------->

![](img/73d3a8be2b461acd3402a22803009908.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Rich Tervet](https://unsplash.com/@richtervet?utm_source=medium&utm_medium=referral) 拍摄的照片

在许多软件中，编程任务会按照你编写的顺序，一次一个地执行和完成。但是在 ReactiveX 中，许多指令可以并行执行，它们的结果稍后会被**观察者**以任意顺序捕获。

## **建立观察员:**

在异步模型中，流程更像这样:

1-定义一个方法，用异步调用的返回值做一些有用的事情(这个方法是 observer 的一部分)。
2-将异步调用本身定义为一个可观察对象。通过订阅将观察者附加到可观察对象上。继续你的生意；每当调用返回时，观察者的方法将开始操作它的返回值——由可观察对象发出的项目。

订阅方法是你如何连接一个观察者和一个被观察对象。您的观察器实现了以下方法的一部分:

**onNext** :
每当一个可观察对象发出一个项目，这个可观察对象就调用这个方法。

**onError** :
一个 Observable 调用这个方法来表示它没有生成预期的数据或者遇到了一些其他的错误。它不会进一步调用 onNext 或 onCompleted。

**onCompleted** :
如果没有遇到任何错误，Observable 在最后一次调用 onNext 后调用该方法

**取消订阅** :
在一些 ReactiveX 实现中，有一个专门的观察者接口 Subscriber，它实现了一个取消订阅的方法。您可以调用此方法来指示订阅者对其当前订阅的任何观察对象不再感兴趣

## 那么如何创造可观察的:

比方说，我们需要在后台执行一些工作，因此我们有许多选项，如 **GCD** 、**线程**或**操作队列**，但这里我们将创建**可观察对象**来执行我们的逻辑，并在逻辑完成时订阅，然后我们可以使用返回的数据，实际上有许多选项来创建可观察对象，让我们看看:

**1- Create:** 它通过编程调用 observer 方法从头开始创建一个可观察对象。

```
**Current emitted number:  1****Current emitted number:  2****Current emitted number:  3****Current emitted number:  4****Current emitted number:  5****Complete**
```

**1-** 用` Create '函数创建可观察对象
2-在闭包中创建函数返回可观察对象，然后在那里做逻辑，像 for 循环然后调用`( on。next(value))`当逻辑完成时发出逻辑
**3-** ，然后调用` . on(。已完成)`完成它。
**4-** 要监听发出的值，可以订阅(onNext、onError、onComplete 和 onDisposed)。

**2- Deferred:**
直到观察者订阅才创建可观察对象，并为每个观察者创建一个新的可观察对象。

```
**Current emitted number:  1****Current emitted number:  2****Current emitted number:  3****Current emitted number:  4****Current emitted number:  5****Complete**
```

**3- From:**
它将一些其他的对象或数据结构转换成可观察的。

```
**Current emitted number:  1****Current emitted number:  2****Current emitted number:  3****Current emitted number:  4****Current emitted number:  6****Current emitted number:  7****Complete**
```

**4-Interval:**
它创建一个可观测值，发出一个由特定时间间隔隔开的整数序列。

```
**Second:  0****Second:  1****Second:  2****Complete**
```

**5-Just:**
它把一个物体或一组物体转换成一个可观察的，发出那个或那些物体的东西。

```
**Current emitted number:  1****Complete**
```

**6-Range**
它创建一个发出一系列连续整数的可观察对象。

```
**Current emitted number:  3****Current emitted number:  4****Current emitted number:  5****Complete**
```

**7-RepeatElement:**
它创建一个可观察对象，重复发出一个特定的项目或项目序列。

```
**Current emitted number:  12****Current emitted number:  12****Complete**
```

**8 定时器:** 它创建一个在给定延迟后发射单个项目的可观察对象。

在这篇文章中谈到了如何创建 RxSwift 可观察。未完待续其他部分……
rx swift[(part 1)](https://medium.com/@deda9/learn-rxswift-part-1-d69ed8461e8f)

[RxSwift](https://github.com/ReactiveX/RxSwift)

[无功编程](https://en.wikipedia.org/wiki/Reactive_programming)

[react vex](http://reactivex.io/)