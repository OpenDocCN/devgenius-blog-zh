# 代码气味 37 —受保护的属性

> 原文：<https://blog.devgenius.io/code-smell-37-protected-attributes-4607260edb2d?source=collection_archive---------4----------------------->

受保护的属性对于封装和控制对我们属性的访问非常有用。他们可能在警告我们另一种气味。

![](img/988bf1e2a59ab821c035d5a6039d9a42.png)

照片由[乔纳森·法伯](https://unsplash.com/@farber)在 [Unsplash](https://unsplash.com/s/photos/safe-box) 上拍摄

# 问题

*   [子分类](https://maximilianocontieri.com/code-smell-11-subclassification-for-code-reuse)用于代码重用。
*   [利斯科夫替代](https://en.wikipedia.org/wiki/Liskov_substitution_principle)违反([固](https://en.wikipedia.org/wiki/SOLID)原理)。
*   可能的子类覆盖。

# 解决方法

1.  偏好构成
2.  不要子类化属性。
3.  提取行为以分离对象。
4.  使用[特性](https://en.wikipedia.org/wiki/Trait_(computer_programming)(如果有的话)。

## 错误的

## 对吧

# 侦查

在支持 *protected* 属性的语言中，我们可以通过策略来避免它们，或者对这种味道发出警告。

# 标签

*   包装

# 结论

受保护的属性是我们应该小心使用的另一个工具。每一个决定都是一种气味，我们应该非常小心属性和继承。

# 关系

[](https://medium.com/dev-genius/code-smell-11-subclassification-for-code-reuse-2e2f5b564204) [## 代码味道 11 —代码重用的子类化

### 代码重用是好的。但是子类化会产生静态耦合。

medium.com](https://medium.com/dev-genius/code-smell-11-subclassification-for-code-reuse-2e2f5b564204) 

# 更多信息

[维基百科上的特征](https://en.wikipedia.org/wiki/Trait_%28computer_programming%29)

> 子类不应该总是共享它们的父类的所有特征，但是可以通过继承来这样做。这会降低程序设计的灵活性。它还引入了在子类上调用没有意义的方法的可能性，或者由于方法不适用于子类而导致错误的可能性。

史蒂夫·克拉布尼克

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)