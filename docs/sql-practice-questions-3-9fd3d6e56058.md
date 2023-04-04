# SQL 练习题— #3

> 原文：<https://blog.devgenius.io/sql-practice-questions-3-9fd3d6e56058?source=collection_archive---------1----------------------->

SQL 练习题的第三部分

点击此处查看[第一部](/sql-practice-questions-1-800ed65d99b2) [第二部](/sql-practice-questions-2-f1116b1f5402) [第四部](/sql-practice-questions-4-e24a6bdb32d4) [第五部](/sql-practice-questions-5-456cfb41757a)

# 问题 1

一家公司想雇佣新员工。公司的工资预算是`$70000`。公司的招聘标准是:

1.  继续雇佣薪水最低的毕业生，直到你不能再雇佣更多的毕业生。
2.  用剩下的预算雇佣工资最低的大三学生。
3.  继续雇佣薪水最低的初级员工，直到你不能雇佣更多的初级员工。

编写一个 SQL 查询来查找在上述标准下雇佣的高年级学生和低年级学生的 id。返回**任意顺序**的结果表。

**示例:**

```
**Input:**
Candidates table:
+-------------+------------+--------+
| employee_id | experience | salary |
+-------------+------------+--------+
| 1           | Junior     | 10000  |
| 9           | Junior     | 15000  |
| 2           | Senior     | 20000  |
| 11          | Senior     | 16000  |
| 13          | Senior     | 50000  |
| 4           | Junior     | 40000  |
+-------------+------------+--------+
**Output:** 
+-------------+
| employee_id |
+-------------+
| 11          |
| 2           |
| 1           |
| 9           |
+-------------+
```

方法:从问题中可以明显看出，我们希望首先雇佣高年级学生，所以我们得到了所有我们能雇佣的高年级学生，然后用剩余的预算雇佣低年级学生，直到我们完全耗尽预算。我们将使用 CTE 来存储雇佣的高年级学生，然后使用它来雇佣低年级学生。

```
with seniorhired as
(
  select * 
   from 
     (  
       select employee_id, sum(salary) over(order by salary ) as run_sum
       from candidates 
       where experience ='Senior'
     ) y 
  where run_sum<=70000
),
juniorhired as
(
  select employee_id
  from
      (  select employee_id, sum(salary) over(order by salary) run_sum
         from candidates 
         where experience ='Junior'
      ) z 
  where run_sum<(select ifnull(70000-max(run_sum), 70000) from seniorhired )
)
select employee_id from seniorhired
union all
select employee_id from juniorhired
```

## 非递归解

```
WITH CTE AS (
    SELECT 
        employee_id, 
        experience, 
        SUM(salary) OVER(PARTITION BY experience ORDER BY salary, employee_id ASC) AS run_sum
    FROM Candidates
)

SELECT 
    employee_id
FROM CTE 
WHERE experience = 'Senior' AND run_sum <= 70000
UNION
SELECT 
    employee_id
FROM CTE 
WHERE experience = 'Junior' AND RN < (SELECT 70000 - IFNULL(MAX(RN),0) FROM CTE WHERE experience = 'Senior' AND run_sum <= 70000)
```

# 问题 2

给定一个包含不同科目学生分数的表，反转该表，使每一行包含不同列中的每个科目的学生及其分数。

**示例:**

```
**student_marks Table**
+-------------+------------+--------+
| student_id  | subject    | marks  |
+-------------+------------+--------+
| 1001        | English    | 88     |
| 1001        | Science    | 90     |
| 1001        | Maths      | 85     |
| 1002        | English    | 70     |
| 1002        | Science    | 80     |
| 1002        | Maths      | 83     |
+-------------+------------+--------+
**Output**:
+-------------+----------+---------+-------+
| student_id  | English  | Science | Maths |
+-------------+----------+---------+-------+
| 1001        | 88       | 90      | 85    |
| 1002        | 70       | 80      | 83    |
+-------------+----------+---------+-------+
```

方法:由于 MySQL 没有 PIVOT，我们可以利用 CASE 语句来查找所需的表。我们为每个主题创建列，如果列名与主题匹配，则选择标记值，否则我们赋零。

```
+-------------+----------+---------+-------+
| student_id  | English  | Science | Maths |
+-------------+----------+---------+-------+
| 1001        | 88       | 0       | 0     |
| 1001        | 0        | 90      | 0     |
| 1001        | 0        | 0       | 85    |
| 1002        | 70       | 0       | 0     |
| 1002        | 0        | 80      | 0     |
| 1002        | 0        | 0       | 83    |
+-------------+----------+---------+-------+
```

这个中间结果将具有与输入相同的行，我们对每个 student_id 的行进行分组和组合。

```
SELECT student_id,
 max(CASE WHEN subject='English' THEN marks else 0) as English,
    max(CASE WHEN subject='Science' THEN marks else 0) as Science,
    max(CASE WHEN subject='Maths' THEN marks else 0) as Maths
FROM student_marks 
GROUP BY student_id
```

# 问题 3

编写一个 SQL 查询来报告购买了产品 **"A "和" B"** 但没有购买产品 **"C"** 的客户的 customer_id 和 customer_name，因为我们想推荐他们购买该产品。返回由`customer_id`命令的结果表**。**

**例如:**

```
**Input:** 
Customers table:
+-------------+---------------+
| customer_id | customer_name |
+-------------+---------------+
| 1           | Daniel        |
| 2           | Diana         |
| 3           | Elizabeth     |
| 4           | Jhon          |
+-------------+---------------+
Orders table:
+------------+--------------+---------------+
| order_id   | customer_id  | product_name  |
+------------+--------------+---------------+
| 10         |     1        |     A         |
| 20         |     1        |     B         |
| 30         |     1        |     D         |
| 40         |     1        |     C         |
| 50         |     2        |     A         |
| 60         |     3        |     A         |
| 70         |     3        |     B         |
| 80         |     3        |     D         |
| 90         |     4        |     C         |
+------------+--------------+---------------+
**Output:** 
+-------------+---------------+
| customer_id | customer_name |
+-------------+---------------+
| 3           | Elizabeth     |
+-------------+---------------+
```

方法:简单的方法是创建三个不同的 CTE，每个都将产品 A、B 和 C 作为采购，然后使用 IN 和 NOT IN 应用过滤器。但是这将需要 3 次表扫描(分别针对 A、B 和 C)。我们可以通过使用 **group_concat** 来一次性完成。

```
+---------------+----------------+---------------+
| customer_id   | customer_name  | product_name  |
+---------------+----------------+---------------+
| 1             | Daniel         | A,B,C,D       |
| 2             | Diana          | A             |
| 3             | Elizabeth      | A,B,D         |
| 4             | Jhon           | C             |
+---------------+----------------+---------------+
```

一旦我们有了上面的结果，我们只需要找到模式 **%A，B%** 而不是 **%A，B，C%** 。

```
with cte as (
    select o.customer_id , c.customer_name, 
        group_concat(o.product_name order by o.product_name) as purchases
    from Orders o left join Customers c
    on o.customer_id = c.customer_id
    group by o.customer_id
)
select customer_id , customer_name from cte
where purchases like '%A,B%' and purchases not like '%A,B,C%'
```

`**group_concat**`函数中的 **Order by 子句并不重要，因为它确保了产品模式的顺序。如果我们不使用 order by，那么 where 子句将被修改如下**

```
WHERE purchases LIKE '%A%'
AND purchases LIKE '%B%'
AND purchases NOT LIKE '%C%'
```

# 问题 4

一家银行希望绘制一张图表，显示银行访问者在一次访问中完成的交易数量，以及在一次访问中完成该交易数量的相应访问者数量。

编写一个 SQL 查询来查找有多少用户访问了银行但没有进行任何交易，有多少用户访问了银行并进行一次交易，等等。

结果表将包含两列:

*   `transactions_count`即一次访问中完成的交易数量。
*   `visits_count`这是在一次银行访问中做了`transactions_count`的相应用户数。

`transactions_count`应该取一个或多个用户从`0`到`max(transactions_count)`的所有值。返回按`transactions_count`排序的结果表。

**示例:**

```
**Input:** 
Visits table:
+---------+------------+
| user_id | visit_date |
+---------+------------+
| 1       | 2020-01-01 |
| 2       | 2020-01-02 |
| 12      | 2020-01-01 |
| 19      | 2020-01-03 |
| 1       | 2020-01-02 |
| 2       | 2020-01-03 |
| 1       | 2020-01-04 |
| 7       | 2020-01-11 |
| 9       | 2020-01-25 |
| 8       | 2020-01-28 |
+---------+------------+
Transactions table:
+---------+------------------+--------+
| user_id | transaction_date | amount |
+---------+------------------+--------+
| 1       | 2020-01-02       | 120    |
| 2       | 2020-01-03       | 22     |
| 7       | 2020-01-11       | 232    |
| 1       | 2020-01-04       | 7      |
| 9       | 2020-01-25       | 33     |
| 9       | 2020-01-25       | 66     |
| 8       | 2020-01-28       | 1      |
| 9       | 2020-01-25       | 99     |
+---------+------------------+--------+
**Output:** 
+--------------------+--------------+
| transactions_count | visits_count |
+--------------------+--------------+
| 0                  | 4            |
| 1                  | 5            |
| 2                  | 0            |
| 3                  | 1            |
+--------------------+--------------+
```

方法:为了找到想要的结果，我们首先需要找到每个用户和访问日期完成了多少交易。我们需要使用左/右连接，因为我们可能会在没有访问的情况下发生事务。

```
+---------+------------------+-------+
| user_id | visit_date       | trans |
+---------+------------------+-------+
| 1       | 2020-01-01       | 0     |
| 1       | 2020-01-02       | 1     |
| 1       | 2020-01-04       | 1     |
| 2       | 2020-01-02       | 0     |
| 2       | 2020-01-03       | 1     |
| 7       | 2020-01-11       | 1     |
| 8       | 2020-01-28       | 1     |
| 9       | 2020-01-25       | 3     |
| 12      | 2020-01-01       | 0     |
| 19      | 2020-01-03       | 0     |
+---------+------------------+-------+
```

一旦我们有了每个用户每天的事务计数，我们就可以用它来找出这些事务需要多少次访问。第一个 CTE 只是生成从 0 到 max 发生的事务数。

```
WITH trans_count as (
    SELECT row_number() over () as rn
    FROM transactions 
    UNION
    SELECT 0
),
t as (
    SELECT coalesce(count(t.amount), 0) as trans
    FROM visits v LEFT JOIN transactions t 
    ON v.user_id = t.user_id 
       AND v.visit_date = t.transaction_date
    GROUP BY v.user_id, v.visit_date
),SELECT rn as transactions_count, coalesce(sum(rn = trans), 0) as visits_count
FROM t RIGHT JOIN trans_count ON trans = rn
WHERE rn <= (SELECT max(trans) FROM t)
GROUP by rn
ORDER by transactions_count
```

查询愉快！！！