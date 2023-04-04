# 代码气味 68 —吸气剂

> 原文：<https://blog.devgenius.io/code-smell-68-getters-68571a0f7fa8?source=collection_archive---------3----------------------->

## 得到的东西是广泛而安全的。但这是一种非常糟糕的做法。

![](img/b0a53e775e22f77219274ec1adec7cfd.png)

照片由[维达尔·诺德里-马西森](https://unsplash.com/@vidarnm)在 [Unsplash](https://unsplash.com/s/photos/pull) 拍摄

# 问题

*   命名
*   信息隐蔽
*   连接
*   封装违规
*   易变性
*   贫血模型

# 解决方法

1.  避免吸气剂
2.  请改用域名
3.  保护您的实施决策。

# 示例代码

## 错误的

## 对吧

# 侦查

在某些情况下，吸气剂符合真正的责任。窗口返回它的颜色将是合理的，它可能会意外地将其存储为颜色。因此， *color()* 方法返回属性 color 可能是一个好的解决方案。

*getColor()* 中断[双射](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af)，因为它是实现性的，在我们的[映射器](https://codeburst.io/what-is-software-9a78c1172cf9)上没有真正的对应。

如果大多数 linters 用 getters 和 setters 检测到贫血的模型，它们可以警告我们。

# 标签

*   信息隐蔽

# 结论

Getters 和 Setters 是一种不成熟的实践。我们不去关注对象行为(本质)，而是不顾一切地去了解对象内脏(偶然)，去违背它们的实现。

# 关系

[](https://medium.com/dev-genius/code-smell-28-setters-5b0e764049aa) [## 气味代码 28 —设定器

### 初级程序员做的第一个练习。ide、教程和高级开发人员不断教他们这种反模式。

medium.com](https://medium.com/dev-genius/code-smell-28-setters-5b0e764049aa) [](https://medium.com/dev-genius/code-smell-01-anemic-models-f9fb5a1323b3) [## 代码气味 01——贫血模型

### 你的对象是一堆没有行为的公共属性。

medium.com](https://medium.com/dev-genius/code-smell-01-anemic-models-f9fb5a1323b3) [](https://medium.com/dev-genius/code-smell-64-inappropriate-intimacy-f1b064984094) [## 代码气味 64——不适当的亲密关系

### 两个纠结于爱情的阶层。

medium.com](https://medium.com/dev-genius/code-smell-64-inappropriate-intimacy-f1b064984094) 

# 更多信息

[](https://medium.com/dev-genius/nude-models-part-ii-getters-b039e5ad3427) [## 裸体模特第二部分:吸气剂

### 可靠的数据结构及其有争议的(读)访问。

medium.com](https://medium.com/dev-genius/nude-models-part-ii-getters-b039e5ad3427) 

> 原型的价值在于它给你的教育，而不是代码本身。

*艾兰·库伯*

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)