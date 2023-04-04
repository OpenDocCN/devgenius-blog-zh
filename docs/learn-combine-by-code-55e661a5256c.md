# 通过代码系列学习合并(第 1 部分)

> 原文：<https://blog.devgenius.io/learn-combine-by-code-55e661a5256c?source=collection_archive---------3----------------------->

![](img/67f97c1a030a690ff0baf2ab0409de48.png)

由 [Max Duzij](https://unsplash.com/@max_duz?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 联合收割机是什么？

Combine 框架提供了一个声明性的 Swift API，用于随时处理值。这些值可以表示多种异步事件。因此，在 ***命令式*** 编程设置`**a = b+c**`中，将意味着`**a**`被赋值为**`**b+c**`**和更高版本的结果，可以更改`**b**` 和`**c**` 的值，而对`**a**`的值没有影响，但是在 ***无功*** 编程中，每当 而无需程序重新执行语句`**a:=b+c**` 来确定`**a**` **的当前赋值。** [维基百科](https://en.wikipedia.org/wiki/Reactive_programming)****

****为了更好地理解组合，我们必须理解 3 个主要概念:****

*   ****发布者和订阅者****
*   ****经营者****
*   ****学科****

# ****1-出版商是什么？****

****它是协议声明的一种类型，可以随时间传递一系列值。没有任何订阅请求的发布者将不提供任何数据。****

****订阅者的`Input`和`Failure`关联类型必须与发布者声明的`Output`和`Failure`类型相匹配。发布者实现`receive(subscriber:)`方法来接受订阅者。****

****发布服务器可以在订阅服务器上调用以下方法:****

*   ****`receive(subscription:)`:确认订阅请求并返回一个`Subscription`实例。订阅者使用订阅向发布者要求元素，并可以使用它来取消发布。****
*   ****`receive(_:)`:将一个元素从发布者传递给订阅者。****
*   ****`receive(completion:)`:通知订阅者发布已经结束，正常或者有错误。****

## ****创建自己的出版商****

****您可以使用 Combine 框架提供的几种类型之一来创建自己的发布者:****

*   ****使用`Subject`的具体子类，比如`PassthroughSubject`，通过调用其`send(_:)`方法按需发布值。****
*   ****每当你更新主题的潜在价值时，使用一个`CurrentValueSubject`来发布。****
*   ****将`@Published`注释添加到您自己类型的属性中。这样，属性获得了一个发布者，每当属性值更改时，该发布者都会发出一个事件。****

# ****2-订户是谁？****

****它负责请求数据并接受发布者提供的数据和可能的失败。有内置的订户来组合:****

*   ****`Assign`将发布者传递的值应用于由“keypath”定义的对象。****

```
****.assign**(to: \**.**isEnabled, on: continueButton)**
```

*   ****`Sink`接受一个闭包，该闭包接收来自发布者的任何结果值。****

```
****.**sink { results **in**
    //Do the logic to the results
}**
```

*   ****`onReceive(publisher)`将视图作为订户使用的功能，它可以在 SwiftUI 中操作`@State`或`@Bindings`。****

```
****struct** **CustomView** : **View** {**@State** **private** **var** currentStatus **=** "enabled"
    **var** body: some **View** {
        **Text**("Current status: \(currentStatus)")
        **.onReceive**(MyPublisher**.**currentStatusPublisher) { 
                **self.**currentStatusValue **=** $0
            }
    }
}**
```

# ****3-运营商是什么？****

****它是预建的功能，包含在 Apple 参考文档的 Publisher 下。像`**map**` **、** `**scan**` 接受一个或多个闭包来定义业务逻辑，还有很多我们将在后面详细检查。****

# ****4-科目是什么？****

****它是一个发布器，通过调用它的`send(_:)`方法，您可以使用它将**值注入到流中。这对于使现有的命令式代码适应组合模型很有用。******

****`CurrentValueSubject`和`PassthroughSubject`也可用于为符合`ObservableObject`的对象创建发布者。SwiftUI 中的许多声明性组件支持该协议。****

*   ****使用一个`CurrentValueSubject`主题，它包装一个值，并通过调用`send(_:)`在值改变时发布一个新元素****
*   ****使用`PassthroughSubject`一个向下游订阅者广播元素的主题，它不像 CurrentValueSubject 那样没有初始值，也没有最近发布元素的缓冲区。****

# ****5-什么是背压？****

****当发布者有很高或无限制的需求时，它发送元素的速度可能会比订阅者处理元素的速度快。这种情况可能会导致元素被丢弃，或者当元素在等待处理时填满缓冲区时，内存压力会迅速增加。背压是控制发布者向订阅者发送元素的速率的概念。当订户接收到元素时，它可以通过向`receive(_:)`返回新的需求值，或者通过在订阅上调用`request(_:)`来请求更多。****

# ****通过用法学习组合****

## ****1-只是****

*   ****当我们只需要向每个订阅者发出一次输出，然后结束。****

```
**ReceiveValue is:  One
ReceiveCompletion is:  finished**
```

## ****2-失败****

*   ****当我们需要终止指定的错误时。****

```
**Error message is: invalid JSON**
```

## ****3-未来****

*   ****当我们需要包装异步逻辑并将其转换成发布器时。比如当我们需要请求用户允许联系时。****

```
**ReceiveValue is: true
ReceiveCompletion is: Finished**
```

## ****4-空的****

*   ****当我们不需要发布任何值时，可以选择立即完成。****

```
**ReceiveCompletion is: Finished**
```

## ****5-延期****

*   ****当我们需要在订阅者订阅发布者之后创建发布者时，这意味着将发布者的设置推迟到实际需要时。当创建 API 以返回发布者时，这是很有用的，因为创建发布者是一项昂贵的工作。****

```
**Do any async call here
ReceiveValue is: true
ReceiveCompletion is: Finished**
```

## ****6- CurrentValueSubject****

*   ****当我们需要包装单个值并在值改变时发布新元素时。****
*   ****当我们需要制作一个随时间发送值但从不完成的发布器时。****

```
**ReceiveValue is: Initial value
ReceiveValue is: First value
ReceiveValue is: Second value**
```

## ****7-通过主题****

*   ****当我们需要向下游用户广播元素时。****
*   ****当我们不需要任何初始值或最近发布的元素的缓冲时。****

```
**ReceiveValue is: First value
ReceiveValue is: Second value**
```

## ****8-全部满足****

*   ****当我们需要对发布者的所有元素进行循环，并返回一个布尔值来指示序列中的每个元素是否满足给定的谓词时。****

```
**Does all satisfy? : true**
```

## ****9-可连接发布者****

*   ****当我们需要在生产任何元素之前执行附加配置或设置时。****
*   ****这个 publisher 不会产生任何元素，直到您调用它的`connect() / autoConnect()`方法。****
*   ****使用`makeConnectable()`从任何失败类型为`Never`的发布者创建一个`ConnectablePublisher`。****

```
**ReceiveValue is: Third value**
```

## ****10-缓冲****

*   ****当我们需要缓冲来自上游发布者的最大数量的元素时。****

```
**value : 98
value : 99
value : 100**
```

## ****11- Catch****

*   ****当我们需要通过用另一个发布者替换失败的发布者来处理来自上游发布者的错误时。****

```
**ReceiveValue is: New Publisher because error invalid JSON
ReceiveCompletion is: Finished**
```

## ****12-收集****

*   ****当我们需要收集( **n)** 的物品一起放入数组中。****

```
**value : [1, 2]
value : [3, 4]
value : [5, 6]**
```

## ****13-按时间收集****

*   ****当我们需要收集( **n)** 中的物品并定期发布它的物品时。****
*   ****在上游发布者提供的缓冲值中指定的间隔期间，time 将使用无限量的内存。****

```
**value : [1, 2]
value : [3, 4]
value : [5, 6]**
```

## ****14-组合测试****

*   ****当我们需要结合两个出版商的最新元素时。****

```
**Value :  ("B", "C")
Value :  ("B", "D")**
```

## ****15-紧凑地图****

*   ****当我们需要重新发布使用每个接收到的元素调用闭包的所有非`nil`结果时。****

```
**Value :  A
Value :  B
Value :  C
Value :  D**
```

****我真的很喜欢反应式编程，喜欢学习它，我已经使用过 RxJava 和 RxSwift，所以如果你已经熟悉它们中的任何一个，学习 Combine 都很容易。希望你和我一样喜欢😃🎉如果是这样，请随意鼓掌，如果你喜欢更多，请随意鼓掌😃与你的朋友分享，与人分享总是令人愉快的。****

****不要忘记检查该系列的其他部分🚀****

*   ****[通过代码系列学习联合收割机(第二部分)](https://medium.com/dev-genius/learn-combine-by-code-series-part-2-aa7083bad99e)****
*   ****[通过代码系列学习联合收割机(第三部分)](https://deda9.medium.com/learn-combine-by-code-series-part-3-c9b857c837eb)****