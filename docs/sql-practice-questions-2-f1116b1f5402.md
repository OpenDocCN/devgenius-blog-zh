# SQL 练习题— #2

> 原文：<https://blog.devgenius.io/sql-practice-questions-2-f1116b1f5402?source=collection_archive---------8----------------------->

SQL 练习题第二部分

点击此处查看第一部分 [第三部分](/sql-practice-questions-3-9fd3d6e56058) [第四部分](/sql-practice-questions-4-e24a6bdb32d4) [第五部分](/sql-practice-questions-5-456cfb41757a)

## 问题 1

编写一个 SQL 查询，向使用了您朋友喜欢的页面的用户推荐页面。它不应该推荐你已经喜欢的网页。返回**任意订单**中没有重复的结果表。

**例 1:**

```
**Input:** 
Friendship table:
+----------+----------+
| user1_id | user2_id |
+----------+----------+
| 1        | 2        |
| 1        | 3        |
| 1        | 4        |
| 2        | 3        |
| 2        | 4        |
| 2        | 5        |
| 6        | 1        |
+----------+----------+
Likes table:
+---------+---------+
| user_id | page_id |
+---------+---------+
| 1       | 88      |
| 2       | 23      |
| 3       | 24      |
| 4       | 56      |
| 5       | 11      |
| 6       | 33      |
| 2       | 77      |
| 3       | 77      |
| 6       | 88      |
+---------+---------+
**Output:** 
+------------------+
| recommended_page |
+------------------+
| 23               |
| 24               |
| 56               |
| 33               |
| 77               |
+------------------+
**Explanation:** 
User one is friend with users 2, 3, 4 and 6.
Suggested pages are 23 from user 2, 24 from user 3, 56 from user 3 and 33 from user 6.
Page 77 is suggested from both user 2 and user 3.
Page 88 is not suggested because user 1 already likes it.
```

首先，我们找到所有和`user_id=1`是朋友的用户。然后，我们获取朋友喜欢的页面，并删除用户 1 已经喜欢的页面。

```
WITH friends AS (
    SELECT user1_id AS friend
    FROM Friendship
    WHERE user2_id = 1

    UNION ALL

    SELECT user2_id AS friend
    FROM Friendship
    WHERE user1_id = 1
),
already_liked_pages AS (
    SELECT page_id
    FROM Likes
    WHERE user_id = 1
)
SELECT
    DISTINCT page_id as recommended_page
FROM Likes
WHERE user_id IN
    (SELECT friend FROM friends)
AND page_id NOT IN
    (SELECT page_id from already_liked_pages)
```

## 问题 2

编写一个 SQL 查询来评估`Expressions`表中的布尔表达式。返回**任意顺序**中的结果表。

**例 1:**

```
**Input:** 
Variables table:
+------+-------+
| name | value |
+------+-------+
| x    | 66    |
| y    | 77    |
+------+-------+
Expressions table:
+--------------+----------+---------------+
| left_operand | operator | right_operand |
+--------------+----------+---------------+
| x            | >        | y             |
| x            | <        | y             |
| x            | =        | y             |
| y            | >        | x             |
| y            | <        | x             |
| x            | =        | x             |
+--------------+----------+---------------+
**Output:** 
+--------------+----------+---------------+-------+
| left_operand | operator | right_operand | value |
+--------------+----------+---------------+-------+
| x            | >        | y             | false |
| x            | <        | y             | true  |
| x            | =        | y             | false |
| y            | >        | x             | true  |
| y            | <        | x             | false |
| x            | =        | x             | true  |
+--------------+----------+---------------+-------+
**Explanation:** 
As shown, you need to find the value of each boolean expression in the table using the variables table.
```

使用 case when 语句解决了这个问题。基于运算符，我们使用正确的代数运算符来检查表达式是真还是假。

```
SELECT e.left_operand, e.operator, e.right_operand,
(
  CASE
    WHEN e.operator = '<' AND v1.value < v2.value THEN 'true'
    WHEN e.operator = '=' AND v1.value = v2.value THEN 'true'
    WHEN e.operator = '>' AND v1.value > v2.value THEN 'true'
    ELSE 'false'
  END
) AS value
FROM Expressions e
JOIN Variables v1
ON e.left_operand = v1.name
JOIN Variables v2
ON e.right_operand = v2.name
```

## 问题 3

有一队人等着上公共汽车。但是，巴士的重量限制为`1000` **公斤**，因此可能会有一些人无法上车。编写一个 SQL 查询来查找可以在不超过重量限制的情况下登上公交车的最后一个人的`person_name`。生成测试用例，使得第一个人不超过重量限制。

**例 1:**

```
**Input:** 
Queue table:
+-----------+-------------+--------+------+
| person_id | person_name | weight | turn |
+-----------+-------------+--------+------+
| 5         | Alice       | 250    | 1    |
| 4         | Bob         | 175    | 5    |
| 3         | Alex        | 350    | 2    |
| 6         | John Cena   | 400    | 3    |
| 1         | Winston     | 500    | 6    |
| 2         | Marie       | 200    | 4    |
+-----------+-------------+--------+------+
**Output:** 
+-------------+
| person_name |
+-------------+
| John Cena   |
+-------------+
**Explanation:** The folowing table is ordered by the turn for simplicity.
+------+----+-----------+--------+--------------+
| Turn | ID | Name      | Weight | Total Weight |
+------+----+-----------+--------+--------------+
| 1    | 5  | Alice     | 250    | 250          |
| 2    | 3  | Alex      | 350    | 600          |
| 3    | 6  | John Cena | 400    | 1000         | (last person to board)
| 4    | 2  | Marie     | 200    | 1200         | (cannot board)
| 5    | 4  | Bob       | 175    | ___          |
| 6    | 1  | Winston   | 500    | ___          |
+------+----+-----------+--------+--------------+
```

一个简单的方法是计算上车人数的累计总数，看看是否超过了公共汽车总载客量的限制。

```
SELECT person_name
FROM (
    SELECT Turn, person_name, SUM(weight) OVER(ORDER BY Turn asc) AS Total_Weight
    FROM Queue
) AS T
WHERE Total_weight <= 1000
ORDER BY Total_weight desc
limit 1
```

## 问题 4

编写一个 SQL 查询来报告每只股票的**资本收益/损失**。股票的**资金损益**是一次或多次买卖股票后的总损益。返回**任意顺序**中的结果表。

**例 1:**

```
**Input:** 
Stocks table:
+---------------+-----------+---------------+--------+
| stock_name    | operation | operation_day | price  |
+---------------+-----------+---------------+--------+
| Leetcode      | Buy       | 1             | 1000   |
| Corona Masks  | Buy       | 2             | 10     |
| Leetcode      | Sell      | 5             | 9000   |
| Handbags      | Buy       | 17            | 30000  |
| Corona Masks  | Sell      | 3             | 1010   |
| Corona Masks  | Buy       | 4             | 1000   |
| Corona Masks  | Sell      | 5             | 500    |
| Corona Masks  | Buy       | 6             | 1000   |
| Handbags      | Sell      | 29            | 7000   |
| Corona Masks  | Sell      | 10            | 10000  |
+---------------+-----------+---------------+--------+
**Output:** 
+---------------+-------------------+
| stock_name    | capital_gain_loss |
+---------------+-------------------+
| Corona Masks  | 9500              |
| Leetcode      | 8000              |
| Handbags      | -23000            |
+---------------+-------------------+
```

当我们买入时，意味着我们花了那么多钱，当我们卖出时，我们得到了那么多钱。总收益或总损失为 sum(收益)— sum(支出)。我们使用 case when 表达式对价格进行正负赋值，然后求和。

```
select stock_name, 
    SUM(CASE WHEN operation='Buy' THEN -price else price end) as capital_gain_loss
from Stocks
GROUP BY stock_name
```

查询愉快！！