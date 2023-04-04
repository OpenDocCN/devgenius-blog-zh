# 在这种编程模式下…

> 原文：<https://blog.devgenius.io/in-that-programming-rut-65600125e010?source=collection_archive---------9----------------------->

![](img/97d9aaebcf3224c0875aa1a6abc72b57.png)

由[杰米·哈根](https://unsplash.com/@dearjamie?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

你是否曾经陷入编程的窠臼，发现自己为每个项目都以同样的方式编写代码；作为一名工程师，你觉得自己没有学习，甚至没有拓展自己的能力？

我确实有。

事实上，我觉得我现在很单调。这就像我的实验风格的个性已经消失了，每天都可以在我的代码中看到。我承认，经过 20 多年的经验和试验，我已经对软件设计和架构(尤其是商业应用程序)有了一个相当坚实的方法。然而，它确实开始让你怀疑你是否已经达到了极限，或者你只是在你的能力中找到了一个舒适的确定水平。

不久前我写了一篇名为[掌握软件的真正方法](https://medium.com/dev-genius/the-true-way-to-software-mastery-64dc0bf8a2f8)的文章，其中我谈到了成为软件大师的想法。虽然，我觉得我已经赢得了我在软件设计和架构领域的同事们的尊重，但是你永远也不能确定。事实上，我们不应该拿今天的自己和其他人比较，而应该拿今天的自己和昨天的自己比较。所以当我把今天的自己和昨天的自己比较时，没有太大的变化…这就是惯例。

# 我现在的车辙(。NET C#)

我发现自己为每个项目一遍又一遍地做着同样的事情。以下是我似乎反复重复的一些项目。

## 接口设计

我构建了我所能构建的最简单的接口，并使它们总是局限于 singleton(从阿迪的角度来看)。我甚至会不遗余力地去做这件事。举个例子，

```
public interface IWorkflowContextAccessor
{
    IWorkflowContext Current { get; }
}
```

这个接口类似于`IHttpContextAccessor`，允许我从任何其他类中获取这个上下文。然而，对于一个 ASP.NET 应用程序，我可以像注入一个作用域依赖一样简单地注入`IWorkflowContext`,然后动态地确定它。

**我的推理**:单例范围的依赖可以注入到所有的 DI 依赖范围(瞬态、范围和单例)。但是反过来就不对了。对我来说，这降低了记忆哪些服务可以被注入到哪些类中的认知负荷。

因此，函数需要的所有东西都必须通过方法参数传递，并限制有状态类的使用，这很有用。

## 扩展一切

我有一个习惯，用 C#为所有东西创建扩展方法。我使用最小的接口设计使新的实现更容易，同时允许更复杂的逻辑存在于其他地方。

下面是我可能创建的一个示例存储库接口:

```
public interface IRepository<T> where T : Model
{
   Task UpsertAsync(T model);
   Task DeleteAsync(Identifier id, Identifier? scopeId = null);
   Task<T[]> FindAsync(ExactMatchQuery query);
}

public abstract class Model
{
   public Identifier Id { get; set; } = Identifier.Empty;
   public Identifier ScopeId { get; set; } = Identifier.Empty;
   public string Name { get; set; } = string.Empty;
   public string Description { get; set; } = string.Empty;
   public DateTime CreatedTimestamp { get; set; } = DateTime.UtcNow;

   public Dictionary<string, string> Properties { get; set; } = new Dictionary<string, string>();
}

public class ExactMatchQuery
{
    public Identifier? Id { get; set; }
    public Identifier? ScopeId { get; set; }
    public string? Name { get; set; }
    public string? Description { get; set; }

    public DateTime? MinimumCreatedTimestamp { get; set; }
    public DateTime? MaximumCreatedTimestamp { get; set; }

    public int Offset { get; set; } = 0;
    public int Limit { get; set; } = 10;
}
```

然后我会到处使用扩展来做非常具体的工作:

```
public static class RepositoryFindExtensions
{
    Task<T?> FindByIdAsync<T>(this IRepository<T> repository, Identifier id, Identifier? scopeId = null) where T : Model
    {
        var found = await repository.FindAsync(new ExactMatchQuery
        {
            Id = id,
            ScopeId = scopeId
        };

        return found.FirstOrDefault();
    }
}
```

**我的理由**:如果你适当地抽象，你可以用简单的接口做很多事情，而不必将它们直接嵌入接口本身。这使得未来的实现具有更少的需求，从而允许这些扩展在任何实现中重用。

但是现在逻辑在主存储库类之外。如果没有像 Visual Studio 或 VSCode 这样的好的 IDE，那么祝你好运。这种分离可能会让初级开发人员感到困惑。

## 装饰大厅的装饰物

还有一种叫做装饰者的设计模式，我发现自己一直在使用它。我喜欢动态添加功能的能力，所以我创建了接口层，以允许添加新代码，而不是更改旧代码。

```
public interface ISqlExecutor
{
    Task<T> ExecuteAsync<T>(Func<ISqlConnection, ISqlTransaction, T> execute);
}
```

那么我至少有两个实现。一个用于我的主执行者，一个用于性能监控。当然，在一个典型的场景中，我通常会有更多。

```
public sealed class SqlServerExecutor : ISqlExecutor
{
    public SqlServerExecutor(...) { }

    public async Task<T> ExecuteAsync<T>(Func<ISqlConnection, ISqlTransaction, T> execute)
    {
        using (var sqlConnection = new SqlConnection(...))
        {
            await sqlConnection.OpenAsync();
            using (var sqlTransaction = sqlConnection.BeginTransaction())
            {
                var results = await execute(sqlConnection, sqlTransaction);
                sqlTransaction.Commit();
                return results;
            }
        }
    }
}

public sealed class PerformanceMonitoringSqlExecutor : ISqlExecutor
{
    private readonly ISqlExecutor _innerExecutor;

    public PerformanceMonitoringSqlExecutor(ISqlExecutor inner)
    {
        _innerExecutor = inner ?? throw new ArgumentNullException(nameof(inner));
    }

    public async Task<T> ExecuteAsync<T>(Func<ISqlConnection, ISqlTransaction, T> execute)
    {
        var watch = StopWatch.StartNew();
        var results = await _innerExecutor(execute);
        watch.Stop();

        // TODO: Log these results somewhere

        return results;
    }
}
```

**我的理由**:这允许我根据配置或其他方法(即 A/B 测试)动态地建立我的需求。这样，我可以避免每个实现中的直接特性标志和臃肿的代码，如日志语句。此外，当新的实现出现时，我不一定需要将这些代码复制到每个实现中。

但是现在代码更加复杂，很难推理，尤其是如果你看到的只是核心业务逻辑中的接口。你怎么知道有东西被记录了呢？有很多样板文件可以让年轻的开发人员容易阅读。

# 解药？

据我所知，我上面所做的一切都不一定是坏的或糟糕的实践。这真的取决于你工作的环境。然而，我这么做已经很多年了；所有新项目都是如此。我来到这里是因为那些年的实验。我尝试了各种技术，去掉了那些让事情变得更糟的技术。但是“更糟”是什么意思呢？

## 爆发

要打破常规，你首先需要知道你作为程序员的价值观是什么。你看重的是什么？以及你如何判断你自己的代码(你的度量标准是什么；无论主观还是客观)？

我有自己的一套衡量标准来评估这些情况:

*   **可读性**:现在和将来负责编写这段代码的团队是否理解这段代码，在哪里找到它，以及它是如何工作的？如果没有，还需要做更多的工作。
*   **可维护性**:我能最大限度地减少这段代码被触碰的次数吗？例如，通过使用 decorators，我可以消除更改实现代码的需要。相反，我可以用一种附加的方式为新功能创建新的实现。
*   **可重用性**:这段代码被抽象了多少？我可以用我生成的代码最小化我未来的样板代码吗？是不是在一个库中，我可以用于未来的项目，在未来的开发中节省时间。
*   **灵活性**:当需求改变时，如果有的话，需要改变多少代码？可以使用特征标志(或者装饰符)吗？或者我可以创建一个简单的实现来替换当前的实现吗？通过创建扩展点，当不可避免地添加或更改特性时，我提供了灵活性。

当然，还有更多，但这些是我的首要目标。我相信在最近几个月，我已经停止问自己这些问题，并导致这种车辙形成。

现实情况是，软件永远不会在所有这些方面同时完美。在平衡这些软件质量时，需要进行权衡。因此，我对这些品质的优先级的衡量会影响我的代码的发展方向。接下来的问题是，我赋予这些品质的优先级或权重是什么？

我真的没有这个问题的答案，但是我相信对于任何一个有身份的开发者来说，他们必须首先定义他们想要达到的软件质量。然后，他们必须优先考虑这些品质，以优化他们的标准。这就是我所说的你的**程序员身份**。这种身份被用来衡量你和你自己，以及潜在的雇主。我认为，这是程序员被雇主拒绝的时候。如果组织和开发人员的身份没有明显的重叠，那么程序员注定会在他/她的职位上失败。不是因为任何技巧上的原因，而是因为他对软件固有的个性。

我会继续寻找，找到摆脱这种困境的方法。谢谢你倾听我的咆哮。如果你有什么建议，尽管提出来。

下次见！