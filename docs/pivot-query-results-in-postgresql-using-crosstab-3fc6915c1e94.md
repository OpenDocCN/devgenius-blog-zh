# 使用交叉表透视 PostgreSQL 中的查询结果

> 原文：<https://blog.devgenius.io/pivot-query-results-in-postgresql-using-crosstab-3fc6915c1e94?source=collection_archive---------0----------------------->

![](img/d6731e9c4c4eec829e79fea9f53d291d.png)

卡斯帕·卡米尔·鲁宾在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

如果您使用过 Microsoft Excel 或 SQL Server，您可能会遇到 pivot 函数，它允许您轻松地转换数据，以便进行比较和查看模式。PostgreSQL 在交叉表函数中拥有相同的功能。

这两个函数从本质上改变了通常垂直排列的数据的方向，使其水平显示。本教程将研究如何利用这个工具。

## 目录

1.  [*先决条件*](#2513)
2.  [*测试场景*](#85e5)
3.  [*设置数据库*](#f12f)
4.  [*了解交叉表功能*](#1eb0)
5.  [*应用交叉表功能*](#388e)
6.  [*结论*](#fa51)

## 先决条件

要阅读这篇文章，您需要对 SQL 有一个基本的了解，尤其是 PostgreSQL。

## 测试场景

该数据库将侧重于三个实体:商店，产品和产品的销售。有三个商店和三种产品，每个商店都可以销售任何一种产品。尽管 sales 表不包含 created_at 属性，但假设我们拥有的记录详细记录了一周内发生的销售额。

本教程的目的是能够将此输出:

转换成以下输出:

## 设置数据库

首先，通过首选客户端打开 PostgreSQL 并创建一个新数据库。

```
CREATE DATABASE crosstab_tutorial;
```

创建数据库后，设置您的表。如上所述，有三个关系:商店表、产品表和销售表。

创建好表后，运行查询来插入虚拟数据。

## 了解交叉表函数

为了能够利用交叉表函数，需要为每个数据库启用一次 tablefunc 模块，这可以使用命令来完成

```
CREATE EXTENSION IF NOT EXISTS tablefunc;
```

交叉表函数有两种形式，一种简单的形式接受一个输入参数，另一种更具表现力的形式接受两个输入参数。

在**简单表单**中，交叉表的输出被限制为正好三列。因此，它对一个组中的所有值一视同仁，将每个值插入第一个可用的列中。但是，如果您的值中有一些空条目，并且您希望值列对应于特定的数据类别，则它不会按预期工作。

```
crosstab(text sql)
```

本教程将关注于**更具表现力的形式**，它接受两个输入参数

```
crosstab(text source_sql, text category_sql)
```

双参数变量提供了一种解决单参数变量所带来的空条目问题的方法。它通过接受对应于输出列的类别的显式列表来处理这个问题。这个显式列表是第二个参数。

`source_sql`产生源数据集。该数据必须包含一个将作为`row_name`的列。该列必须是第一列。数据必须有两个附加列:类别列**和数值列**。**类别列和值列总是最后两个。**

在我们的测试案例中，行名列将是商店名称，类别列将是产品名称，值列将是售出单位的总和。

`source_sql`可能有额外的列，但是上述三个基本列是必需的，并且按照上面所示的顺序排列。额外的列将出现在 row_name 列和 category 列之间，如下所示:

`category_sql`按照应用于`source_sql`的相同顺序生成我们的类别列表。因此，如果`category_sql`以特定方式订购，相同的订购条件应适用于`source_sql`中的类别栏。

`category_sql`查询必须只产生一列和至少一行，否则将会产生错误。结果必须只包含不同的值，重复的值将导致错误。

`crosstab`函数同样返回类型`setof`的结果，因此输出列的实际名称和类型必须在`FROM`子句中明确定义，并且必须对应于`source_sql.`中的列类型。`FROM`子句必须定义正确数量的输出列及其正确的数据类型。

如果`*source_sql*`查询的结果有 n 列，其中的前 n-2 列必须与输出的前 n-2 列匹配。剩余的输出列必须具有`*source_sql*`查询结果的最后一列的类型，并且必须与`*category_sql*`查询结果中的行数一样多。

因此，交叉表函数的输出应该包含行名列和任何额外的列。在这些之上，应该有一个由`category_sql.`产生的每个不同类别的列

`source_sql`查询结果中具有相同 row_name 值的每组行将在输出结果中被压缩成一行。

因此，`source_sql`必须包含`order by 1`子句，以便将相同的 row_names 分组在一起。

## 应用交叉表函数

首先，我们需要使用以下命令来启用上一节中描述的 tablefunc 模块:

```
CREATE EXTENSION IF NOT EXISTS tablefunc;
```

让我们看看 **source_sql** ，它将提供我们需要转换的结果集。我们将使用的语句是:

当查询运行时，它将产生:

如上表所示， **store_name** 列是我们的 row_name， **product_name** 对应于 category 列， **total_units** 对应于 value 列。

至于 **category_sql** ，我们将使用产品名称，并通过以下查询获得它们:

这将产生如下所示的类别:

有了源 sql 和类别 sql，我们现在可以通过下面的查询将交叉表函数应用到源结果中

这将把我们的数据转换成如下所示的格式:

输出值列由源数据集中具有匹配类别值的行的值字段填充。例如，在源数据集中，根据表中的第 4 行，Beatty Group 销售了 470 单位的葡萄酒，这将出现在 Beatty Group 行和 wine 列下。

其匹配类别不存在于该组的任何输入行中的输出列用空值填充。

如上所述，`source_sql`应该总是指定`ORDER BY 1`以确保具有相同 row_name 的值被分组在一起。然而，组内类别的排序并不重要。

如交叉表查询所示，`FROM`子句中指定的订单应该与`category_sql`产生的订单相匹配。

## 结论

交叉表在日常操作中起着重要的作用，一旦理解了，就很容易使用。

从 PostgresSQl 文档中找到更多关于交叉表函数的信息[https://www.postgresql.org/docs/12/tablefunc.html](https://www.postgresql.org/docs/12/tablefunc.html)

*PS:本教程中的虚拟数据是使用*[*【https://www.mockaroo.com/】*](https://www.mockaroo.com/)生成的