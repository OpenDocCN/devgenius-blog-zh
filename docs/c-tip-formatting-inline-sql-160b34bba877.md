# C#提示:格式化内联 SQL

> 原文：<https://blog.devgenius.io/c-tip-formatting-inline-sql-160b34bba877?source=collection_archive---------6----------------------->

嘿，这只是我做事的方式…

![](img/1f77496db317d5802b9b1a97a960b0ec.png)

由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Sunder Muthukumaran](https://unsplash.com/@sunder_2k25?utm_source=medium&utm_medium=referral) 拍摄的照片

我们都知道格式化我们的代码对我们代码库的质量有多重要。我们从许多作者那里学到了缩进和样式的规则。在这篇文章中，我将保持简单。我将提供一些格式化内联 SQL 的基本技巧(如果你喜欢这类东西的话)。

> 前半部分将从总体上讲述 SQL，后半部分将深入探讨更多的 C#细节。

# 结构化查询语言

## 大写 SQL 关键字

SQL 是一种非常英式的查询语言。区分什么是 SQL 单词和什么是结构/数据单词是很重要的。

*坏*

```
select * from instances.productcosthistory where postedtimestamp > ‘2022–01–01’
```

*好的*

```
SELECT * FROM instances.productcosthistory WHERE postedtimestamp > '2022-01-01'
```

## 在关键字处中断

这只是帮助我们更快地定位感兴趣的查询部分。

*坏*

```
SELECT * FROM instances.productcosthistory PC JOIN instances.products P ON P.ProductId = PC.ProductId WHERE PC.postedtimestamp > '2022-01-01'
```

*好的*

```
SELECT * 
FROM instances.productcosthistory PC 
JOIN instances.products P 
ON P.ProductId = PC.ProductId 
WHERE PC.postedtimestamp > '2022-01-01'
```

## 子句缩进

有助于跟踪什么是主关键字的一部分(或者在这种情况下是 Join)

```
SELECT * 
FROM instances.productcosthistory PC 
JOIN instances.products P 
    ON P.ProductId = PC.ProductId
JOIN definitions.products PD
    ON PD.DefinitionId = P.DefinitionId
WHERE PC.postedtimestamp > '2022-01-01'
```

## 请使用方括号

这一条可能是最有争议的，但是对于数据库中定义的任何东西(例如，模式、表、列)都要使用括号。这使得在查询中识别这些对象更加容易。然而，任何临时的“变量”我一般不会放在括号里。

*不良*

```
SELECT * 
FROM instances.productcosthistory PC 
JOIN instances.products P 
    ON P.ProductId = PC.ProductId
JOIN definitions.products PD
    ON PD.DefinitionId = P.DefinitionId
WHERE PC.postedtimestamp > '2022-01-01'
```

*好的*

```
SELECT * 
FROM [Instances].[ProductCostHistory] PC 
JOIN [Instances].[Products] P 
    ON P.[ProductId] = PC.[ProductId]
JOIN [Definitions].[Products] PD
    ON PD.[DefinitionId] = P.[DefinitionId]
WHERE PC.[PostedTimestamp] > '2022-01-01'
```

## 缩进多个条件或参数

如果我们有一个 SELECT、WHERE、JOIN 或任何有多个子句的东西，添加一个换行符并执行缩进。基本上，让我们对一条信息保持 1 行。

```
SELECT 
    P.[ProductId],
    P.[Name],
    P.[Description]
FROM instances.productcosthistory PC 
JOIN instances.products P 
    ON P.ProductId = PC.ProductId AND 
       P.CreatedTimestamp = PC.CreatedTimestamp
JOIN Definitions.Products PD
    ON PD.DefinitionId = P.DefinitionId
WHERE 
    PC.PostedTimestamp > '2022-01-01' AND
    PD.Type = 0
```

这也适用于更新，

```
UPDATE [dbo].[KeyValues]
SET [Key] = 'Color',
    [Value] = 'Blue'
WHERE
    [Key] = 'Color' AND
    [Value] = 'Red'
```

## 在 Insert 语句中对齐参数

对我来说，插页有点不同。您可能要插入比选择或更新更多的字段。那么，我们真的想要一个 25 行长的 SQL 语句吗？大概不会。因此，不要将每个参数放在自己的行上，让我们在合理的水平宽度上换行，并确保参数对齐。

```
INSERT INTO [Snapshots].[Snapshots]
    ([TargetTimestamp], [CreatedTimestamp], [SnapshotType],
     [Notes], [AssociatedUserId])
VALUES
    (@TargetTimestamp, @CreatedTimestamp, @SnapshotType,
     @Notes, @AssociatedUserId)
```

这同样适用于使用 SELECT 语句插入时，

```
INSERT INTO [Snapshots].[Snapshots]
    ([TargetTimestamp], [CreatedTimestamp], [SnapshotType],
     [Notes], [AssociatedUserId])
SELECT 
    [TargetTimestamp], @CreatedTimestamp, [SnapshotType],
    'Cloned Snapshot', @AssociatedUserId
FROM [Snapshots].[Snapshots]
WHERE [SnapshotId] = 4660
```

## 用一行和一个分号分隔复合查询

这是个人偏好，但是既然你无论如何都需要分号，为什么不只是在调用之间使用它来清楚地标识每个语句呢？

```
DECLARE @Ids TABLE ([Id] INT NOT NULL)
;
INSERT INTO @Ids ([Id])
SELECT S.[SnapshotId]
FROM [Snapshots].[Snapshots] S
JOIN [Snapshots].[InventoryTransactions] IT
    ON IT.[SnapshotId] = S.[SnapshotId]
;
SELECT COUNT(*)
FROM [Snapshots].[Snapshots]
WHERE [SnapshotId] IN (SELECT [Id] FROM @Ids)
;
SELECT *
FROM [Snapshots].[Snapshots]
WHERE [SnapshotId] IN (SELECT [Id] FROM @Ids)
;
```

> **澄清**:我们实际上并不一定要使用分号(具体用 T-SQL)。大多数语句不需要它。然而，有几个这样做(即 CTE 声明)。事实是分号在 ANSI SQL-92 标准中，所以我使用它们。

## 缩进 SQL 块

当语句与一个块(即`BEGIN TRANS`或`IF`)组合在一起时，缩进该块中的所有代码。

```
SET XACT_ABORT ON;
BEGIN TRANSACTION;
    INSERT INTO [Snapshots].[Snapshots]
        ([TargetTimestamp], [CreatedTimestamp], [SnapshotType], 
         [Notes], [AssociatedUserId])
    VALUES
        (@targetTimestamp, @createdTimestamp, @snapshotType, 
         @notes, @associatedUserId)
    ;
    DECLARE @SnapshotId INT = SCOPE_IDENTITY()
    ;
    INSERT INTO [Snapshots].[InventoryTransactions] 
        ([SnapshotId], [Quantity], [ProductInstanceId],
         [InventoryLocationId])
    SELECT 
         @SnapshotId, SUM(IT.[Quantity]), IT.[ProductInstanceId], 
         IT.[InventoryLocationId]
    FROM [Transactions].[InventoryTransactions] IT
    GROUP BY 
        IT.[ProductInstanceId], 
        IT.[InventoryLocationId]
    ;
COMMIT;SELECT *
FROM [Snapshots].[Snapshots]
WHERE [SnapshotId] = @SnapshotId
;
```

> **注意**:我在这里故意把分号放在`BEGIN TRANSACTION`和`COMMIT`的末尾。这已经是一个 block 语句了，而且在视觉上已经被隔离了，所以我看不出有什么理由浪费这一行。

# C#

现在让我们进入文章的 C#部分。以上所有规则都适用，但我们现在将在 C#中展示它们，并提供一些可读性提示。

## 使用逐字@标识符

通过在创建字符串时使用[逐字标识符](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/tokens/verbatim)，您可以在 SQL 的风格上给自己更多的灵活性。我们上面展示的一切都可以在这些字符串中工作。

## 总是从新的一行开始

我觉得这一条想一想就很明显了。

## 在声明开始后缩进 SQL 一次

就像任何其他相关代码一样，在声明之后缩进。

> **额外提示**:始终将最后一个引号和分号放在单独的一行，并确保它与如上所示的声明对齐。

## 让 SQL 靠近执行代码

不要做以下事情，

这里，我们将 SQL 作为常量分隔在类的顶部。我认为人们这样做是因为这样更“干净”但是我们所做的一切都是为了让它更具可读性，这样代码就更容易理解。让它远离正在执行的代码，现在我们必须去找到它。当这些类变得很大时，试图在类的顶部和方法之间来回跳转是令人沮丧的。

相反，让我们像下面这样做，

老实说，如果不是 lambda 风格的方法，这可能更具可读性。但是，它更接近于执行，所以我可以很快看到这里发生了什么，在方法中。它在一个地方。

## 如果查询变得太复杂，就把它拆开

有时候查询是一个很难处理的问题，尤其是对于 C#代码。在我的例子中，我使用一些“提供者”来更容易地构建查询。必要时，我将利用插入的逐字组合来分解复杂的 SQL，

如果你仔细看，你会看到`{provider.JoinClause}`和`{provider.WhereClause}`。当然，这些子句也是动态的，所以这是我在没有极其复杂的查询的情况下完成任务的唯一方法。但是从可读性的角度来看，有时这是有帮助的。对我来说，说“*嘿，这部分可能很复杂，让我去看看那个特定的逻辑*”比试图在一个巨大的 SQL 语句中解析它更有帮助。

## 在有帮助的时候加入评论

有时您需要知道某些查询从哪里开始和停止，以及您在哪个部分。例如，这个查询按照不同的库存数量对其进行了分类。

除此之外(这打破了我以前的一些规则)，对你来说可能看起来更干净。以下是格式不同的完全相同的查询，

我觉得这样看起来更干净(有时候；取决于我的心情)因为它包含的代码行更少。但我会让你决定。

# 结论和签署

希望这些技巧能给你一些在 C#应用程序中编写更简洁的内联 SQL 的想法。最终，从可读性的角度来看，由您来决定什么更好。我认为当你写代码的时候，你应该试着把你的同事考虑进去。你读起来总是更容易(因为是你写的)。但你的犯罪伙伴可能不会这么想。

让我知道你的想法！请**跟随我**来到 Medium。这有助于鼓励我继续写作！

*我思故我在，一个错误的程序员……*

## 相关文章

*   [C#问题:SQL 执行器服务](/c-problem-sql-executor-service-deb459132a50)
*   [C#问题:SQL 执行器服务——依赖注入](/c-tip-sql-executor-service-dependency-injection-4455efb453b0)
*   [高级 C# — SQL Server 批量上传](/advanced-c-sql-server-bulk-upload-57ad6be6e6a1)
*   [高级 C# —通用 SQL 表结构](/advanced-c-common-sql-table-structures-bb7f7149b887)