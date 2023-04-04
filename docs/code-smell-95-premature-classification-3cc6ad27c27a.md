# 气味代码 95 —过早分类

> 原文：<https://blog.devgenius.io/code-smell-95-premature-classification-3cc6ad27c27a?source=collection_archive---------4----------------------->

## 我们过于一般化了。在我们看到足够多的具体化之前，我们不应该创造抽象。

![](img/adead55e86b3a91d9d456129ff5cd563.png)

法耶·康尼什在 [Unsplash](https://unsplash.com/s/photos/tree) 上的照片

> TL；博士:不要猜测未来会给你带来什么。

# 语境

亚里士多德式分类是计算机科学中的一个大问题。
我们倾向于在收集足够的知识和背景之前**对事物进行分类和命名。**

# 问题

*   未来学
*   糟糕的设计

# 解决方法

1.  等待凝固
2.  后期重构

# 示例代码

## 错误的

## 对吧

# 侦查

只有一个子类的抽象类是过早分类的标志

# 标签

*   糟糕的设计
*   分类

# 结论

当处理类时，抽象一出现，我们就给它们命名。

我们的规则是根据行为选择好的名字。

在命名具体的子类之前，我们不应该命名抽象。

# 关系

[](/code-smell-11-subclassification-for-code-reuse-2e2f5b564204) [## 代码味道 11 —代码重用的子类化

### 代码重用是好的。但是子类化会产生静态耦合。

blog.devgenius.io](/code-smell-11-subclassification-for-code-reuse-2e2f5b564204) 

# 更多信息

[](/what-exactly-is-a-name-part-i-the-quest-b812a4b1e0bf) [## 名字到底是什么？—第一部分:探索

### 我们都同意:好名声永远是最重要的。让我们找到他们。

blog.devgenius.io](/what-exactly-is-a-name-part-i-the-quest-b812a4b1e0bf) 

> 让我们改变对程序构造的传统态度:与其想象我们的主要任务是指导计算机做什么，不如让我们集中精力向人类解释我们希望计算机做什么。

唐纳德·e·克努特

[](/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)