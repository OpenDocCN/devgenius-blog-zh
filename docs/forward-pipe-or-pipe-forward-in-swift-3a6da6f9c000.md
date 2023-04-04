# Swift 自定义操作符—转发管道或管道转发

> 原文：<https://blog.devgenius.io/forward-pipe-or-pipe-forward-in-swift-3a6da6f9c000?source=collection_archive---------8----------------------->

有时在 swift 中使用自定义操作符是可以的，如果它易于理解并简化了您的代码😉

![](img/569c059ad880e1e7dcaf866fbd27f653.png)

塞缪尔·西亚尼帕尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

让我们先了解一下为什么、做什么以及如何使用这个前向管道操作符

**【为什么】我们需要它** 🧐 **:** 在 swift 中编写您的应用程序时，您可能会遇到接受输入参数并返回某些内容的函数，这些返回值被其他函数使用，等等。

在下面的函数调用中，乘法函数调用出现在 add 函数调用之前，但实际上 add 函数首先执行，因为**组合函数由内向外执行**。在这种情况下，前向管道操作符就很方便了，它将提高代码的可读性。

```
multiply(val: add(val: 2))
```

**“什么”是前进管**🤷 **:** 前向管道运算符是一个**中缀运算符**大多出现在函数式编程语言中，如 **F#** 和 **Elixir** 。用`|>`符号表示。该运算符从左到右工作，即该运算符从左侧获取值，并将其作为输入传递给右侧的函数，如下所示。

```
2 |> add |> multiply
```

**“如何”实现快速转发管道**👨‍💻 **:** 要在 swift 中创建新操作员，您必须首先定义如下所示的`precedancegroup`。

```
**precedencegroup** ForwardPipe {
    **associativity**: **left**
}
```

结合性定义了属于同一优先组的运算符如何组合在一起，即从左还是从右。换句话说，我们希望操作符在处理**前向管道**时，隐式地对其左侧**的值进行分组。您也可以使用`higherThan`或`lowerThan`属性指定一个操作员相对于其他操作员的优先级。**

> **注意:**如果你创建一个没有任何优先组的操作符，那么它属于没有结合性的`DefaultPrecedance`优先组类别。

要了解更多关于优先级和结合性的信息，你可以参考苹果的官方文件[这个](https://developer.apple.com/documentation/swift/swift_standard_library/operator_declarations)和[这个](https://docs.swift.org/swift-book/LanguageGuide/AdvancedOperators.html#//apple_ref/doc/uid/TP40014097-CH27-ID41)

那很酷，但是你的接线员在哪里？🤔🤔 🤔

好吧。好吧，我们还没有声明一个。让我们继续创建我们的自定义**中缀**操作符，它将由符号`|>`表示。

```
**infix** **operator** |> : ForwardPipe
```

这样操作符声明就完成了。是的，你没听错😁

但是 wait✋.我们现在必须创建一个函数来实现我们的操作符的功能。

```
**func** |> <T, U>(value: T, function: ((T) -> U)) -> U {
    **return** function(value)
}
```

上面的函数名为`|>`，带有两个通用类型参数`T`和`U`以及两个参数。`T`是输入值，该值进一步馈入`function`，T5 接受`T`并返回`U`。整个函数返回值`U`，因为这是`function`返回的值。

下面是完整的代码片段和示例。🤩🤩🤩🤩

这就是斯威夫特的前进管道。🥳

# 结论:

在本文中，我们了解了如何在 swift 中创建自定义操作员。我们创建了正向管道操作符，并了解了如何使用它。

享受…..还有快乐编码…🙂

# *延伸阅读:*

[](http://undefinedvalue.com/fs-pipe-forward-operator-swift.html) [## 未定义的值

### 注意:在 2015 年 WWDC 大会上，Apple 发布了 Swift 2，其中包括一些变化和一个名为“协议扩展”的新功能，该功能…

undefinedvalue.com](http://undefinedvalue.com/fs-pipe-forward-operator-swift.html)