# 使用 DDD 和 CQRS 的生态交错带构建 Laravel 应用程序

> 原文：<https://blog.devgenius.io/build-laravel-application-using-ddd-and-cqrs-with-ecotone-3e049de10a3a?source=collection_archive---------0----------------------->

![](img/3d505a62cfb2227b972e84214e3e917a.png)

DDD 在 PHP 世界变得越来越受欢迎，有很多关于如何接近它的讨论。
在 Laravel 社区,[也获得了发展势头](https://twitter.com/PovilasKorop/status/1517034905682812929)。

你是否一直想知道如何在 Laravel 中实现它？
在本文中，我将解释 DDD 构建模块的基础知识，以及我们如何在 Laravel 应用程序中使用它们。

这不是必需的，但是我鼓励你在开始之前看一下之前关于 CQRS 的博客[“用 PHP 走进 CQRS”](https://blog.ecotone.tech/cqrs-in-php/)。

# 雄辩的奥姆和 DDD

在 DDD，我们使用*领域模型*，这不同于*雄辩模型。*
领域模型与基础设施和框架相关的实现是干净的，以便对修改和扩展有完全的控制。

> 领域模型旨在解决与业务相关的问题，而不是技术问题。得益于此，维护和扩展它们变得更加容易。

如果我们决定使用雄辩术并将其与领域模型混合，那么我们就引入了数据库依赖。然而，我们这样做是有原因的，因为我们想获得雄辩的整合带来的好处。

> 对抗自己的框架可能会给代码库带来更高的复杂性，并增加可维护性成本。

对于构建没有雄辩的领域模型，将会有一个单独的博客文章，在这篇文章中，我们将关注，如果我们决定与雄辩的 ORM 携手共进，我们能实现什么。

![](img/aeafeb6b83ab5e909a70653838d09c10.png)

# 聚集

在 DDD，我们建造`Aggregates which are classes rich in behaviour`。
以`Article`为例，它可能是`published`，但首先需要作者指定为`approved`。

这可能是雄辩的模型，我们将使用`->save()`方法存储在数据库中。
公开集合上的动作(方法)并把它作为修改数据的唯一方式是很重要的。
直接在数据库上调用 insert 和 updates SQLs，可能会引入不正确的数据或绕过我们的约束。

> 如果修改给定集合的唯一方法是通过他的公共方法，并且方法在内部保护它自己的状态，那么我们就到了我们的模型总是有效的地方。

# 价值对象

模型的一部分是值对象。VO 是不可变的类。它们是需要验证的给定类型的包装器。

如果系统中有像电子邮件这样的值对象，那么我们可以传递它并确保它总是有效的。这减少了系统中保护逻辑的数量。

# 实现与拉勒韦尔的生态交错

要使用 Laravel 安装启用生态区:

```
composer require ecotone/laravel
```

对于自动序列化和反序列化:

```
composer require ecotone/jms-converter
```

# 雄辩创造新的聚合

我们将实现`Issue`聚合。如果出现系统错误，我们的客户可以报告问题。
先来定义一下`ReportIssue Command`。

为了执行`ReportIssue`，我们需要调用`Command Bus`。

什么来自`$request->all()`从`HTML Form`返回的数据是:

```
["email" => "johnybravo@gmail.com","content" => "My PC is not working"]
```

`ReportIssue`里面的`Email`是值对象类，所以我们需要知道如何从`string`转换到`Email`类。为了做到这一点，我们将注册`Converter`。

> Ecotone JMS 转换器包将处理从数组/ JSON / XML 到 PHP 类的所有反序列化以及其他反序列化。

让我们实现我们的雄辩`Issue Aggregate`:

1.  我们用`#[Aggregate]`标记我们的模型，这样生态交错区可以找到它
2.  我们在`"issue.report"`路由键下启用工厂方法。
3.  因为我们想要发布包含`Id`的事件，我们需要将`Issue`保存在工厂方法中，所以将分配标识符
4.  我们用来自`WithAggregateEvents` trait 的`recordThat`方法发布`IssueWasReported`。
5.  我们需要公开带有生态区标识符的公共方法`#[AggregateIdentifierMethod("id")]`

现在我们可以执行我们的`Controller we have defined previously to send Command and create new Issue`。

# 改变总发行

现在，我们可以增加结束问题的可能性。

> 没有为命令处理程序定义命令类，这是因为我们在这里不需要任何数据。当需要时，生态区允许仅通过路由名称无缝路由给定的处理者。

现在我们想执行这个命令处理程序

`metadata`部分是额外的信息，不是命令本身的一部分。
生态区用它来传递周围框架相关的信息。
经过`aggregate.id`我们告诉生态区哪个`Issue Aggregate it should execute`。

> 您可以使用元数据来传递您自己的额外细节，您想通过命令传递这些细节，例如`User Id`。

# 事件处理

当`Issue`聚合被报告时，我们正在发布事件:

我们可以注册订户，在报告问题后，订户将向客户发送确认电子邮件。

到`subscribe to specific Event we type hint given class as first parameter`(就像命令处理程序一样)并用`#[EventHandler]`标记方法。

# 摘要

`Ecotone Framework`正在处理胶合代码并提供构建模块，因此我们可以快速可靠地构建应用程序。它以设计这些框架方式与 Laravel 和雄辩集成。
在需要的情况下，您可以使用新功能扩展您的应用程序，如[事件源](https://blog.ecotone.tech/implementing-event-sourcing-php-application-in-15-minutes/)和[异步通信](https://blog.ecotone.tech/asynchronous-php/)。

如果你想用`Laravel and Ecotone`运行演示应用，你可以在[这个库](https://github.com/ecotoneframework/php-ddd-cqrs-event-sourcing-symfony-laravel-ecotone)下找到它。