# 日常编码问题:问题#11

> 原文：<https://blog.devgenius.io/daily-coding-problem-problem-11-3452b3a63ddb?source=collection_archive---------16----------------------->

![](img/cc29d03fd1c0157ae34a05361702889a.png)

照片由 [XPS](https://unsplash.com/@xps?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

我又遇到了另一个问题！。

# 问题

> 实现一个自动完成系统。也就是说，给定一个查询字符串`s`和一组所有可能的查询字符串，返回该组中以 s 为前缀的所有字符串。
> 
> 例如，给定查询字符串`de`和字符串集合[ `dog`、`deer`、`deal` ]，返回[ `deer`、`deal` ]。
> 
> 提示:尝试将字典预处理成更有效的数据结构，以加快查询速度。

这是一个相对简单的问题，可以使用 regex 来解决。让我们用 regex 试试吧！。

# 解决方案 1:正则表达式

**Python:**

**开始:**

**时间复杂度:** O(len(query))

**空间复杂度:** O(1)

我希望你们喜欢这篇文章。

如果你觉得有帮助，请分享和鼓掌非常感谢！😄

欢迎在评论区提出你的疑问！。