# 及时查询 SQL Server 时态表简介

> 原文：<https://blog.devgenius.io/a-query-in-time-introduction-to-sql-server-temporal-tables-145ddb1355d9?source=collection_archive---------0----------------------->

![](img/613e5c11836ddc680c64a7b9014c778c.png)

在设计数据库模式时，版本控制往往是一个问题。有许多考虑和事情可能会出错。你必须弄清楚如何创建历史记录，最好是自动创建。您必须弄清楚如何一起和分别查询正常记录和历史记录。您需要确定如何处理回滚。更不用说向现有的模式添加版本控制的额外麻烦了。

幸运的是，SQL Server 有一个特性可以解决我们的许多问题:时态表。

# 时态表？

[微软的文档](https://docs.microsoft.com/en-us/sql/relational-databases/tables/temporal-tables?view=azuresqldb-current)很好地总结了什么是时态表:

SQL Server 2016 引入了对时态表(也称为系统版本时态表)的支持，作为一种数据库功能，它提供了内置支持，可提供关于存储在表*中的任意时间点*的数据的信息，而不仅仅是当前时刻正确的数据。时态是 ANSI SQL 2011 中引入的一项数据库功能。

这里重点强调的是，时态表允许您在时间查询表**。此外，时态表是“系统版本化”的，这意味着您不需要担心手动管理版本，并且它实际上是一个标准的 [ANSI SQL 特性](https://en.wikipedia.org/wiki/SQL:2011)，因此它在其他数据库系统中也受支持。在这篇文章中，我们将重点关注 SQL Server，但是这些概念应该可以转移。**

在 SQL Server 中创建时态表时，会在后台创建历史表。主表包含当前时间点存在的记录，历史表包含记录的所有先前版本。您可以正常查询主表，也可以在查询中添加时态子句来查找历史记录。

![](img/229d50dffb6e73a337d881f4c0924e7c.png)

[*显示 SQL Server 如何自动保存旧版本记录的图表。*](https://docs.microsoft.com/en-us/sql/relational-databases/tables/temporal-tables?view=azuresqldb-current)

# 创建时态表

首先让我们考虑一个普通的表，我们可以用它作为我们的示例主表。对于这篇文章，我们将使用一个基本的个人记录:

```
create table dbo.Person
(
    [PersonId] int not null primary key clustered,
    [Name] nvarchar(max) not null,
    [Email] nvarchar(max) null,
    [Address] nvarchar(max) null,
    [PhoneNumber] nvarchar(max) null
)
```

该表不是时态的，但是有了与之比较的正常定义将突出我们需要使其时态的内容:

```
create table dbo.Person
(
    [PersonId] int not null primary key clustered identity(1,1),
    [Name] nvarchar(max) not null,
    [Email] nvarchar(max) null,
    [Address] nvarchar(max) null,
    [PhoneNumber] nvarchar(max) null, [SysStartTime] datetime2 generated always as row start hidden not null,
    [SysEndTime] datetime2 generated always as row end hidden not null,
    Period for system_time (SysStartTime, SysEndTime)
)
with (system_versioning = on (history_table = Person_History))
```

主要有两个变化。首先，我们添加 SysStartTime 和 SysEndTime 作为生成的列，然后用它来创建 Period 列。这些是时态表的必需列。隐藏开始和结束时间列是可选的，但在不需要时有助于隐藏版本控制。其次，我们在语句末尾添加了 with (system_versioning = on (…))。这将使用上面定义的 Period 列创建历史表。历史表的默认命名有点混乱，所以我们还定义了该表的名称。我喜欢 _History 后缀，所以我们将在这里使用它。

SQL Server Management Studio 告诉我们该表现在是系统版本，并将历史表嵌套在主表下，这很有帮助:

![](img/dbed01a7dbb51c5f0137864c3ccbf675.png)

*显示人员表及其嵌套历史表的 SSMS 对象资源管理器。*

现在我们已经创建了一个历史表，我们可以像平常一样在 Person 表中插入、更新和删除数据。SQL Server 将在后台更新历史表。

# 使现有表成为临时表

将普通表转换成时态表非常简单。让我们从普通的表定义开始，添加必要的列，然后启用版本控制:

```
create table dbo.Person
(
    [PersonId] int not null primary key clustered identity(1,1),
    [Name] nvarchar(max) not null,
    [Email] nvarchar(max) null,
    [Address] nvarchar(max) null,
    [PhoneNumber] nvarchar(max) null
)
goalter table dbo.Person 
    add [SysStartTime] datetime2 generated always as row start hidden not null
            constraint DF_SysStart default sysutcdatetime(),
        [SysEndTime] datetime2 generated always as row end hidden not null
            constraint DF_SysEnd default convert(datetime2, '9999-12-31 23:59:59.9999999'),
        Period for system_time (SysStartTime, SysEndTime)
goalter table dbo.Person set (system_versioning = on (history_table = dbo.Person_History))
go
```

您可以看到，启用版本控制与创建启用版本控制的表非常相似。

还可以使用 alter table<tablename>set(system _ versioning = off)来禁用表上的版本控制。这将使历史表成为一个普通的表，如果您想完全删除版本控制，可以将其删除。</tablename>

# 修改时态表中的数据

在时态表中插入、更新或删除记录并不需要特别考虑。执行这些操作将导致在历史表中自动创建新记录。

您不能(也不应该)修改历史表中的数据，如果 Period 列是隐藏的(实际上不必隐藏),那么任何现有的查询和语句都将正常工作。您甚至可以[改变主表的模式](https://docs.microsoft.com/en-us/sql/relational-databases/tables/changing-the-schema-of-a-system-versioned-temporal-table?view=azuresqldb-current)，历史表将自动更新以匹配。

让我们运行一些语句，向历史表中添加一些数据:

```
insert into Person (Name, Email, Address, PhoneNumber) values ('Jeff Winger', 'jwinger@example.com', '1233 Main St', '555-555-5555')
insert into Person (Name, Email, Address, PhoneNumber) values ('Abed Nadir', 'anadir@example.com', '55 W East Rd', '555-555-5555')
insert into Person (Name, Email, Address, PhoneNumber) values ('Shirley Bennet', 'sbennet@example.com', '809 E South Ave', '555-555-5555')
insert into Person (Name, Email, Address, PhoneNumber) values ('Troy Barnes', 'tbone@fake.com', '55 W East Rd', '555-555-5555')
insert into Person (Name, Email, Address, PhoneNumber) values ('Annie Edison', 'aedison@example.com', '55 W East Rd', '555-555-5555')
insert into Person (Name, Email, Address, PhoneNumber) values ('Pierce Hawthorne', 'phawthorne@example.com', '123 N Fake Blvd', '555-555-5555')
insert into Person (Name, Email, Address, PhoneNumber) values ('Britta Perry', 'bperry@example.com', '9985 Main St', '555-555-5555')
```

现在让我们看看我们的桌子是什么样子:

![](img/82af1b488efa7fbefabe50f9793a6109.png)

*SSMS 查询显示第一次插入语句后的结果。*

请注意，特定于历史的列是隐藏的。在处理主表时，我们根本不需要处理它们。现在让我们更新一些记录，这样我们就可以开始使用历史表了:

```
update Person set PhoneNumber = '555-123-4567' where PersonId = 1
update Person set Email = 'jwinger@wingerlaw.com' where PersonId = 1
update Person set PhoneNumber = '555-999-2222' where PersonId = 2
update Person set Address = null where PersonId = 6
-- Pause for a few minutes to make later queries easier.
update Person set Email = 'sbennet@greendalecc.com' where PersonId = 4
-- Pause for a few minutes to make later queries easier.
update Person set PhoneNumber = '555-222-9999' where PersonId = 2
update Person set Email = 'abed@troyandabed.com' where PersonId = 2
update Person set Email = 'troy@troyandabed.com' where PersonId = 4
-- Pause for a few minutes to make later queries easier.
insert into Person (Name) values ('Ben Chang')
-- Pause for a few minutes to make later queries easier.
delete from Person where PersonId = 8
```

![](img/0b89e92edc4a1efc0301137899731f18.png)

*SSMS 查询显示一些更新语句后的结果。*

# 查询时态表

查询历史表有两种主要方式。第一种方法是通过在查询中添加基于时间的子句来查看表的以前版本。第二种方法是手动查看历史表，这样可以看到所有以前版本的记录。

![](img/84da2f113778f3139f5c7a6c85b803d8.png)

[*显示 SQL Server 在查询中使用时态子句时如何组合主表和历史表中的数据的图表。*](https://docs.microsoft.com/en-us/sql/relational-databases/tables/temporal-tables?view=azuresqldb-current)

# 按时间查询

[基于时间的原因](https://docs.microsoft.com/en-us/sql/relational-databases/tables/querying-data-in-a-system-versioned-temporal-table?view=azuresqldb-current)可以添加到其他正常查询中，以使用保存的版本。这允许您做一些事情，比如查看某个时间点的表是什么样子，或者查看特定时间范围内的记录是什么样子。

您可以使用 for system_time <clause>添加这些子句，其中子句可以是:</clause>

*   截至<datetime></datetime>
*   从<datetime>到<datetime></datetime></datetime>
*   在<datetime>和<datetime>之间</datetime></datetime>
*   包含在(<datetime>，<datetime>)中</datetime></datetime>
*   全部

利用这些，你可以在你的桌子上进行“时间旅行”。让我们看一些例子。“截止日期”查询获取在某个时间点存在的记录:

```
select *
from Person
for system_time as of '2021-02-05 18:25:00'
order by PersonId
```

该查询返回原始的表内容(这里使用的日期时间大约是我第一次插入后的一分钟)。

![](img/7a2b5bc6bd1801a82413363b889c4d02.png)

*显示“截止日期”查询和结果的屏幕截图。*

使用其他种类的子句，您可以查看给定时间段内记录的更改。from … to 和 between … and 查询将返回在定义的时间段内存在的记录，而 contains in …将返回在该时间段内仅在存在的行。

```
select *
from Person
for system_time from '2021-02-05 18:25:00'
                to '2021-02-05 18:30:00'
order by PersonId
```

![](img/13ac434104aeaffb2929de254807b60d.png)

*显示“发件人”查询和结果的屏幕截图。*

请注意，时态查询可以为同一个主键返回多条记录。这可能会使一些应用程序出错。

# 直接历史访问

查询可以像访问主表一样访问历史表(也就是说，它遵循主表上的任何安全规则)。这意味着您可以像查询任何其他表一样查询历史表。这有时是查询旧版本记录的有用方法。

请注意，历史表只包含旧的版本的记录，而不是每个记录的最新版本。如果需要包含这两者，请在主表的查询中使用 from system_time all。

```
select *
from Person_History
where PersonId = 2
order by SysStartTime, SysEndTime
```

![](img/1e8c0de1fd66ab7101d4e38cf315b545.png)

*显示历史表查询和结果的屏幕截图。*

请注意，历史表包括开始和结束时间列。即使我们将它们设置为在主表上隐藏，它们在历史表上也是可见的。

# 考虑

对于时态表，首先需要考虑的是数据库的大小。版本控制功能强大，但会占用更多空间。有[种不同的解决方案](https://docs.microsoft.com/en-us/sql/relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables?view=azuresqldb-current),您可以实现来控制使用的空间和保留的历史数量。我的首选方法是对保留期设置一个限制，这样历史记录只能追溯到以前:with(system _ versioning = on(history _ retention _ period = 6 个月)。对于您的应用程序来说，这可能不是完美的解决方案，因此请仔细阅读控制保留的方法，并实施最适合您的选项。

另一个考虑是，与记录版本的交互主要是一个手动过程。如果您想要执行比较或回滚，您的应用程序必须实现这些功能。SQL Server 负责版本控制的最难的部分，即保存历史表，但剩下的就看您自己了。请注意，并不是所有的 ORM 库都支持时态表，所以如何将它们集成到应用程序中会因堆栈的不同而不同。

总而言之，时态表是 SQL 的一个强大特性，可以减轻版本控制的痛苦。它可能不是在每种情况下都完美，但当您需要实现数据的历史和版本控制时，它可以让您以一种简单明了的方式开始。

## 引文

“时态表— SQL Server。”*时态表— SQL Server | Microsoft Docs* ，Docs . Microsoft . com/en-us/SQL/关系数据库/表/时态表？view=azuresqldb-current。

## ——安德鲁·莫斯卡迪诺，软件开发者 [AWH](http://awh.net) 。我们正在帮助企业通过技术推动增长。