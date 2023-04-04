# 代码气味 31——业务对象上的意外方法

> 原文：<https://blog.devgenius.io/code-smell-31-accidental-methods-on-business-objects-2ff9dc48a783?source=collection_archive---------4----------------------->

向对象添加持久化、序列化、显示、导入、导出代码会增加其协议并带来耦合。

![](img/1183f5ef9ae0ed09ab3545bb35cea1c7.png)

照片由 [Unsplash](https://unsplash.com/s/photos/mess) 上的 [Robert Bye](https://unsplash.com/@robertbye) 拍摄

# 问题

*   可读性
*   连接
*   可维护性

# 解决方法

1.  保持你的物品干净。
2.  分离业务对象。
3.  分离意外关注点:将持久性、格式化、序列化转移到特殊对象。
4.  使用[双射](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af)保留基本协议。

# 例子

*   坚持
*   标识符
*   序列化
*   格式化

# 示例代码

## 错误的

## 对吧

# 侦查

基于对可疑姓名的命名和暗示来创建林挺规则是困难的(但并非不可能)。

# 例外

*   一些框架强迫我们在对象中注入脏代码。(例如标识符)。

我们应该尝试使用更好的语言/框架。

# 标签

*   宣言的

# 结论

我们很习惯看到商业对象被污染。这很正常。我们需要反思这些设计的后果和耦合。

> 简单的事情应该简单，复杂的事情应该可能。

艾伦·凯

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)