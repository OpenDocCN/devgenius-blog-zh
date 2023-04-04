# 代码气味 50 —对象键

> 原文：<https://blog.devgenius.io/code-smell-50-object-keys-c2599b569403?source=collection_archive---------2----------------------->

主键、id、引用。我们添加到对象中的第一个属性。它们在现实世界中并不存在。

![](img/2e0d208414ef3049d9ba256ee7f4c769.png)

由[莫里斯·威廉姆斯](https://unsplash.com/@mauricew98)在 [Unsplash](https://unsplash.com/s/photos/keychain) 上拍摄的照片

# 问题

*   连接
*   意外实现
*   [双射原理](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af)违反。

# 解决方法

1.  参考*对象*到*对象*。
2.  构建一个[映射器](https://codeburst.io/what-is-software-9a78c1172cf9)。
3.  只有在需要提供外部(偶然的)引用时才使用键。数据库、API、序列化。
4.  尽可能使用暗键或[guid](https://en.wikipedia.org/wiki/Universally_unique_identifier)。
5.  如果你害怕得到一个大的关系图，使用代理或者延迟加载。
6.  不要使用 dto。

# 示例代码

## 错误的

## 对吧

# 侦查

这是一项设计政策。

如果我们定义了包含序列 *id* 的属性或函数，我们可以强制业务对象向我们发出警告。

# 标签

*   意外的

# 结论

面向对象编程不需要 id。你引用对象(本质的)而从不引用 id(偶然的)。

如果您需要提供系统范围之外的引用(API、接口、序列化),请使用晦涩且无意义的 id(guid)。

# 关系

[](https://medium.com/dev-genius/code-smell-40-dtos-ca35f5d8f7c9) [## 代码气味 40—dto

### dto 被广泛使用并“解决”了实际问题，是吗？

medium.com](https://medium.com/dev-genius/code-smell-40-dtos-ca35f5d8f7c9) 

# 更多信息

[](https://codeburst.io/what-is-software-9a78c1172cf9) [## 软件有什么问题？

### 软件正在吞噬世界。我们这些工作、生活和热爱软件的人通常不会停下来思考它的…

codeburst.io](https://codeburst.io/what-is-software-9a78c1172cf9) [](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af) [## 唯一的软件设计原则

### 如果我们在一个单一的规则上建立我们的整个范式，我们可以保持它的简单并做出优秀的模型。

codeburst.io](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af) 

> 计算机科学中的所有问题都可以通过另一种间接方式来解决。

*大卫·惠勒*

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)