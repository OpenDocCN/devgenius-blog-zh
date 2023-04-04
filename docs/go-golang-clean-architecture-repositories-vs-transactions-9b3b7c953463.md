# Go (Golang):干净的架构和存储库 vs 事务

> 原文：<https://blog.devgenius.io/go-golang-clean-architecture-repositories-vs-transactions-9b3b7c953463?source=collection_archive---------0----------------------->

![](img/880c8a8a71724867b0ed1c7485fa2446.png)

迪安·普在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

这篇文章有一个附带的资源库。来看看:【https://github.com/rafael-piovesan/go-rocket-ride 

# 介绍

Go (Golang)已经流行了一段时间，干净架构和存储库模式的结合也是如此。因此，人们很自然会尝试将它应用到日常场景中，并与其他堆栈和模式进行比较。也就是说，这不是另一篇旨在强调这种组织 Go 应用程序的特殊选择的来龙去脉的帖子。这一次的动机是如何让数据库事务适应这个难题，这种类型的事务跨越多个存储库，甚至必须适应对外部 API 的请求。

# 问题是

作为这次讨论的背景，我选择了 Brandur Leach 撰写的一篇文章，其中他解释了如何通过实现利用 SQL 数据库 ACID 保证的类条纹幂等键来创建健壮的 API。我强烈推荐你慢慢来，通读一遍:[https://brandur.org/idempotency-keys](https://brandur.org/idempotency-keys)

如果您不熟悉幂等键的概念，这里有一段摘自本文的引文:

> *(…)* ***幂等*** 是一个强大的概念。幂等端点可以被调用任意次，同时保证副作用只发生一次。在一个混乱的世界里，客户机和服务器可能偶尔会崩溃，或者在请求过程中连接中断，这对于使系统更能抵御故障是一个巨大的帮助。不确定请求是成功还是失败的客户端可以简单地不断重试，直到得到明确的响应。

他也是 Stripe 博客上发表的另一篇关于这个话题的文章的作者。一定要去看看:[https://stripe.com/blog/idempotency](https://stripe.com/blog/idempotency)

因此，回到手头的问题，他提出的挑战是创建一个管理“火箭之旅”的 API([Stripe 的演示应用](https://github.com/stripe/stripe-connect-rocketrides))，从客户端的角度来看，这应该是幂等的，这意味着，无论出于什么原因，如果请求在客户端从服务器获得明确响应之前失败，后续请求可以在保证它们只会改变系统状态一次的情况下进行。

# **解决方案**

考虑到主要的用例(例如，创建一个新的游乐设施)，这里是 TL；他的解决方案的博士版本:

1.  检索或创建一个新的幂等键记录(它对于任何给定的用户都应该是唯一的，但对于整个系统来说不是唯一的)
2.  创建新的游乐设备和新的审计条目记录
3.  调用 Stripe 的 API(向提供的用户信息所标识的客户收费),并使用收到的收费 ID 更新乘车记录
4.  向用户发送一个回执(这是异步的，这里我们只创建一个新的暂存作业记录，它将被随后的另一个进程使用)
5.  前面的每个步骤(1 到 4)都是在它们自己的事务中执行的
6.  前面的每个步骤(1 到 4)还会更新幂等键状态，标记执行当前步骤时到达的最后一个成功恢复点
7.  对 Stripe 的 API 的调用也是幂等的，这使得整个过程更容易实现，否则，我们将需要其他步骤来处理边缘情况
8.  在重试的情况下，在第一步，我们使用关于我们之前到达的最后一个恢复点的信息，并从那里重新开始执行(它实际上是一个状态机)

在阅读了这个简短的描述之后，不用多想，人们可能会想到以下实体: ***幂等键*** ， ***用户*** ， ***骑*** ， ***审计条目*** 和 ***分段作业*** 。此外，一个更有经验的读者/工程师可能会讨论应用 DDD 概念的优点，比如聚合(更多关于这个[这里](https://levelup.gitconnected.com/practical-ddd-in-golang-aggregate-de13f561e629))以及它们与存储库模式的结合使用(更多细节[这里](https://levelup.gitconnected.com/practical-ddd-in-golang-repository-d308c9d79ba7))，这将会对我在本文中试图描述的情况产生不同的影响。所以，我们暂时不走这条路。

此外，如前所述，步骤 1 到 4 应该在它们自己的事务中执行，也就是说，一个特定步骤中包含的*所有*动作要么应该作为一个整体成功完成，要么*所有*应该立即失败，也就是说，它们应该形成一个原子操作。正是这种保证使系统变得稳定，它是作为该解决方案基础的关键设计特性之一。

在这方面，如果我们以“步骤 2”为例，我们可以看到它实际上由 3 个动作组成:(1) *创建一个 ride* , (2) *创建一个审计条目*,( 3)*更新幂等键*。这一步将在所有 3 个操作成功执行后完成，这意味着达到了一个新的恢复点。

现在，我们来看看“步骤 3”，它的动作是:(1) *调用 Stripe 的 API* , (2) *更新 ride*,( 3)*更新幂等键*。这次的区别是外部 API 调用，一旦我们离开系统的边界，就(几乎)不可能保证原子操作。但是，对我们来说幸运的是，Stripe 的 API 还实现了幂等端点的概念，因此，如果出现任何错误，无论是在我们这边还是在他们那边，除了业务约束(例如，余额不足)，我们都可以重试请求，直到得到明确的响应。

最后，再一次，在没有进一步阐述 DDD 概念的情况下，这就是我想要解决的场景类型，或者，至少，寻找能够解决它的真实世界的实现。更具体地说，对于“步骤 2”，我们可以讨论一个应该跨越 3 个不同存储库的事务(*我知道这很长，但是请耐心等待*)，而对于“步骤 3”，则是 2 个存储库和一个外部 API 调用。

# 可能实现中的加权

## 在传输层启动事务

HTTP 动词是对可以在资源上执行的操作(GET、POST、PUT、DELETE 等)的抽象。)，人们很自然地会将 HTTP 请求视为原子操作。这就是为什么在传输层(来自 Clean Architecture 的那个)处理事务，并在其中执行所有操作是非常常见的。这里有一篇很棒的文章描述了实现这一点的一种可能的方法:[https://medium . com/we sionary-team/implement-database-transactions-with-repository-pattern-golang-gin-and-gorm-application-907517 FD 0743](https://medium.com/wesionary-team/implement-database-transactions-with-repository-pattern-golang-gin-and-gorm-application-907517fd0743)。

基本上，您需要在您的服务和存储库上提供一个方法`WithTx(tx *sql.Tx)`,这样就有可能将在传输层创建的事务句柄传播到其他层。如果你对此感到好奇，这里也有带源代码的存储库:[https://github . com/dipeshkc/Golang-Gorm-MultiLayer-d b-Transaction](https://github.com/dipeshhkc/Golang-Gorm-MultiLayer-DB-Transaction)。

我对这种方法的主要保留意见是关于实现细节的泄漏(有必要创建一个`WithTx(tx *sql.Tx)`来实际传递各层之间的句柄)，但是，更重要的是，在我们的系统上引入长时间运行的事务的风险。毫无疑问，我们应该努力避免长时间运行的操作，因为这可能会对我们的应用程序造成重大影响(*如果你去过那里，你就知道我在说什么*)，在我看来，如果没有足够重视这一点，这就是这种设计最终可能导致的结果。

## 在存储库层启动事务

当我们谈论存储库和事务时，这种设计选择可能是第一个想到的。它自然地隐藏了实现细节，使我们能够切换框架甚至数据库，而不必改变用例/领域层的任何东西。与以前的设计相反，它还刺激了细粒度和短期事务的创建。再次，互联网为我们提供了很好的文章来证明这一点，就像这篇文章:[https://dev . to/techschoolguru/a-clean-way-to-implement-database-transaction-in-golang-2ba](https://dev.to/techschoolguru/a-clean-way-to-implement-database-transaction-in-golang-2ba)。同样检查其对应的源代码回购:[https://github.com/techschool/simplebank](https://github.com/techschool/simplebank)。

我对这一特定选择的考虑是，根据情况，我们可能最终会将太多的业务逻辑推送到持久层，如下面的摘录所示(*同样，这也可以被视为很长的一段时间，但这足以说明我的观点*):

示例代码摘自:[https://github . com/tech school/simple bank/blob/master/db/sqlc/store . go](https://github.com/techschool/simplebank/blob/master/db/sqlc/store.go)

## 在用例/域层启动事务

这种方法的想法是在前面提到的所有要点之间找到平衡:

1.  创建细粒度的短期事务
2.  避免(太多)实现细节的泄露
3.  将业务逻辑/规则保持在用例/领域层
4.  提供不同框架和数据库的抽象

在我们继续之前，对于下面的例子，我使用了一个名为 [**Bun**](https://bun.uptrace.dev/) (接替 [**go-pg**](https://pg.uptrace.dev/) 的新项目)的 ORM，但这并不重要。它可以被其他 ORM 或者甚至是标准 lib 所取代(*继续阅读关于这个*的更多细节)。此外，如果您愿意，可以查看本文附带的资源库并跟随它:[https://github.com/rafael-piovesan/go-rocket-ride](https://github.com/rafael-piovesan/go-rocket-ride)。

从存储库层开始，下面是其实际实现的一部分:

这里主要介绍的是方法`Atomic(...) error`。它为我们提供了从用例/域层中创建事务的方法，但不需要传递任何事务句柄，同时也不需要暴露太多的实现细节(*让我们面对现实吧，“原子”在这个上下文中只是“事务”的同义词*)。此外，如果我们决定从 SQL 数据库切换到通常不支持事务的 NoSQL，我们必须做的唯一改变是用对所提供的回调函数的简单调用来替换它的实现(*请记住这只是过于简化，因为从 ACID 切换到最终一致性将意味着您要对整个应用程序进行更大的检查*)。

另一个关键的注意事项是，`Atomic(...) error`方法创建了存储库实现的一个新实例，并将其作为参数提供给回调函数。在作为参数传递给方法的回调函数中执行数据库操作时，应该使用这个新实例。否则，语句不会在事务内部执行。这里有一个如何使用它的例子:

注意，即使我们通过`r.store`调用了`Atomic(...)`方法，数据存储操作也应该使用作为参数传入回调函数的`ds rocketride.Datastore`实例来执行。未能正确理解这一点有时会导致一点挫折，甚至导致难以理解和意想不到的错误。这就是为什么避免这种情况并加强结构之间的区别的更好的方法是以这种方式重写上面的代码:

最后，下面是“步骤 3”的代码，我们调用 Stripe 的 API，然后用它的响应更新数据库。正如你所看到的，这里发生了很多事情，但是要指出的有趣的事情是，对外部 API 的调用不需要*需要*放在原子块内部，因为我们在我们这边创建的数据库事务不会影响远程服务器。强制性的是，接下来的两个更新应该自动发生:(1) *用费用 ID 更新 ride* ，以及(2) *用新的恢复点更新幂等键*。因此，您是否决定将 API 调用放在原子块内部，这只是一个美学问题。

# 奖金

## 明显高人一等

如果您花时间查看本文对应的 [GitHub repo](https://github.com/rafael-piovesan/go-rocket-ride) ，您可能会注意到整个系统只有一个存储库接口:

我知道这对于很多人来说可能并不理想，特别是当系统开始增长时，我们会面临一个接口包含大量方法的风险。因此，在这个概念的基础上有一个可能的改进，应该会有所帮助:

基本上，你会有多个不同的专门的存储库，但是你**不会**将它们作为直接依赖提供给用例/领域层。相反，您仍然有一个根存储库，它将作为持久性依赖注入，并为您提供每个特定存储库的`getter`方法。这样，它的职责将是实现`Atomic(...) error`方法，并将事务句柄或常规数据库连接传递给其他存储库实例。

这种方法首先出现在这里:[https://zeni das . WordPress . com/recipes/repository-pattern-and-transaction-management-in-golang/](https://zenidas.wordpress.com/recipes/repository-pattern-and-transaction-management-in-golang/)。所以，请务必查看更多详细信息。

## 使用标准库

所以，让我们举个例子，你决定从[**Bun**](https://bun.uptrace.dev/)**(一个 ORM)切换到[**sqlc**](https://docs.sqlc.dev/en/latest/index.html#)**(一个依赖于标准 lib 的代码生成工具)。下面是我们的存储库实现的样子:****

# ****结论****

****在寻找如何使用 Golang 正确地创建和管理事务以及与之相关的最佳实践时，我偶然发现了大量的博客、论坛讨论和许多其他类型的在线教程，但仍然很难找到比较可能方法的信息。因此，我决定根据我的一些发现写这篇文章，希望能帮助那些和我有同样问题的人。****

## ****参考****

1.  ****[https://brandur.org/idempotency-keys](https://brandur.org/idempotency-keys)****
2.  ****https://stripe.com/blog/idempotency****
3.  ****[https://level up . git connected . com/practical-DDD-in-golang-aggregate-de 13 f 561 e 629](https://levelup.gitconnected.com/practical-ddd-in-golang-aggregate-de13f561e629)****
4.  ****[https://level up . git connected . com/practical-DDD-in-golang-repository-d 308 c 9d 79 ba 7](https://levelup.gitconnected.com/practical-ddd-in-golang-repository-d308c9d79ba7)****
5.  ****[https://medium . com/we sionary-team/implement-database-transactions-with-repository-pattern-golang-gin-and-gorm-application-907517 FD 0743](https://medium.com/wesionary-team/implement-database-transactions-with-repository-pattern-golang-gin-and-gorm-application-907517fd0743)****
6.  ****[https://github . com/dipeshkc/Golang-Gorm-MultiLayer-d b-Transaction](https://github.com/dipeshhkc/Golang-Gorm-MultiLayer-DB-Transaction)****
7.  ****[https://dev . to/techschoolguru/a-clean-way-to-implement-database-transaction-in-golang-2ba](https://dev.to/techschoolguru/a-clean-way-to-implement-database-transaction-in-golang-2ba)****
8.  ****[https://github.com/techschool/simplebank](https://github.com/techschool/simplebank)****
9.  ****[https://zeni das . WordPress . com/recipes/repository-pattern-and-transaction-management-in-golang/](https://zenidas.wordpress.com/recipes/repository-pattern-and-transaction-management-in-golang/)****
10.  ****[https://bun.uptrace.dev/](https://bun.uptrace.dev/)****
11.  ****[https://github.com/uptrace/bun](https://github.com/uptrace/bun)****
12.  ****[https://docs.sqlc.dev/en/latest/index.html](https://docs.sqlc.dev/en/latest/index.html)****
13.  ****[https://github.com/kyleconroy/sqlc](https://github.com/kyleconroy/sqlc)****