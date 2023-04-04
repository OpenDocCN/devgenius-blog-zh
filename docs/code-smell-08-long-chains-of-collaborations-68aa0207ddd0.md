# 代码气味 08——长长的合作链

> 原文：<https://blog.devgenius.io/code-smell-08-long-chains-of-collaborations-68aa0207ddd0?source=collection_archive---------8----------------------->

使长链产生耦合和涟漪效应。任何连锁变化都会破坏代码。

![](img/124de3a52cab1059a59f9479d31579d2.png)

[耐嚼](https://unsplash.com/@chewy)在 [Unsplash](https://unsplash.com/s/photos/dog) 上拍照

# 问题

*   连接
*   打破封装

# 解决方法

*   创建中间方法。
*   想想[德米特里](https://en.wikipedia.org/wiki/Law_of_Demeter)定律。
*   创建更高级别的消息。

# 示例代码

## 错误的

## 对吧

# 侦查

使用解析树可以实现自动检测。

# 也被称为

*   德米特里定律

# 标签

*   宣言的
*   包装

# 结论

避免连续的消息调用。尝试隐藏中间协作并创建新的协议。

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)