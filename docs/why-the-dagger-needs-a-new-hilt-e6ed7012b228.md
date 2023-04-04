# 为什么匕首需要新的刀柄

> 原文：<https://blog.devgenius.io/why-the-dagger-needs-a-new-hilt-e6ed7012b228?source=collection_archive---------25----------------------->

![](img/eda313d16383ee2e8dbf59063587d193.png)

在我做程序员的第五年，我终于明白了[依赖注入](https://en.wikipedia.org/wiki/Dependency_injection#:~:text=In%20software%20engineering%2C%20dependency%20injection,object%20is%20called%20a%20service.)是怎么回事，以及我为什么需要它。不久之后，我意识到没有阿迪框架走这条路是不切实际的。我发现了匕首并决定使用它。这是我遵循指南时的最初想法:

> 什么是组件？？！！我只想使用我的模块中的类！

我认为 Dagger 的困惑来源于它除了是阿迪框架之外，还指示了一种额外的架构方法:[基于组件的应用程序开发](https://en.wikipedia.org/wiki/Component-based_software_engineering)。

## 通过现实生活中的例子理解组件

人体是一个复杂的系统。它包含数百万种功能和数万亿个细胞。但是它也可以被描述为具有清晰边界的少量离散的独立器官。由于这一点，医生可以为特定的器官提供正确的治疗，甚至替换它。

![](img/3a8b85a21dac115911ee9f9b71d066e8.png)

这种模式无处不在:大系统被分成许多小的离散部分，每个部分都有自己的功能。

Android 应用程序是一个软件系统。软件能从其类的描述中被理解吗？
当然不是！一个普通的软件项目会有数千个类。我们将不得不使用一个额外的抽象层，它不是 Java 或 Kotlin 的一部分。

## 软件成分

一个[软件组件](https://simplicable.com/new/software-components#:~:text=Software%20components%20are%20parts%20of,interchangeable%20parts%20of%20a%20machine.)是封装在一个接口后面的一堆功能，它创建了一个清晰的边界，定义了组件的内部或外部。
一个组件通常由几个类组成，这些类为了达到某个目的而结合在一起。其中有些类可能是其公共 API 的一部分，有些可能是其实现的内部部分:

在这个特定的例子中:`LocationManager` *、* `LocationListener` *、*和`Location`是由`LocationManager`组件**、**公开的 API，但是在内部使用的实现可能有更多的类。

## 好的。那么匕首和它有什么关系呢？

当你的代码被恰当地组织成组件时——类并不只是漂浮在项目的模块空间中，等待被抓取。阶级根本就不应该是第[第一公民](https://en.wikipedia.org/wiki/First-class_citizen)！它们只是组件的构建模块。

带着这种想法，Dagger 的 API 提供了一种机制来**构建组件而不是类。**

我个人认为，通过组件封装代码是一条可行之路，但是让它成为 DI 框架 API 的一部分无疑会让 Dagger 更难理解。

如果这还不够，既然 Android 不让你实例化`Activity` / `Service` 组件，Dagger 就没法建了。所以对于 android 构造来说，**标注了** `**@Component**` **的接口并不是真正的组件**。这只是在调用`onCreate()` 期间应该注入自身的“真实”组件的内部内容:

## 那么《剑柄》有什么变化？

Google 理解这些概念上的差距，但是想要提供一个向后兼容的解决方案。因此，在 Hilt 中，他们在 Dagger 的 API 之上创建了一个额外的抽象层，由**为您创建这些组件**:

不用创建 Dagger 组件，你只需要扩展`ComponentActivity` *，*添加`@AndroidEntryPoint`，并在模块定义中指定它可以“安装”到哪个组件类型中。

所以最后，组件真的是组件，事情是有意义的。

## 结论

如果你已经在使用匕首——用剑柄代替它不会给你额外的超能力。如果你是 DI 框架的新手——Hilt 可能更容易集成。考虑到剑柄会在匕首之上增加额外的[注释处理，这可能会减慢你的建造时间](https://www.techyourchance.com/dagger-hilt/)。通过理解 Dagger 的“组件”哲学和陷阱，您可以避免它。

如果你喜欢我的解释，那么你也会发现我建议的定义应用组件的方法是有用的。