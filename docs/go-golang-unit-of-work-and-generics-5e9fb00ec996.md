# Go (Golang):工作单位和仿制药

> 原文：<https://blog.devgenius.io/go-golang-unit-of-work-and-generics-5e9fb00ec996?source=collection_archive---------0----------------------->

![](img/b47c70dacdb3f808ab4b6ed4da8bff32.png)

[Max Duzij](https://unsplash.com/@max_duz?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

这是我之前关于跨多个存储库的事务的故事的延续，所以一定要查看它，以便阅读更多关于这个主题的内容: [Go (Golang):干净的架构&存储库对比事务](/go-golang-clean-architecture-repositories-vs-transactions-9b3b7c953463)

这也是 GitHub 回购的源代码:[https://github.com/rafael-piovesan/go-rocket-ride](https://github.com/rafael-piovesan/go-rocket-ride)

# 介绍

[之前](/go-golang-clean-architecture-repositories-vs-transactions-9b3b7c953463)中，我讨论了处理必须由多个存储库共享的数据库事务的不同方法。熟悉工作单元模式的人通常都知道这个问题，根据 Martin Fowler 的说法:

> 维护受业务事务影响的对象列表，并协调更改的写出和并发问题的解决。

此外，这是一个非常普遍的模式。NET 和 Java+Hibernate 社区。所以，我在这里的重点不是引入任何新概念，而是提出一个如何在 Go (Golang)中实现它的实用方法。对于好奇的读者来说，关于它背后的理论有很好的来源(查看最后的参考资料)。话虽如此，我不会试图详细解释它，也不会宣扬它的优点和/或缺点，同时试图将其应用于现实世界的应用中。

最后一条信息，对于下面的代码示例，我正在使用 [Bun(一种 Golang ORM)](https://bun.uptrace.dev/) 。确保检查它的文档，以防不太清楚。

# 给我看看密码

这是自我上一篇文章以来的第一个变化，术语(类型和变量被重命名，以直接引用工作单元模式)。这是我的队友们的一些很好的反馈和有价值的贡献后的结果(你会在这里看到的部分代码是他们的想法)。

这里的要点是方法`Do(context.Context, UnitOfWorkBlock) error`如何创建一个新事务，将它传递给`UnitOfWorkStore`实例中的新存储库，最后，调用调用者代码提供的回调函数。

`UnitOfWorkStore`仅保存对存储库的引用，如下所示:

最后，将模式付诸实践(*明白了吗？因为它是一个工作单元…是的，很蹩脚，对此表示抱歉*):

# 太好了，但是它有什么新的吗？

细心的读者可能已经意识到，除了重命名一些内容和简化部分代码之外，这个实现与我以前的文章基本相同。这是真的。自从 Go 1.18 发布以来，随着泛型的引入，主要的变化发生在存储库实现上(阅读更多:[https://go.dev/doc/tutorial/generics](https://go.dev/doc/tutorial/generics))。这里有一个“之前与之后”的对比:

好了，不同之处在于，`AuditRecord`接口现在是另一个接口`data.ICRUDStore`的组合，除此之外，后者是一个通用接口，而不是实现在数据库上持久化实体的实际过程。因此，当我们调用`NewAuditRecord(...)`时，我们得到的是`AuditRecord`的一个实例，而这个实例又是一个`data.ICRUDStore[entity.AuditRecord]`。

不错！但是这能做什么呢？让我们来看看我们与由`data.New[T](...)`返回的类型相关联的实际实现:

它的最大好处是简化了前面的代码，通过为所有的库提供一个单一的实现，并且仍然保持编译器检查我们传递给它们的方法的所有参数类型的安全性(当我们有一个接受类型`interface{}`的参数的函数时，这是主要的问题)。

回到前面的例子，只是为了强调所有东西是如何组合在一起的:

# 结论

将我的代码的第一个版本与最新版本进行比较，我不认为这些变化是一个巨大的突破，但它们确实提高了可读性和简单性，这反过来有助于可维护性。尽管如此，玩泛型还是很有意思的，希望能启发人们寻找更多关于如何以及在哪里使用它们的例子。

# 参考

1.  [本文 GitHub 资源库](https://github.com/rafael-piovesan/go-rocket-ride)
2.  [企业应用架构的模式](https://www.martinfowler.com/eaaCatalog/)
3.  [在 ASP.NET MVC 应用程序中实现存储库和工作单元模式](https://docs.microsoft.com/en-us/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application)
4.  [教程:泛型入门](https://go.dev/doc/tutorial/generics)