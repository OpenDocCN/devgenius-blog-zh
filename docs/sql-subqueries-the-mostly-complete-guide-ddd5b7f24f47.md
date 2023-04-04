# SQL 子查询,(大部分)完整指南

> 原文：<https://blog.devgenius.io/sql-subqueries-the-mostly-complete-guide-ddd5b7f24f47?source=collection_archive---------6----------------------->

## 对最常见的子查询用途和关注点的深入回顾

![](img/8e08bc08cc80254705c13606acda1895.png)

Clark Van Der Beken 在 [Unsplash](https://unsplash.com/s/photos/geometric?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

# 基础知识

子查询是嵌套在主查询的 SELECT、FROM 或 WHERE 子句中的查询。子查询用括号括起来，以便与主查询分开。

在下面的示例中，使用 WHERE 子句中的子查询选择年龄大于平均值的所有用户。

```
SELECT first_name,
       last_name,
       age
FROM   users
WHERE  age > (SELECT *Avg*(age)
              FROM   users)
```

子查询出现在 WHERE 子句中比较的右侧。该子查询返回平均值的单个值(标量值)。如果我们用子查询代替实际平均值，查询的功能将完全相同:

```
SELECT first_name,
       last_name,
       age
FROM   users
WHERE  age > 31
```

# 深潜

为了更深入地研究子查询，我们可以看看子查询在 SELECT、FROM 和 WHERE 子句中的行为。这是一个很好的轨迹，因为每个用例都有独特的关注点。

所以让我们从顶部开始，用 SELECT。

# 在 SELECT 中使用子查询

select 子句**中的子查询必须只返回一行和一列；否则，出现错误**。因此，select 中的子查询通常用于生成聚合值，而不将行分组在一起。

考虑这个例子:

```
SELECT first_name,
       last_name,
       age,
       (SELECT *Avg*(age)
        FROM   users) as avg_age
FROM   users
```

类似上面例子的子查询的行为类似于聚合窗口函数。然而，与窗口函数不同，子查询不支持分区。

此外，SELECT 子句中的子查询可以从外部查询中访问表列。**当子查询引用外部查询表中的列时，它被称为相关子查询**。下面是 SELECT 子句中相关子查询的一个示例。

```
SELECT first_name,
       last_name,
       (SELECT verification_status
        FROM   verified_users
        WHERE  users.id = verified_users.user_id)
FROM   users 
```

在相关子查询中，查询是逐行执行的。SQL 引擎首先执行外部查询，然后执行内部查询。执行顺序允许内部查询引用外部查询中的列值。

当内部查询不引用外部查询列时(换句话说，当它不是相关子查询时)，查询不会逐行执行。**相反，SQL 引擎首先执行内部查询，然后执行外部查询。**

**注意，如果在上面的示例中，verified_users 包含重复的 user _ ids，查询将会失败。*

# 在 FROM 中使用子查询

子查询可以在 FROM 子句中用作查询选择的主结果集，也可以用作连接到标准表的结果集。

让我们看一下第一个场景的简单版本。

```
SELECT first_name,
       last_name
FROM   (SELECT first_name,
               last_name,
               state
        FROM   users
        WHERE  state = 'Kansas') kansas
```

请注意，在本例中，子查询被赋予了一个别名(kansas)。这是因为 FROM 子句中的所有子查询都必须有别名。

这里是一个在连接中使用的子查询的例子。这里，子查询必须有别名。

```
SELECT first_name,
       last_name,
       verification_status
FROM   users
       INNER JOIN (SELECT verification_status,
                          user_id
                   FROM   verified_users
                   WHERE  verification_status = 'true') v
               ON users.id = v.user_id
```

# 在 WHERE 中使用子查询

本节介绍如何在 WHERE 子句中使用子查询。这些查询有两种类型:单行和多行。

## 单行子查询

在本文开头的基本示例中，我们在 WHERE 子句中使用了一个返回单个值的子查询。

```
SELECT first_name,
       last_name,
       age
FROM   users
WHERE  age > (SELECT *Avg*(age)
              FROM   users)
```

下面是一个更复杂的示例，其中子查询本身是一个复杂的查询，用于确定雇员最近的奖金支出，然后选择所有更高的奖金支出。

```
SELECT user_id,
       bonus_amount
FROM   bonus_table
WHERE  bonus_amount > (SELECT bonus_amount
                FROM   (SELECT user_id,
                               bonus_amount,
                               **Row_number**()
                                 OVER(
                                   partition BY user_id
                                   ORDER BY date DESC) AS last_bonus
                        FROM   bonus_table
                        WHERE  user_id = 123) lb
                WHERE  last_bonus = 1)
```

这个例子演示了子查询可以像普通查询一样复杂和健壮(并且包括它们自己的子查询！).当您需要基于从复杂查询返回的单个值过滤数据**时，子查询通常是首选方法。**

**此外，这被称为单行子查询。**

关联时，单行子查询可以近似内部连接。例如，如果我们有一个包含所有用户的用户表和一个只包含电子邮件已验证的用户的 verified_users 表，下面的代码将模拟它们之间的内部连接。

```
SELECT first_name,
       last_name
FROM   users
WHERE  users.id = (SELECT user_id
                         FROM   verified_users
                         WHERE  users.id = user_id)
```

**注意，触发关联的是内部查询引用外部查询列。*

下面是相关单行子查询的另一个示例。

```
SELECT first_name,
       last_name,
       state,
       age
FROM   user_states AS outer_states
WHERE  age = (SELECT *Min*(age)
              FROM   user_states AS inner_states
              WHERE  inner_states.state = outer_states.state
              GROUP  BY inner_states.state)
```

该查询选择年龄等于其所在州的最小年龄的用户列表。

**注意，该查询需要别名才能执行。*

## 多行子查询

WHERE 子句中使用的返回多行的子查询称为多行子查询。必须将多行子查询与集合比较运算符(in、ALL、ANY)结合使用。**此外，像单行子查询一样，多行子查询必须只返回一列。**

如果您不熟悉这些集合运算符，请务必记住，每个运算符都用于确定一行是否包含在查询的输出中。

**使用 IN 运算符**

IN 运算符确定一个值是否存在于列值列表中。在下面的例子中，我们返回一个所有用户的列表，这些用户的年龄出现在居住在俄亥俄州的所有用户的年龄列表中。

```
SELECT first_name,
       last_name,
       age
FROM   user_states
WHERE  age IN (SELECT age
               FROM   user_states
               WHERE  state = 'Ohio')
```

**使用 ALL 运算符**

ALL 运算符确定单个行值与结果集的每个行值之间的比较是否一致为真。如果比较一致为真，则包括与比较左侧关联的行。

在下面的示例中，我们返回一个用户列表，这些用户的年龄大于居住在堪萨斯州的每个用户的年龄。

```
SELECT first_name,
       last_name,
       age
FROM   user_states
WHERE  age > ALL (SELECT age
               FROM   user_states
               WHERE  state = 'Kansas')
```

**使用任意运算符**

ANY 运算符确定单个行值与结果集的每个行值之间的比较是否至少有一次为真。如果比较至少有一次为真，则包括与比较左侧关联的行。

在下面的示例中，我们返回年龄大于各州用户最大年龄的用户列表。

```
SELECT first_name,
       last_name,
       state,
       age
FROM   user_states
WHERE  age > ANY (SELECT *Max*(age)
                  FROM   user_states
                  GROUP  BY state)
```

## 概括我们所学的内容

子查询是 SQL 中比较令人困惑的主题之一。如果你感到迷茫，请记住以下亮点:

1.  所有子查询都必须用括号括起来🎯
2.  SELECT 子句中使用的子查询必须返回单个值🎯
3.  FROM 子句中使用的子查询必须有别名🎯
4.  如果 WHERE 子句中的子查询返回多行，则它必须使用集合比较运算符(in、ALL、ANY)🎯
5.  引用外部查询列的子查询是相关的🎯

如果您想阅读子查询的一些不同解释，请尝试以下文章:

*   [W3 资源—什么是子查询](https://www.w3resource.com/sql/subqueries/understanding-sql-subqueries.php)
*   [W3 资源多行子查询](https://www.w3resource.com/sql/subqueries/multiplee-row-column-subqueries.php)
*   [SQL Server 相关子查询](https://www.sqlservertutorial.net/sql-server-basics/sql-server-correlated-subquery/)
*   [模式—带有练习题的子查询概述](https://mode.com/sql-tutorial/sql-sub-queries/)

感谢阅读。我希望这篇文章对你有所帮助。如果你这样做了，一定要给我一个关注更多的 SQL 内容！