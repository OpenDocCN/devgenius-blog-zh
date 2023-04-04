# 气味代码 28 —设定器

> 原文：<https://blog.devgenius.io/code-smell-28-setters-5b0e764049aa?source=collection_archive---------4----------------------->

初级程序员做的第一个练习。ide、教程和高级开发人员不断教他们这种反模式。

![](img/dea3ffac1a8897a085b3fdb0b2ecc0cd.png)

由[维克多·罗德里格斯](https://unsplash.com/@vimarovi)在 [Unsplash](https://unsplash.com/s/photos/crowded) 上拍摄的照片

# 问题

*   易变性
*   信息隐蔽
*   贫血模型
*   快速失败
*   完整
*   重复代码

# 解决方法

1.  避免 Setters
2.  设置私有初始化的基本属性。

# 示例代码

## 错误的

突变带来了很多问题

违反了信息隐藏

# 对吧

# 侦查

第一步是禁止公共属性(如果语言允许的话)。

其次，我们将搜索方法 *setXXXX()* ，分析方法结构(应该是对属性 *xxxx* 的赋值)。

我们不应该禁止设置*意外状态*的方法，因为这是有效的。他们不应该被命名为*设置者*，因为他们要求对象*改变*，但是他们不*设置*任何东西。

# 例子

*   DTOs

# 例外

对于非必要的属性，设置属性是安全的。

本质行为是区分一个物体和另一个物体的东西。

与行为有关，与*数据*无关。它不是一个*主键*定义。

一些模式，像 [Builder](https://en.wikipedia.org/wiki/Builder_pattern) 需要以一种受控的、递增的方式设置部件。验证在最后完成，并且[真实实体隐喻](https://codeburst.io/what-is-software-9a78c1172cf9)需要它。

设置*偶然值*有许多已经提到的缺点和考虑。

# 标签

*   变化
*   信息隐蔽

# 结论

创建不完整和贫血的对象是一种非常糟糕的做法，违反了
可变性、快速失效原则和真实世界[双射](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af)。

# 关系

[](https://medium.com/dev-genius/code-smell-01-anemic-models-f9fb5a1323b3) [## 代码气味 01——贫血模型

### 你的对象是一堆没有行为的公共属性。

medium.com](https://medium.com/dev-genius/code-smell-01-anemic-models-f9fb5a1323b3) 

# 更多信息

[](https://codeburst.io/fail-fast-3f3f036032b0) [## 快速失败

### 失败是时尚。制造比思考容易得多，失败不是耻辱，让我们把这个想法带到我们的…

codeburst.io](https://codeburst.io/fail-fast-3f3f036032b0) 

这里是关于*设置器*的完整讨论

[](https://medium.com/dev-genius/nude-models-part-i-setters-77ac784a91f3) [## 裸体模特第一部分:模特

### 可靠的数据结构及其有争议的(写)访问。

medium.com](https://medium.com/dev-genius/nude-models-part-i-setters-77ac784a91f3) 

> 面向对象的编程语言支持封装，从而提高了软件重用、精炼、测试、维护和扩展的能力。只有在设计过程中最大化封装，才能实现这种支持的全部好处。

*丽贝卡·沃斯-布洛克*

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)