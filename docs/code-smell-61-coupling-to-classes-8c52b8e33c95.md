# 代码气味 61——耦合到类

> 原文：<https://blog.devgenius.io/code-smell-61-coupling-to-classes-8c52b8e33c95?source=collection_archive---------2----------------------->

## 上课很方便。我们可以随时调用它们。这样好吗？

![](img/0a5f74e8be072186a7d8189b8a991994.png)

由[马可·比安切蒂](https://unsplash.com/@marcobian)在 [Unsplash](https://unsplash.com/s/photos/hug) 上拍摄的照片

# 问题

*   连接
*   展开性
*   很难嘲笑

# 解决方法

1.  使用接口或特征(如果有的话)。
2.  使用依赖注入。
3.  支持松散耦合。

# 示例代码

## 错误的

## 对吧

# 侦查

我们几乎可以使用任何 linter 来查找对类的引用。我们不应该滥用，因为许多使用可能是假阳性。

# 标签

*   连接

# 结论

对接口的依赖使得系统耦合性降低，从而更具可扩展性和可测试性。

接口的变化没有具体的实现频繁。

有些对象实现了许多接口，声明哪个部分依赖于哪个接口使得耦合更细粒度，对象更内聚。

# 关系

[](https://medium.com/dev-genius/code-smell-30-mocking-business-885299c32d6f) [## 代码气味 30——嘲弄商业

### 在测试行为时，嘲笑是一个很好的帮助。像许多其他工具一样，我们正在滥用它们。

medium.com](https://medium.com/dev-genius/code-smell-30-mocking-business-885299c32d6f) 

# 更多信息

[](https://codeburst.io/coupling-the-one-and-only-software-design-problem-869e293a9f04) [## 耦合:唯一的软件设计问题

### 对我们软件的所有故障进行根本原因分析，会发现一个有多种伪装的单一罪魁祸首。

codeburst.io](https://codeburst.io/coupling-the-one-and-only-software-design-problem-869e293a9f04) [](https://en.wikipedia.org/wiki/Loose_coupling) [## 松耦合

### 在计算和系统设计中，松耦合系统是指其中每个组件都拥有或利用…

en.wikipedia.org](https://en.wikipedia.org/wiki/Loose_coupling) 

> 当你的代码依赖于一个接口时，这种依赖通常是很小且不明显的。除非接口改变，否则您的代码不必改变，并且接口通常远不如其背后的代码改变频繁。当您有一个接口时，您可以编辑实现该接口的类或添加实现该接口的新类，所有这些都不会影响使用该接口的代码。
> 
> 因此，依赖接口或抽象类比依赖具体类更好。当您依赖于不太易变的东西时，您可以最小化特定变化触发大规模重新编译的机会。

*迈克尔羽毛*

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)