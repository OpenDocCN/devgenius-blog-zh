# SQL 练习问题— #4

> 原文：<https://blog.devgenius.io/sql-practice-questions-4-e24a6bdb32d4?source=collection_archive---------17----------------------->

第四部分 SQL 实践问题

点击此处查看[第一部分](/sql-practice-questions-1-800ed65d99b2) [第二部分](/sql-practice-questions-2-f1116b1f5402) [第三部分](/sql-practice-questions-3-9fd3d6e56058) [第五部分](/sql-practice-questions-5-456cfb41757a)

# 问题 1

写一个 SQL 查询来交换每两个连续学生的座位号。如果学生人数是奇数，则不会交换最后一名学生的 id。返回按`id` **排序的结果表，升序为**。

**示例:**

```
**Input:** 
Seat table:
+----+---------+
| id | student |
+----+---------+
| 1  | Abbot   |
| 2  | Doris   |
| 3  | Emerson |
| 4  | Green   |
| 5  | Jeames  |
+----+---------+
**Output:** 
+----+---------+
| id | student |
+----+---------+
| 1  | Doris   |
| 2  | Abbot   |
| 3  | Green   |
| 4  | Emerson |
| 5  | Jeames  |
+----+---------+
**Explanation:** 
Note that if the number of students is odd, there is no need to change the last one's seat.
```

**方法:**交换用例时，根据 id 是奇数还是偶数来选择上一个的值。T

```
SELECT 
    CASE 
      WHEN MOD(id, 2)=1 AND id != (SELECT COUNT(*) FROM seat) THEN id+1
      WHEN MOD(id, 2)=0 THEN id-1
      ELSE id 
    END as id, 
    student
FROM seat
ORDER BY id
```

在上面的查询中，我们正在评估每行的座位数，如果我们使用自连接，我们可以改进这一点。

```
SELECT s1.id as id, coalesce(s2.student, s1.student) as student
FROM Seat as s1
LEFT join Seat as s2
ON (s1.id%2=0 AND s1.id = s2.id+1) OR (s1.id%2=1 AND s1.id=s2.id-1)
```

# 问题 2

编写一个 SQL 查询来查找`2019-08-16`上所有产品的价格。假设所有产品变更前的价格为`10`。按**任意顺序返回结果表**。

**例:**

```
**Input:** 
Products table:
+------------+-----------+-------------+
| product_id | new_price | change_date |
+------------+-----------+-------------+
| 1          | 20        | 2019-08-14  |
| 2          | 50        | 2019-08-14  |
| 1          | 30        | 2019-08-15  |
| 1          | 35        | 2019-08-16  |
| 2          | 65        | 2019-08-17  |
| 3          | 20        | 2019-08-18  |
+------------+-----------+-------------+
**Output:** 
+------------+-------+
| product_id | price |
+------------+-------+
| 2          | 50    |
| 1          | 35    |
| 3          | 10    |
+------------+-------+
```

**方式:**通过排名来选择产品的最后一次变更日期。如果是最后一个`change_date < 2019–08–16`我们就接受这个价格。我们对所有产品进行联合，并将它们的价格表示为 10，这在最后一次查询(cte)中没有出现

```
WITH cte as (
    select product_id, new_price, 
    rank() over(
          PARTITION BY product_id ORDER BY change_date desc
        ) as rnk from Products
    WHERE change_date <= '2019-08-16'
)
SELECT product_id, new_price as price 
FROM cte 
WHERE rnk = 1UNIONSELECT product_id, 10  
FROM Products 
WHERE product_id NOT IN (SELECT product_id from cte)
```

# 问题 3

编写一个 SQL 查询来报告每个玩家赢得的大满贯锦标赛的数量。不包括未赢得任何锦标赛的球员。按**任意顺序返回结果表**。

**例:**

```
**Input:** 
Players table:
+-----------+-------------+
| player_id | player_name |
+-----------+-------------+
| 1         | Nadal       |
| 2         | Federer     |
| 3         | Novak       |
+-----------+-------------+
Championships table:
+------+-----------+---------+---------+---------+
| year | Wimbledon | Fr_open | US_open | Au_open |
+------+-----------+---------+---------+---------+
| 2018 | 1         | 1       | 1       | 1       |
| 2019 | 1         | 1       | 2       | 2       |
| 2020 | 2         | 1       | 2       | 2       |
+------+-----------+---------+---------+---------+
**Output:** 
+-----------+-------------+-------------------+
| player_id | player_name | grand_slams_count |
+-----------+-------------+-------------------+
| 2         | Federer     | 5                 |
| 1         | Nadal       | 7                 |
```

方法

这与 UNPIVOT 类似。但是 MySQL 没有 UNPIVOT 命令。我们采用了与 PIVOT 类似的方法。我们可以创建四个结果集，每个结果集都属于锦标赛类型，然后根据获胜者标识将它们组合起来，以获得获胜次数。

```
WITH CTE AS
(
    SELECT Wimbledon as won
    FROM Championships

    UNION ALL

    SELECT Fr_open as won
    FROM Championships

    UNION ALL

    SELECT US_open as won
    FROM Championships

    UNION ALL

    SELECT Au_open as won
    FROM Championships
)
SELECT player_id, player_name, 
    COUNT(won) as grand_slams_count
FROM Players P
INNER JOIN CTE 
ON P.player_id = CTE.won
group by player_id
```

在上面的查询中，我们对表扫描了 4 次才得到结果，我们能做得更好吗？是的。我们可以使用 when 子句，并在一次扫描中获得相同的结果。

```
WITH cte as (
  SELECT player_id, player_name,
   (SUM(case when wimbledon = player_id then 1 else 0 end ))+
    (SUM(case when Fr_open = player_id then 1 else 0 end ))+
    (SUM(case when US_open = player_id then 1 else 0 end ))+
    (SUM(case when Au_open = player_id then 1 else 0 end ))
    as grand_slams_count
  FROM championships, players
  GROUP BY player_id
  HAVING grand_slams_count !=0
)
select * from cte
```

# 问题 4

你正在经营一个电子商务网站，寻找**不平衡订单**。**不平衡订单**是指每张订单(包括其本身)的**最大**数量严格大于的**平均**数量。

订单的**平均**数量计算为`(total quantity of all products in the order) / (number of different products in the order)`。订单的**最大**数量是订单中任一单个产品的最大`quantity`。

编写一个 SQL 查询来查找所有不平衡订单的`order_id`。返回**任意顺序**的结果表。

**例 1:**

```
**Input:** 
OrdersDetails table:
+----------+------------+----------+
| order_id | product_id | quantity |
+----------+------------+----------+
| 1        | 1          | 12       |
| 1        | 2          | 10       |
| 1        | 3          | 15       |
| 2        | 1          | 8        |
| 2        | 4          | 4        |
| 2        | 5          | 6        |
| 3        | 3          | 5        |
| 3        | 4          | 18       |
| 4        | 5          | 2        |
| 4        | 6          | 8        |
| 5        | 7          | 9        |
| 5        | 8          | 9        |
| 3        | 9          | 20       |
| 2        | 9          | 4        |
+----------+------------+----------+
**Output:** 
+----------+
| order_id |
+----------+
| 1        |
| 3        |
+----------+
```

方法:我们需要首先计算每个订单 id 的平均订单数量。然后我们找到最大订货量大于所有平均订货量的订单。

```
SELECT order_id
FROM OrdersDetails 
GROUP BY order_id
HAVING max(quantity) > ALL(
   SELECT avg(quantity) FROM OrdersDetails group by order_id
)
```

上面的查询很慢，因为 Having 子句中的子查询是针对每个 order_id 执行的，并且所有关键字都检查所有平均值。在当前示例中，比较进行了 5 * 5 次。我们可以通过使用 CTE 方法来减少这种调用。

```
WITH temp as 
(
    SELECT order_id, avg(quantity) as avg_q, max(quantity) as max_q
    FROM OrdersDetails
    GROUP BY order_id
)SELECT order_id 
from temp
where max_q > (select max(avg_q) from temp )
```

查询愉快！！！