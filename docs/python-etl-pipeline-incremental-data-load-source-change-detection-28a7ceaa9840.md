# Python ETL 管道:增量数据加载源变化检测

> 原文：<https://blog.devgenius.io/python-etl-pipeline-incremental-data-load-source-change-detection-28a7ceaa9840?source=collection_archive---------2----------------------->

![](img/2776e7c5438045e50509ed8eeecb8364.png)

源变化检测

我们将继续使用 ETL 增量数据加载方法。ETL 中的增量数据加载(提取、转换和加载)是理想的设计模式。使用这种方法，我们可以处理最少的数据，使用更少的资源，从而减少时间。在[上一篇文章](/python-etl-pipeline-the-incremental-data-load-techniques-20bdedaae8f)中，我们讨论了各种增量加载方法，包括源变化检测、目标变化比较和变化数据捕获(CDC)。我们在该会话中实现了目标更改比较。今天我们将讨论源代码变更检测。在源代码变更检测设计模式中，我们使用两个关键字段*修改于*和*创建于*日期时间字段来检测变更。我们将数据拉入自上次 ETL 运行以来新的和/或修改过的 ETL 管道。这确实需要额外的 set 来存储 ETL 日志，以确定上一次 ETL 运行的时间。

完整代码可在 [GitHub](https://github.com/hnawaz007/pythondataanalysis/tree/main/ETL%20Pipeline) 上获得。视频教程可以在 YouTube 上找到。

**设置**

我们将在源系统(SQL Server)中设置几个表。一个*客户*和*事实 _ 交易*表。我们将在*客户*表中存储客户详细信息，在*事实 _ 交易*表中存储与这些客户相关的交易。让我们继续在这些表中插入一些记录。

在目标端，在 Postgres 中，我们将创建一个 *etl* 模式。在这个模式中，我们创建了一个存储 ETL 日志的表。

![](img/bee797d4774ffb9c73bf65263534e6c2.png)

创建 ETL 模式

这个表的目的是存储 ETL 执行日期、表名和一些其他细节。每当我们的 ETL 脚本运行时，它将从这个日志表中获取最后的 ETL 运行日期，我们将根据 *create_at* 和 *modified_at* date 将这个日期提供给 where 子句。确保向将在该模式上运行您的 ETL 脚本的用户授予权限。我将授予 *etl* 用户对 *etl* 模式的权限。所有的 SQL 脚本都可以在 GitHub 上获得。

![](img/86e838f0f41946452ef6efff60fda88e.png)

ETL 日志表

我们可以在 SQL server 中针对*客户*表构建一个查询，在该查询中，我们对该表中的 *created_at* 和 *modified_at* 字段进行过滤。为了挑选新的和更改的记录，我们将在这两个过滤器之间使用*或*子句。这将为我们提供该表中的新记录和更改记录。对于其他表，我们也可以遵循这种模式。

```
USE AdventureWorksDW2019
GO
SELECT *
FROM [dbo].[customers]
where ( modified_at >= convert(datetime2,'2022-03-30 16:51:13.362179') **OR**
created_at >= convert(datetime2,'2022-03-30 16:51:13.362179'))
```

**实现源变化检测**

让我们利用之前 [ETL 管道](/how-to-build-an-etl-pipeline-with-python-1b78407c3875)会话中的代码来定义带有数据库细节的变量，并在 Jupyter Notebook 中建立数据库连接。为了在 etl 模式中插入 ETL 日志，我们将创建一个函数。该函数接受 logid、表名、行数、状态和错误消息。我们用这些信息构建一个字典，并添加带有当前日期时间的日期。为了将这些信息插入数据库，我们将其转换为数据帧，并使用“to_sql”函数将其保存到数据库中。

将 ETL 日志插入日志表

让我们定义另一个函数，通过表名从这个表中获取最后的 ETL 运行日期。我们查询表名等于 provided table 的日志表，并选择 max last extract date-time 值。如果没有匹配，那么我们将日期时间设置为“1900–01–01”。此函数返回最后提取的日期时间值。

获取上次 ETL 运行日期

我们使用前面建立的 *AdventureWorks* 数据库中的客户表。我们将数据读入一个带有 pandas 的 DataFrame，使用 SQL 和过滤器 created_at 和 modified_at 字段，最后提取 datetime。

**处理上插**

为了插入和更新数据，我们创建了一个 upsert 函数。我们检查最后提取的日期时间值是否为“1900–01–01”。如果这个条件为真，那么我们将所有数据从源加载到目的地。此外，我们在 ETL 日志表中插入一行带有当前时间戳的数据。我们的两个环境现在是同步的。

将数据插入目标

在 else 子句中，我们构建了一个在目标表中向上插入行的新函数。此时，我们可以在 customer 表中再次加载相同的数据，不会出现任何问题。我们将最终得到同一个客户的两条记录。为了避免这个问题，并使用 Postgres 中的“*冲突时插入*”功能，我们在表上添加了一个唯一约束。现在，我们可以一次性处理新的和修改过的记录。让我们插上。

```
**ALTER TABLE public.stg_customer
 ADD CONSTRAINT stg_customer_uq UNIQUE ("customerId");**
```

我们将使用与上次类似的功能。它接受一个数据帧，带有修改过的记录、一个表名和表的主键。我们将数据帧存储在一个临时表中，并构建一个动态 SQL *INSERT ON CONFLICT* 语句。我们在目标表中插入行，如果客户 Id 已经存在，那么在*冲突*时，我们用传入的数据更新现有的行。

动态上插功能

让我们在源表中再记录几条记录。此外，更新一条记录，这样我们就有了新的和修改过的行。

我们再次运行源查询，从源系统中提取变更。这给了我们新的数据和修改后的数据。我们用数据框、表名和键列调用 upsert 函数。该函数使用来自源的新数据插入并更新表行。我们已经成功地更新了目标表。

**结论:**

*   我们描述了什么是源变更检测 ETL 方法？以及可用于实现增量数据加载的不同方法。
*   我们展示了使用 Pandas 在 ETL 管道中实现目的地变化比较是多么容易。
*   我们使用 Python、Pandas、SQL Server 和 PostgreSQL 在 ETL 管道中实现了增量加载方法。
*   完整的代码可以在[这里找到](https://github.com/hnawaz007/pythondataanalysis/tree/main/ETL%20Pipeline)