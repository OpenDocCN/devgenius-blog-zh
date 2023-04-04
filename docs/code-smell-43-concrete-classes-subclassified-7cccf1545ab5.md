# 代码气味 43——具体分类

> 原文：<https://blog.devgenius.io/code-smell-43-concrete-classes-subclassified-7cccf1545ab5?source=collection_archive---------5----------------------->

## *继承。具体的类。重用。奇妙的混合。*

![](img/1b418dd9573df670bd75a5a2cc062ef4.png)

# 问题

*   糟糕的模型
*   连接
*   [里斯科夫换人](https://en.wikipedia.org/wiki/Liskov_substitution_principle)违规
*   方法覆盖
*   [映射器](https://codeburst.io/what-is-software-9a78c1172cf9)故障

# 解决方法

1.  子类应该是专门化的。
2.  重构层次结构。
3.  喜欢作文。
4.  叶类应该是具体的。
5.  非叶类应该是抽象的。

# 示例代码

## 错误的

## 对吧

# 侦查

重写一个具体的方法是一个清晰的气味。我们可以在大多数棉绒上执行这些政策。

抽象类应该只有几个具体的方法。我们可以对照预先设定的违规阈值进行检查。

# 标签

*   作文

# 结论

偶然的细分分类是初级开发者的第一个明显优势。

比较成熟的反而找作曲机会。

组合是动态的、多重的、可插入的、更易测试的、更易维护的，并且比继承耦合性更低。

如果实体遵循关系*的行为类似于*，则只对其进行子分类。

子类化之后，父类应该是抽象的。

# 关系

[](https://medium.com/dev-genius/code-smell-11-subclassification-for-code-reuse-2e2f5b564204) [## 代码味道 11 —代码重用的子类化

### 代码重用是好的。但是子类化会产生静态耦合。

medium.com](https://medium.com/dev-genius/code-smell-11-subclassification-for-code-reuse-2e2f5b564204) 

# 更多信息

[](https://en.wikipedia.org/wiki/Composition_over_inheritance) [## 继承之上的组合

### 面向对象编程(OOP)中的复合超越继承(或复合重用原则)是…

en.wikipedia.org](https://en.wikipedia.org/wiki/Composition_over_inheritance) 

> 软件是一种气体；它膨胀以充满它的容器。

*纳森·梅尔沃德*

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)