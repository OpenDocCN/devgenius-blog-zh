# 代码气味 97——没有同理心的错误消息

> 原文：<https://blog.devgenius.io/code-smell-97-error-messages-without-empathy-952c320976d8?source=collection_archive---------3----------------------->

## 我们应该特别注意用户(和我们自己)的错误描述。

![](img/d372638bba6104c8d2472bfd0eb30468.png)

[Unsplash](https://unsplash.com/s/photos/error-message) 上[视觉](https://unsplash.com/@visuals)的照片

> TL；DR:使用有意义的完整描述并建议纠正措施。

# 问题

*   最小惊奇原则

# 解决方法

1.使用声明性错误消息

2.显示清除退出动作

# 语境

程序员很少是 UX 专家。

我们也低估了我们可以在柜台两边的事实。

# 示例代码

## 错误的

## 对吧

# 侦查

[X]手册

我们需要阅读代码审查中的所有异常消息。

# 标签

*   例外
*   UX

# 结论

当提出异常或显示消息时，我们需要考虑我们的最终用户。

> 虽然众所周知程序员从不犯错，但是通过检查程序中关键点的错误来取悦用户仍然是一个好主意。

*罗伯特·d·施耐德*

[](/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)