# SQL 练习题— #5

> 原文：<https://blog.devgenius.io/sql-practice-questions-5-456cfb41757a?source=collection_archive---------16----------------------->

SQL 练习题的第五部分

点击此处查看[第一部分](/sql-practice-questions-1-800ed65d99b2) [第二部分](/sql-practice-questions-2-f1116b1f5402) [第三部分](/sql-practice-questions-3-9fd3d6e56058) [第四部分](/sql-practice-questions-4-e24a6bdb32d4)

问题 1

编写一个 SQL 查询来报告电影院中所有连续的可用座位。按升序返回由`seat_id` **排序**的结果表**。测试用例的生成使得两个以上的座位连续可用。**

**例如:**

```
**Input:** 
Cinema table:
+---------+------+
| seat_id | free |
+---------+------+
| 1       | 1    |
| 2       | 0    |
| 3       | 1    |
| 4       | 1    |
| 5       | 1    |
+---------+------+
**Output:** 
+---------+
| seat_id |
+---------+
| 3       |
| 4       |
| 5       |
+---------+
```

**方法:**我们需要检查两行是否是连续的并且都是空闲的，我们可以通过使用

```
SELECT DISTINCT a.seat_id
FROM cinema a INNER JOIN cinema b
ON abs(a.seat_id - b.seat_id) = 1
  AND a.free = 1 and b.free = 1
ORDER BY a.seat_id
```

# 问题 2

编写一个 SQL 查询来查找应该从 Leetflex 中禁止的帐户的`user_id`。一个帐户应该被禁止，如果它在某个时刻从两个不同的 IP 地址登录。返回**任意顺序**中的结果表。

**例 1:**

```
**Input:** 
LogInfo table:
+---------+------------+---------------------+---------------------+
| user_id | ip_address | login               | logout              |
+---------+------------+---------------------+---------------------+
| 1       | 1          | 2021-02-01 09:00:00 | 2021-02-01 09:30:00 |
| 1       | 2          | 2021-02-01 08:00:00 | 2021-02-01 11:30:00 |
| 2       | 6          | 2021-02-01 20:30:00 | 2021-02-01 22:00:00 |
| 2       | 7          | 2021-02-02 20:30:00 | 2021-02-02 22:00:00 |
| 3       | 9          | 2021-02-01 16:00:00 | 2021-02-01 16:59:59 |
| 3       | 13         | 2021-02-01 17:00:00 | 2021-02-01 17:59:59 |
| 4       | 10         | 2021-02-01 16:00:00 | 2021-02-01 17:00:00 |
| 4       | 11         | 2021-02-01 17:00:00 | 2021-02-01 17:59:59 |
+---------+------------+---------------------+---------------------+
**Output:** 
+------------+
| user_id |
+------------+
| 1          |
| 4          |
+------------+
```

**方法:**我们需要为相同的 user_id 找到两行，它们在登录和注销时间上重叠。我们通过在相同的 user_id 和不同的 IP 上使用 self join 来实现，然后检查它们的登录和注销时间之间的重叠。

```
SELECT distinct l1.user_id
FROM LogInfo l1
INNER JOIN LogInfo l2
ON l1.user_id = l2.user_id and l1.ip_address != l2.ip_address
WHERE (l2.login <= l1.logout and l2.login >= l1.login) OR 
    (l2.logout <= l1.logout and l2.logout >= l1.login)
```

# 问题 3

编写一个 SQL 查询来报告所有**经理**的 id 和姓名、直接向他们报告**的员工数量**，以及四舍五入为最接近整数的报告的平均年龄。返回由`employee_id`排序的结果表。

**例 1:**

```
**Input:** 
Employees table:
+-------------+---------+------------+-----+
| employee_id | name    | reports_to | age |
+-------------+---------+------------+-----+
| 9           | Hercy   | null       | 43  |
| 6           | Alice   | 9          | 41  |
| 4           | Bob     | 9          | 36  |
| 2           | Winston | null       | 37  |
+-------------+---------+------------+-----+
**Output:** 
+-------------+-------+---------------+-------------+
| employee_id | name  | reports_count | average_age |
+-------------+-------+---------------+-------------+
| 9           | Hercy | 2             | 39          |
+-------------+-------+---------------+-------------+
```

**方法:**我们需要对 reports_to 执行 Grroup by，然后执行 self join 来查找员工的 nmae。

```
SELECT e1.employee_id, e1.name, 
    count(e1.employee_id) as reports_count, 
    round(avg(e2.age)) as average_age
FROM employees e1 INNER JOIN employees e2
ON e1.employee_id = e2.reports_to
GROUP BY e1.employee_id
ORDER BY e1.employee_id
```

# 问题 4

编写一个 SQL 查询来查找至少连续出现三次的所有数字。返回**任意顺序**中的结果表。

**例 1:**

```
**Input:** 
Logs table:
+----+-----+
| id | num |
+----+-----+
| 1  | 1   |
| 2  | 1   |
| 3  | 1   |
| 4  | 2   |
| 5  | 1   |
| 6  | 2   |
| 7  | 2   |
+----+-----+
**Output:** 
+-----------------+
| ConsecutiveNums |
+-----------------+
| 1               |
+-----------------+
```

**方法:**这个问题要求连续三次，所以如果检查 prev、cur 和 next 是否相等，我们就能找到解决方法。我们可以使用`LEAD & LAG`函数来实现同样的功能。

```
WITH cte as (
    SELECT num, 
        LEAD(num, 1) OVER() AS next_num, 
        LAG(num) OVER (ORDER BY id) AS prev_num
    FROM `Logs`
)
SELECT DISTINCT num as ConsecutiveNums 
FROM cte WHERE num=next_num and num=prev_num;
```