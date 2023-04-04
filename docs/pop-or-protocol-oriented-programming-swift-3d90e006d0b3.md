# 面向协议的编程

> 原文：<https://blog.devgenius.io/pop-or-protocol-oriented-programming-swift-3d90e006d0b3?source=collection_archive---------0----------------------->

## POP 比 OOP 好吗？

## Swift 中的 POP 介绍

![](img/680541f245f8521394fcfbd33eff2634.png)

谢尔盖·佐尔金在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Protocol 是 Swift 的一个基本特性。面向协议的编程是一种以完全不同的方式编写代码的强大方法。

协议有很多特性，但这次我们不会深入探讨，假设您已经了解了协议的基础和优点:

在本文中，我们将深入讨论面向协议的编程，我们将确定 Swift 为何面向协议，列举 POP 和 OOP(面向对象编程)两种范式的优缺点，最后我们将确定使用哪一种。

# 什么是面向协议编程或 POP？

顾名思义，我们可能会错误地认为面向协议的编程就是关于协议的。实际上，这不仅是编写应用程序的不同方式，也是我们思考编程的不同方式。是一种不同的编程范式，它改进了我们在应用程序中实现代码的方式，并解决了与面向对象编程相关的问题。

# **为什么要在 Swift 中实施面向协议的编程？**

在其他一些语言中，如 C++、C #或其他具有 objective-c 协议的语言，这些接口和协议并不实现它们协议中的任何东西，这些协议只是作为一个蓝图来指定一个类应该采用的需求。相反，Swift 协议有一个规则改变者特性:*协议扩展。*

我们还可以注意到，Swift 标准库(例如，看看 Swift 的数组定义，它继承了 11 个协议)也是使用协议实现的。即使我们知道一些苹果框架仍然是用 Objective-C、C++或 C 编写的，我们也可以注意到一些新的框架，如 SwiftUI 或其他如 GameplayKit，是由协议推动的。除此之外，您还可以扩展 Swift 标准库协议。

此外，Swift 著名的委托模式使用协议作为完整的资源。

另一方面，由于 Swift 推动我们使用更多值类型而不是引用类型来构建我们的应用程序，这些值类型(如 structs 和 enums)可以符合协议，但不支持继承，因此协议得到了增强。除此之外，协议在 Swift 中作为类型工作，并支持多态性。

POP 的另一个优点是对象可以符合 Swift 中的多种协议。在 OOP 中，对象只能从一个超类继承。这就引出了一个著名的问题:大型单片类(Massive View Controller 你在吗？).使用组合将其分解成小组件，使代码更容易重用。

此外，面向协议的编程利用强大的协议来围绕它构建我们的应用程序架构。这一点以及我们必须编写单元测试的事实使我们利用这一范例来避免类之间的紧密耦合，并鼓励使用依赖注入。

此外，使用 POP 实现您的应用程序可以让整个团队受益于更容易的重构和维护过程。你可能会问为什么？答案是[坚实的](https://medium.com/better-programming/solid-swift-by-examples-part-one-35018d53d3e6)和良好的实践。SOLID 引导我们开始使用 POP 范式以不同的方式编写代码，SOLID 是为使用 POP 而设计的。同样，你应该利用协议来编写小函数、类、结构和代码文件，将相关的属性和函数分组到协议、结构、枚举和类中，并使用有意义的变量名。即使我们使用 OOP 或其他范例，我们也应该始终使用软件开发的基本良好实践。

在这里，你有一大堆理由开始审视流行范例。当然，POP 的关键是利用协议的特性，如协议继承、组合和扩展。

# 如何使用

如果你来自 OOP，继承可能是你开始编码时首先想到的事情之一。所以现在，与其这样，不如试着像对待基类一样，先考虑协议。然后，您可以通过继承初始协议来创建新的协议，并编写扩展，以便它们具有默认的实现。

这就是这个范例的工作方式，我们应该编写小的特定协议，然后向这些协议扩展添加默认实现和新功能。

下面是实现 POP 必须了解的协议的更深层次的观点:

[](https://medium.com/better-programming/protocols-from-zero-to-hero-72423dfdfe21) [## 如何掌握 Swift 中的协议

### 协议定义、继承、扩展等等

medium.com](https://medium.com/better-programming/protocols-from-zero-to-hero-72423dfdfe21) 

# 协议性能

乍一看，使用 POP 可能会降低应用程序在构建时间和静态分派的其他好处方面的性能，但这种范式(POP)让我们使用结构而不是类(值类型而不是引用类型)来平衡和倾斜性能，使性能更好。

无论如何，为了实现这个目标，你可以将这种炫耀与其他实践结合起来以提高性能，看看苹果的博客吧。

[](https://developer.apple.com/swift/blog/?id=27) [## 通过减少动态调度提高性能- Swift 博客

### 像许多其他语言一样，Swift 允许一个类覆盖在其超类中声明的方法和属性。这个…

developer.apple.com](https://developer.apple.com/swift/blog/?id=27) 

关于性能的最后一句话，您可以使用 POP 来提高性能，但是不要把这个范例当作灵丹妙药，您需要继续遵循所有的性能提示和技巧来达到我们的性能目标。

# 面向对象的常见问题

在 OOP 中，继承本身并不灵活，因为只允许一个超类，这使得这个超类太大，并且可能有太多的实例共享对该对象的引用。在 OOP 中，你必须马上选择一个超类，没有其他的建模选择。

如果超类有存储的属性，子类必须接受它们，增加了初始化的负担。

超类必须在方法体中有一些东西，它不能是一个蓝图(例如，如果你的 func 不是 Void，你必须返回一个值，在其他情况下，你必须将体设置为空或调用 fatalError)。

最后，您必须使用 final 关键字来避免函数被覆盖

# 所以 POP 还是 OOP

我们已经列出了两种范式之间的一些差异。我们可能认为面向协议的编程明显优于面向对象的编程，但是我们应该停止考虑它们之间的区别以及哪一个更好，并开始使用两种范例中最好的。POP 和 OOP 并不互相排斥，它们可以互相补充。

# 结论

我们已经读到了许多从 OOP 转向 POP 的好理由，但是，和往常一样，我们不应该仅仅因为我们读到了它或者它变得流行就转向一个新的范例、方法或者框架。我们应该像专业人士一样，选择正确的武器杀死我们的怪物。

总之，让我们从协议开始，但是不要忘记基类或超类的好处。POP 功能强大但不完美，记住 OOP 和 POP 可以互相补充。

# **参考书目**

1.  WWDC 2015 关于“[Swift 中面向协议的编程”的演讲](https://developer.apple.com/videos/play/wwdc2015/408/)
2.  WWDC 2016 关于“[协议和 UIKit 应用](https://developer.apple.com/videos/play/wwdc2016/419/)中面向价值的编程”的演讲
3.  Swift 标准库[http://swiftdoc.org](http://swiftdoc.org)
4.  Swift 标准库阵列定义[http://swiftdoc.org/v3.1/type/Array/](http://swiftdoc.org/v3.1/type/Array/)
5.  协议功能[https://medium.com/@piero9212/protocol-masters](https://medium.com/@piero9212/protocol-masters)
6.  SOLID Swift[https://medium . com/better-programming/SOLID-Swift-by-examples-part-one-35018d 53d 3 e 6](https://medium.com/better-programming/solid-swift-by-examples-part-one-35018d53d3e6)
7.  SOLID Swift[https://medium . com/better-programming/SOLID-Swift-by-examples-part-two-82 AC 3c 457 e4e](https://medium.com/better-programming/solid-swift-by-examples-part-two-82ac3c457e4e)
8.  实 Swift[https://medium . com/better-programming/SOLID-Swift-by-examples-part-three-675672 C1 EC 20](https://medium.com/better-programming/solid-swift-by-examples-part-three-675672c1ec20)
9.  实 Swift[https://medium . com/better-programming/SOLID-Swift-by-examples-part-four-EC 31 BDB 2872](https://medium.com/better-programming/solid-swift-by-examples-part-four-ec31bdb2872)
10.  SOLID Swift[https://medium . com/better-programming/SOLID-Swift-by-examples-part-five-a860 b 86 b 85 f 5](https://medium.com/better-programming/solid-swift-by-examples-part-five-a860b86b85f5)
11.  面向协议编程乔恩霍夫曼电子书