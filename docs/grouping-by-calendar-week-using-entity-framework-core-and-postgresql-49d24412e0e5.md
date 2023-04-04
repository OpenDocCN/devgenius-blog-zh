# 使用实体框架核心和 PostgreSQL 按日历周分组

> 原文：<https://blog.devgenius.io/grouping-by-calendar-week-using-entity-framework-core-and-postgresql-49d24412e0e5?source=collection_archive---------17----------------------->

![](img/4398a1da75ee2db7fd04a59b242c76e7.png)

在本文中，我们将研究如何使用自定义数据库函数映射的实体框架特性，按日历周(或日期的其他部分，如按年或按月)对数据库行进行分组。

本文中展示的代码示例假设您正在 PostgreSQL 数据库之上使用实体框架核心，使用的是 [Npgsql。PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL/) 包，但是一般的方法也适用于其他数据库。

让我们假设你正在实现你自己的博客平台，并想提供一个功能，让你看到每年有多少博客文章被写。下面的代码将允许您直接从数据库中获取这种聚合统计数据:

上面显示的代码是可行的，因为 Npgsql 附带了一个定制的数据库函数映射，它将把第`12`行的`GroupBy`子句转换成使用 PostgreSQL `date_part`函数，只从`PublishedOn`列中提取年份。你可以在这里看到做这个[的代码。](https://github.com/npgsql/efcore.pg/blob/4f2da8baabc6c5d28e6a16d14878aa954972b9b0/src/EFCore.PG/Query/ExpressionTranslators/Internal/NpgsqlDateTimeMemberTranslator.cs#L107)

不幸的是，如果你也想按日历周分组，那你就不走运了。一个原因是用于列的`DateTimeOffset`类型没有日历周的属性——您必须使用 [ISOWeek 类](https://docs.microsoft.com/en-us/dotnet/api/system.globalization.isoweek.getweekofyear?view=net-6.0#system-globalization-isoweek-getweekofyear(system-datetime))来代替。第二个原因是 Npgsql 没有为此提供定制的数据库函数映射。因此，如果不添加一些自定义代码，您将需要从数据库中加载所有帖子，并在内存中进行分组——讨厌！

幸运的是，添加自定义数据库函数映射很容易。

上面显示的代码向 DataContext 类添加了一个类型化的 C#方法，并将其映射到 PostgreSQL 数据库提供的`date_trunc`函数。第 16 行中 C#方法的实现并不重要——它从未真正执行过，因为实体框架会将它的任何用法转换成直接在数据库中执行的 date `date_trunc`函数。同样，代码也映射了`date_part`函数。

一旦我们做到了这一点，我们就可以利用这些方法以一种类型安全的方式按日历周进行分组:

这段代码现在使用了我们上面定义的`DateTrunc`和`DatePart`方法:

*   映射到`date_trunc`函数的`DateTrunc`将返回每周星期一的日期，因此我们通过第 4 行的`GroupBy`调用得到的分组键将是代表这些星期一的`DateTimeOffset`
*   第 5–11 行中的`Select`语句现在将从这些星期一中提取`Year`(利用 Npgsql 提供的函数映射)，并使用映射到`date_part`函数的`DatePart`方法提取日历周数。

瞧，您现在有了一种按日历周对数据库中的数据进行分组的类型安全的方法。

觉得这篇文章有用？在 Medium 上关注我( [Christian Eder](https://medium.com/@christian.johann.eder) )并查看我下面关于 Azure、作为代码的基础设施和物联网的文章！不要忘记👏这篇文章分享一下吧！

*   [使用 Newtonsoft。ASP.NET 的 JSON Core 和 SignalR](https://medium.com/@christian.johann.eder/using-newtonsoft-json-in-asp-net-core-and-signalr-55b0fa4645aa)
*   [使用 CDK 将 ASP.NET 核心 API 部署到 AWS Fargate](https://awstip.com/deploying-an-asp-net-core-api-to-aws-fargate-using-cdk-dab10bef51a1)
*   [消费者驱动的契约测试——使用 Azure 函数托管模拟 API](https://medium.com/@christian.johann.eder/consumer-driven-contract-testing-hosting-a-mocked-api-using-azure-functions-a42d634c47fd)