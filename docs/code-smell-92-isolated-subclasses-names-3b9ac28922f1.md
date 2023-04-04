# 代码气味 92 —独立的子类名称

> 原文：<https://blog.devgenius.io/code-smell-92-isolated-subclasses-names-3b9ac28922f1?source=collection_archive---------3----------------------->

## 如果你的类是全局的，使用全限定名

![](img/084923a06907798fe7caeb08eee48c40.png)

由[Edvard Alexander lvaag](https://unsplash.com/@edvardr)在 [Unsplash](https://unsplash.com/s/photos/hierarchy) 上拍摄的照片

> *TL；DR:不要在子类中使用缩写*

# 问题

*   可读性
*   错误

# 解决方法

1.  重命名您的类以提供上下文
2.  使用模块、命名空间或完全限定名

# 示例代码

## 错误的

```
abstract class PerserveranceDirection { 
}class North extends PerserveranceDirection {}
class East extends PerserveranceDirection {}
class West extends PerserveranceDirection {}
class South extends PerserveranceDirection {}//Subclasses have short names and meaningless outside the hierarchy
//If we reference East we might mistake it for the Cardinal Point
```

## 对吧

```
abstract class PerserveranceDirection { 
}class PerserveranceDirectionNorth extends PerserveranceDirection {}
class PerserveranceDirectionEast extends PerserveranceDirection {}
class PerserveranceDirectionWest extends PerserveranceDirection {}
class PerserveranceDirectionSouth extends PerserveranceDirection {}//Subclasses have fully quallified names
```

# 侦查

自动检测不是一件容易的事情。我们可以对子类执行本地命名策略。

# 标签

*   命名

# 结论

明智地选择你的名字。

如果您的语言支持，请使用模块、命名空间和局部范围。

# 关系

[](/code-smell-11-subclassification-for-code-reuse-2e2f5b564204) [## 代码味道 11 —代码重用的子类化

### 代码重用是好的。但是子类化会产生静态耦合。

blog.devgenius.io](/code-smell-11-subclassification-for-code-reuse-2e2f5b564204) 

# 更多信息

[](/what-exactly-is-a-name-part-i-the-quest-b812a4b1e0bf) [## 名字到底是什么？—第一部分:探索

### 我们都同意:好名声永远是最重要的。让我们找到他们。

blog.devgenius.io](/what-exactly-is-a-name-part-i-the-quest-b812a4b1e0bf) [](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af) [## 唯一的软件设计原则

### 如果我们在一个单一的规则上建立我们的整个范式，我们可以保持它的简单并做出优秀的模型。

codeburst.io](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af) 

> 在与缓慢的系统进行的永无止境的战斗中，程序员的主要武器是改变内部模块结构。我们的第一反应应该是重组模块的数据结构。

*佛瑞德·P·布鲁克斯*

[](/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)