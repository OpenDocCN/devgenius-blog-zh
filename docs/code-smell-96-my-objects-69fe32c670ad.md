# 代码气味 96 —我的物品

> 原文：<https://blog.devgenius.io/code-smell-96-my-objects-69fe32c670ad?source=collection_archive---------4----------------------->

## 你没有自己的物品。

![](img/20f889a54501c9089cf287fcbcfae892.png)

米歇尔·bożek 在 [Unsplash](https://unsplash.com/s/photos/kid-toy) 上拍摄的照片

*TL；DR:不要用* ***my*** *作为名字前缀。*

# 问题

*   缺乏背景
*   双射断层

# 解决方法

1.删除“*我的“*前缀。

2.更改为角色建议名称。

# 语境

几个老教程用‘我的’这个词作为懒名。
这是模糊的，会导致上下文错误。

# 示例代码

## 错误的

## 对吧

# 侦查

[x]自动

我们可以告诉 linters 和 static checkers 搜索这个前缀并警告我们。

# 标签

*   命名

# 结论

避免使用 **my** 。

对象根据使用环境而变化。

# 更多信息

[](/what-exactly-is-a-name-part-ii-rehab-fc5058b3ff1) [## 名字到底是什么？第二部分:康复

### 我们都同意:好名声永远是最重要的。让我们找到他们。

blog.devgenius.io](/what-exactly-is-a-name-part-ii-rehab-fc5058b3ff1) 

> 回想我修改代码的经历，我发现我花在阅读现有代码上的时间比写新代码的时间多得多。因此，如果我想让我的代码便宜，我应该让它易于阅读。

肯特·贝克

[](/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)