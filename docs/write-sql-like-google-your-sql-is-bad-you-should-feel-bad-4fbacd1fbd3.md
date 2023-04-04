# 你的 SQL 很差(你应该感觉很差)

> 原文：<https://blog.devgenius.io/write-sql-like-google-your-sql-is-bad-you-should-feel-bad-4fbacd1fbd3?source=collection_archive---------0----------------------->

当我试图将我的第一个 SQL 提交到我的代码库中时，我收到的响应肯定比提交的代码要长，而且我多年来一直在编写(我认为是)好的 SQL。环顾其他代码的样本，我意识到我的代码是糟糕的。非常糟糕。经过反思，我意识到我没有理解可读性或一致性的价值，现在我想对数据景观产生积极的影响。

![](img/e00f61811312629a888d59733fcd1a53.png)

“唉，真的吗？格式化？格式化让编程变得如此困难"我们需要认真讨论一下这个问题。让我们来看看[谷歌如何写 SQL](https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax) 。把老狗扣上，让我们学些新把戏。

**逗号不属于字段名之前**

是的，我知道你为什么这么做。是的，很容易注释掉任何字段，包括第一个。它不直观，不可读，也不人性化。你这样做没有赢得任何“工程师”场景分，所以请停止。这使得你的代码变得混乱和难以阅读，如果你这样做，你很可能会犯下其他可读性错误。

而不是:

```
SELECT
  column_1
 ,column_2
 ,column_3
FROM my_table
```

像正常人一样写:

```
SELECT
  column_1,
  column_2,
  column_3
FROM my_table
```

像你希望的那样缩进你的代码

你写的所有代码都是同一缩进吗？你想知道什么是订单吗？请对我撒谎，说不。想象一下:你刚刚接了一个失败的可怜的开发者的项目。他们不使用缩进，你偶尔会怀疑他们的 return 键是不是在项目进行到一半时坏了，你会怀疑他们是不是故意让它不可读。你必须调试他们那一堆热气腾腾的字母汤。

而不是:

```
SELECT
column_1,
column_2,
column_3
FROM my_table my
JOIN (SELECT column_1, column_2 FROM that_table) tt 
ON my.column1=tt.column_1
```

假装你关心下一个开发者的幸福:

```
SELECT
    column_1,
    column_2,
    column_3
FROM my_table my
JOIN (
    SELECT 
        column_1, 
        column_2 
    FROM that_table
    ) tt 
    ON my.column_1 = tt.column_1
```

现在您可以看到列列表开始和结束的位置，子查询的范围，以及*的*列列表开始和结束的位置。这并不太难，是吗？

**您的代码不属于您的数据库**

想象一下:有人从会计部门发来电子邮件，说某个你从未听说过的表格不再令人耳目一新。您在 BigQuery 中搜索表；没有结果。MS-SQL Server？那里也没有。甲骨文呢？织补，真的以为它会在那里。*该表是如何被刷新的？*你大声惊叹。一个想法！构建该表的代码只需要将*和*登记到您的代码库中，对吗？因此，您转到 repo，搜索表名，然后…没有找到。没什么。甚至没有一个陈旧，过时，无用的参考。

这个故事并不牵强，到处都在发生。我喜欢听到有人抱怨人们有多爱他们喜欢的数据库，构建各种存储过程和物化视图，并使用他们的数据库进行版本控制。“只要你知道具体在哪里找，就超级容易找到，”我心里想。

如果您正在编写任何人都依赖的 SQL，看在上帝的份上，请将它签入您的代码库中，在那里它可以得到正确的版本控制。哦，你的代码上周还好好的，这周就不行了？为什么不回滚呢？您的 SQL server 存储过程中的版本控制现在怎么样了？

签入代码可以消除各种问题。它允许正确的代码提交和批准。它支持更强大的 CI/CD 体系结构。我可以继续说下去，但是所有这些关于源代码控制的讨论都让我流口水。

**停止发明你不执行的规则**

格式问题是一种瘟疫，如果不加以控制，它将会蔓延。一旦您的 SQL 开发人员真正弄清楚如何格式化他们的 SQL，这将变得很自然。为了有效地设置这种期望，需要实施您的格式化规则。起初，您可能会注意到自己在编写代码，然后重构它，直到它符合您的风格准则。在下一阶段，您将发现自己正在以一种几乎完全符合您的格式准则的方式编写 SQL。在下一阶段，你会发现你的生活更轻松，你更快乐，更年轻，有更多的时间享受你的游艇。最后，您将到达目的地:编写 SQL 的最简单方法是预先格式化，不需要额外的努力。没错；所有的可读性，没有更多的努力。你先在这里听到的。

**让你的操作符不同于那些糟糕命名的变量**

当有人阅读您的代码时(或者您在编写代码一个多小时后试图阅读您自己的代码)，很难区分操作符和变量。

从技术上讲，这是有效的 SQL:

```
select 'select' as select from from where where is not 'not'
```

任何写这个的人都应该被涂上柏油，插上羽毛，但它的语法是正确的。与其修复问题，不如让我们只是正确格式化，拍拍自己的背；这是漫长的一天。

```
SELECT
  'select' AS select 
FROM from 
WHERE where IS NOT 'not'
```

我们现在可以很容易地区分操作符和字符串名、表名和列名。

说到变量；强制统一。当使用大写操作符时，小写加下划线是最易读的，但是如果你想使用 titleCase 或 CamelCase，请自便。统一做就好。孩子们，不要把不同的名字和开车混为一谈。

**停止写清单，就像你害怕回车键一样**

你写过购物清单吗？也许是待办事项清单？我敢肯定不止几个人举手了。还记得你有 50 行要写，但单子上只有 5 项吗？现在想象你有无限多条线。为什么你把所有的清单都写在同一行？你认为你在拯救雨林吗？

当您成功地将整个列表塞进一行时，逐行查看更改有多容易？(剧透:很痛苦。)如何在列名后添加注释？

而不是:

```
# column2 represents the failure of my life's greatest ambition
select column1, column2, column3 from my_table where column1=5orselect column1, column2, column3 # column3 is sale_qty * amt
from my_table where column1=5
```

假装，哪怕只是一会儿，多一行不会让你的主机崩溃:

```
SELECT
    column1,
    column2, 
    column3 # Calculated as: sale_qty * amt
FROM my_table 
WHERE column1 = 5
```

**使用更好的列名和表别名**

一般来说，这是你第一次发挥创造力，自己写一些无意义的东西，这些东西最终会被放入你的代码库中，所以要让调试代码变得容易。

而不是:

```
# These names are unnecessarily long and unhelpful
SELECT
    column1 AS sales_per_customer_account_daily,
    column2 AS customer_daily_account_profit,
    column3 AS cumulative_units_sold_customer
FROM my_tableor # These names are too short and useless
SELECT
    column1 AS sale,
    column2 AS profit,
    column3 AS cust_sales
FROM my_table
```

问问你自己“如果我关心别人的幸福会怎么样”

```
SELECT
    column1 AS day_sales,
    column2 AS profit_customer_day,
    column3 AS cumulative_customer_unit_sales
FROM my_table
```

表别名也是如此。我们都这样做过，我们都需要一起停下来。我想象一个乌托邦式的和平照耀着我们的星球。

而不是:

```
SELECT
    column1 AS day_sales,
    column2 AS profit_customer_day,
    column3 AS cumulative_customer_unit_sales
FROM sales t
JOIN returns r
  ON t.order_id = r.order_id
```

清晰明了，让你的代码易读:

```
SELECT
    sale.column1 AS day_sales,
    sale.column2 AS profit_customer_day,
    sale.column3 AS cumulative_customer_unit_sales
FROM sales_orders_daily sale
JOIN returns_cumulative ret
  ON sale.order_id = ret.order_id
```

现在我们看到解释柱子的来源有多容易了吧？是的，这些列只存在于一个表中(sales_orders_daily ),但是避免使用别名对任何人都没有好处。好吧。也许是你的手指。心皮隧道是真实存在的。记得经常拉伸。

如果我们都同意尽我们的一份力量来采纳一个风格指南，不管你选择哪一个风格指南，你的生活会变得更容易。这是一个可以做出的有意识的决定。权力在你手中。你如何选择使用它取决于你自己。