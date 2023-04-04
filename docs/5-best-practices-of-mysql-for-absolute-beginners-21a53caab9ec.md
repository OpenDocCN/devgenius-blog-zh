# MySQL 新手的 5 个最佳实践

> 原文：<https://blog.devgenius.io/5-best-practices-of-mysql-for-absolute-beginners-21a53caab9ec?source=collection_archive---------4----------------------->

## 从一开始就纠正你的错误

![](img/b7c1b3f831cdedd1e731d1cec160044f.png)

照片由[布伦丹·丘奇](https://unsplash.com/@bdchu614?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/idea?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

当我们需要变得优秀或高于平均水平时，我们应该通过展示我们的工作过程来证明这一点。你正在做的事情可能不遵循有组织的方法也知道同样的事情。

但是从一开始，如果你遵循最佳实践，你会变得非常出色，在你的工作岗位上是不可替代的。

# 数据分析工作

作为数据分析师，我们需要组织数据，从数据库中找出一些东西。这就是为什么我们可能有小型或大型数据表。我们应该用这张桌子玩。

通过使用如此多的语法，我们解决了如此多的关键问题。我们应该在 MySQL 中工作，查询我们想知道的东西。不仅仅是 MySQL，我们也用 Python，R 语言，Power Bi 等。

在这篇文章中，我写的是 MySQL 的最佳实践，这是非常基本的。

# 1.避免选择*

当我们使用这些命令`SELECT *`时，我们将从数据库中获取所有的表。但是一般来说，我们不需要一次收集所有的信息。

它给出了不必要的数据，看起来很乱，非常大。这不是一个可以理解的，清晰的信息。

所以我们可以有选择地使用这个命令。假设我们使用它，

`SELECT first_name, Customer_id`，那么我们一次只会得到这两条信息。

# 2.日期时间

当您使用日期-时间时，您应该遵循可接受的全球格式。某处可能用 DD-MM-YYYY。

但是最佳实践是使用 YYYY-MM-DD。试着正确使用这个来储存。

# 3.不要随意使用 ORDER BY 子句

需要这个子句的时候就用。一般来说，它适用于多用户环境。

当需要了解性能时，我使用`ORDER BY`从句。它不适合最大规模的数据库。所以我尽量只对次要数据库使用它。

# 4.使用 EXISTS 子句

在开始查询或了解现有数据之前，我们使用语法从`WHERE`子句显示所有数据。但这不是好的做法。

当我们回忆包含记录的子查询 period 时，`EXISTS`子句会快速给出结果。如果子查询没有历史记录，那么`EXISTS`命令返回 false。

# 5.使用连接

Join 非常有效，并且比子查询更快。但是我更喜欢用`LEFT_JOIN`和`RIGHT_JOIN`。因为这两个主要是一个表中有组织的数据。

虽然子查询命令比连接命令更容易阅读，但是使用连接的性能是显著且快速的。

点击[了解更多关于 JOIN 的信息。](https://javascript.plainenglish.io/learn-join-in-mysql-database-a-practical-approach-1933c5c6bae6)如果你想知道成为数据分析师的路线图，[点击这里](/beginner-roadmap-to-become-a-successful-data-analyst-f864703844f5)。