# 代码气味 55——对象狂欢

> 原文：<https://blog.devgenius.io/code-smell-55-object-orgy-529c754a2fac?source=collection_archive---------4----------------------->

## 如果你将你的对象视为数据持有者，你将违反它们的封装，但你不应该这样做，因为在现实生活中，你应该总是征求同意。

![](img/dfcf69ca219f597fbc3bef774b761f0b.png)

图片作者[尼古拉斯·普桑](https://www.nationalgallery.org.uk/paintings/nicolas-poussin-a-bacchanalian-revel-before-a-term#)

# 问题

*   信息隐藏违规
*   封装违规
*   连接

# 解决方法

1.  耦合到接口和行为，而不是数据。

# 示例代码

## 错误的

## 对吧

# 侦查

你可以设置你的 linters 来警告你公共属性，setters 和 getters 的使用，并阻止它们。

# 标签

*   连接

# 结论

如果您的类被 setters、getters 和 public 方法污染了，您肯定会有办法耦合到它们的意外实现。

# 也被称为

*   不适当的亲密

# 关系

[](https://medium.com/dev-genius/code-smell-01-anemic-models-f9fb5a1323b3) [## 代码气味 01——贫血模型

### 你的对象是一堆没有行为的公共属性。

medium.com](https://medium.com/dev-genius/code-smell-01-anemic-models-f9fb5a1323b3) [](https://medium.com/dev-genius/code-smell-28-setters-5b0e764049aa) [## 气味代码 28 —设定器

### 初级程序员做的第一个练习。ide、教程和高级开发人员不断教他们这种反模式。

medium.com](https://medium.com/dev-genius/code-smell-28-setters-5b0e764049aa) 

# 更多信息

[](https://en.wikipedia.org/wiki/Object_orgy) [## 对象狂欢

### 在计算机编程中，对象狂欢是一个术语，在 Perl 编程社区中很常见，描述一个常见的失败…

en.wikipedia.org](https://en.wikipedia.org/wiki/Object_orgy) [](https://refactoring.guru/es/smells/inappropriate-intimacy) [## 不适当的亲密

### 一个类使用另一个类的内部字段和方法。

重构大师](https://refactoring.guru/es/smells/inappropriate-intimacy) 

> *数据结构只是一种愚蠢的编程语言。*

比尔·戈斯帕

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)