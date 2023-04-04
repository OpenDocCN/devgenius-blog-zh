# 贫血领域模型反模式

> 原文：<https://blog.devgenius.io/anemic-domain-model-anti-pattern-973de1af0c23?source=collection_archive---------1----------------------->

![](img/2023e1ac2c8cc77465025d4f18343284.png)

# 封装—面向对象

> 在面向对象编程(OOP)中，**封装**指的是将数据与对该数据进行操作的方法捆绑在一起，或者限制对某个对象组件的直接访问

在面向对象和封装之前，对象中的数据通常由其他实体访问。例如，要计算 Kotlin 中 vector 类的大小:

*<我选择 Kotlin 进行演示，因为它的语法不太冗长。然而，这种反模式同样适用于 OOP >* 之后的所有语言

main 函数通过像 getters 和 setters 这样的*访问函数来访问 Vector 类的字段。*

*(在 [Kotlin](https://kotlinlang.org/) ， *vector.x* 中的*调用 *vector* 类的自动生成的 *getX()* 函数，该函数返回 x 的值。

在这种模式下，调用者方法会因逻辑而变得臃肿，而且经常会导致重复，因为每个调用者方法都有自己的逻辑副本。

如果这些方法是用面向对象的原则编写的，它应该是这样的

计算量级的逻辑已被移到 Vector 类中，更接近于数据。所有调用方法需要做的就是调用 *calculateMagnitude* 函数来获得向量的大小。它不需要知道相同的逻辑。此外，由于避免了代码重复，对幅度计算逻辑的任何更改(如向 vector 类添加新维度)都将更容易进行，因为逻辑的代码封装在 *vector* 类中。

# 贫血领域模型反模式

让我们来看看驾照门户的后端代码。门户有一个前端，当用户第一次登录时，它会问候用户。如果客户是成年人(18 岁以上)，它将获取客户的驾照。

通常有一个*服务*类来保存上述业务逻辑，并编排数据库和前端之间的数据。如果遵循贫血域模型，其代码如下

*客户*数据类保存数据，而*客户服务*类拥有业务逻辑。但是很明显，检查客户是否是成年人的逻辑在服务中是“散乱的”,并且被需要它的不同功能所重复。此外，如果有更多的服务类需要检查，这些类中的逻辑也会被复制。

但是，如果将客户特定的逻辑移到客户数据类中，由于代码的模块化，CustomerService 类会更精简。

这将通过将特定于域对象的逻辑限制到相应的类来确保封装，并且服务类的责任也将被限制到业务逻辑的编排。

**参考文献:**

[https://www . infoworld . com/article/2075271/encapsulation-is-not-information-hiding . html](https://www.infoworld.com/article/2075271/encapsulation-is-not-information-hiding.html)

[](https://www.martinfowler.com/bliki/AnemicDomainModel.html) [## bliki: AnemicDomainModel

### 这是一种已经存在了很长时间的反模式，但似乎有了特别的突破…

www.martinfowler.com](https://www.martinfowler.com/bliki/AnemicDomainModel.html) [](https://danielrusnok.medium.com/what-is-anemic-domain-model-and-why-it-can-be-harmful-2677b1b0a79a) [## 什么是贫血型糖尿病？为什么它会有害？

### 贫血领域模型是软件开发中的一种常见方法，许多人甚至不知道他们正在使用它…

danielrusnok.medium.com](https://danielrusnok.medium.com/what-is-anemic-domain-model-and-why-it-can-be-harmful-2677b1b0a79a) [](https://medium.com/@ahmed.badeir/anemic-model-vs-rich-model-fce9df58d09e) [## 贫血模型与富裕模型

### 软件开发中最常见的反模式之一是“贫血模型”,在这篇文章中，我将解释…

medium.com](https://medium.com/@ahmed.badeir/anemic-model-vs-rich-model-fce9df58d09e) [](https://blog.latenightdev.net/rich-domain-models-for-the-win-696ddade1319) [## 丰富的领域模型赢得胜利！

### 帮自己一个忙:现在停止编写贫血的领域模型

blog.latenightdev.net](https://blog.latenightdev.net/rich-domain-models-for-the-win-696ddade1319)