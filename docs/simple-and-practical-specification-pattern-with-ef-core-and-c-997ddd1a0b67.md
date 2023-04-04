# 简单实用的规范模式，带 EF 内核和 C#

> 原文：<https://blog.devgenius.io/simple-and-practical-specification-pattern-with-ef-core-and-c-997ddd1a0b67?source=collection_archive---------0----------------------->

![](img/eb6339383028b4f2e8815f020986a137.png)

萨曼莎·加德斯在 [Unsplash](https://unsplash.com/s/photos/simple-and-practical?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

# 摘要

这里我将介绍如何用 C#实现规范模式。提高效率的关键是使用 lambda 表达式并观察客户端评估。

## 实施规范

在[这篇文章](https://medium.com/dev-genius/common-closure-principle-the-story-of-an-evolving-architecture-6919b452c8db)中，我描述了一个酒店预订系统及其组件。另外，[在这里](https://itnext.io/specification-pattern-and-how-to-quantify-the-improved-software-stability-9a7cf5a74f1f)我解释了[规范模式](https://en.wikipedia.org/wiki/Specification_pattern)是如何改进那个架构的。在当前的故事中，我将介绍如何实现利用这种模式的查询处理程序。选择的框架是 EF Core，语言是 C#。

规范模式的主要部分是一个抽象的规范类。让我们看看如何使用 C#实现它。

现在让我们看看 C#中的查询处理程序是什么样子的。

正如您在上面的代码片段中所看到的,“GetBookingBySpecification”方法调用了 where 子句中规范的 main 方法。要尝试这样做，我们可以使用下面的类，它指定所有处于完成状态的预订。

正如所料，查询处理程序将返回所有已完成的预订。

## 查询性能问题

我们正在使用实体框架。它会将 Linq 查询翻译成 SQL 查询。如果我们[打开查询日志](https://docs.microsoft.com/en-us/ef/core/miscellaneous/logging?tabs=v3)，我们可以看到为“FinishedBookingSpecification”生成的查询如下所示。

```
SELECT b.Id, b.Status,...
FROM bookingrecord AS b
```

令人惊讶的是，我们可以看到它缺少“在哪里”的条件。原因是 EF 在将 Linq 查询转换成 SQL 查询方面有限制。EF 将在内存中运行查询的其余部分。它叫做[客户评估](https://docs.microsoft.com/en-us/ef/core/querying/client-eval)。这种查询效率很低，随着应用程序的扩展，这将成为一个问题。让我们在下一节看看如何改进这一点。

## 使用 lambda 表达式

C#有 lambda 表达式，如果在 Linq 查询中使用，可以转换成 SQL 查询。让我们看看 lambda 表达式的抽象规范是什么样子的。

让我们也使用 lambda 表达式重新实现“FinishedBookingSpecification”。

下面是我们需要如何更改查询处理程序类。

最后，如果我们运行查询并查看生成的结果，我们会看到这个。

```
SELECT b.Id, b.Status,...
FROM bookingrecord AS b
WHERE b.status = 0
```

我们已经实现了我们正在寻找的 SQL 查询。

## 客户评估

2.2 版之前的 EF Core 做客户端评估，需要更密切的关注。当在规范模式中编写表达式时，如果查询的性能很重要，有一种方法可以避免客户端评估或者至少抛出一个异常。尽管如此，在以后的版本中，默认情况下会抛出异常。

## 结论和讨论

在 C#中使用 lambda 表达式将有助于实现更高效的规范。然而，你需要留意客户的评价。

使用任何框架对框架的类型及其版本都很敏感。任何聪明的开发人员都需要时刻意识到手头框架的局限性。EF 也不例外，请确保您知道您使用的是哪个版本，并且您已经查看了文档。