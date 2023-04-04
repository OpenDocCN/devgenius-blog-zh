# 请让我们更好地利用日志消息

> 原文：<https://blog.devgenius.io/help-me-help-you-maintain-your-app-please-2cfcfe22b66?source=collection_archive---------6----------------------->

我们能否在早期就关注软件的一些核心方面，而不是等到最后？

![](img/6cedd2aa4f769c085b8776d363b99582.png)

亚历克斯·莫托克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

你有没有遇到过这样的情况，你的老板让你看一些 5 年前由一个不再在公司工作的开发人员写的代码并解决一个问题？你是否注意到，10 次中有 9 次，弄清楚如何调试应用程序、找到日志并理解它们占用了你 80%的时间？然后你必须解决这个问题？

是啊。这种事在我身上发生了一次又一次…

所以我开始思考为什么会出现这种情况。为什么每个应用程序都需要我们花费几个小时去理解其内部工作的基本原理？为什么它似乎发生在我们自己编写的应用程序中？我的意思是，我已经忘记了我自己的东西是如何工作的，不记得日志在哪里，如何查看它们，以及阅读模糊的、无意义的条目。回到我自己的代码上，意识到我已经忘记了一个模糊的错误消息意味着什么，这有点尴尬。

然而，多年来的经验告诉我一些事情，当调试我们的代码并在应用程序的整个生命周期中维护它时，如何使我的生活以及其他生活变得更容易。

# 维护优先编码

所以我不会提出一些新发现的编码“范式”，比如“测试驱动开发”或类似的东西。我想说的是，也许我们应该关注如何提前维护我们的代码。我们的策略是什么？这是一个非常关键的概念，感觉在游戏中被强加的太晚了。这里有一些问题是我们在从事绿地项目时应该尽早问自己的。

*   应用程序中的异常会在哪里被捕获？
*   对于应用程序的用户来说，异常是如何被捕获或冒泡的？
*   信息将被记录在我们系统的什么地方？它只是日志文件还是控制台？我们将提供什么类型的信息？
*   当问题出现时，将提供什么类型的背景？
*   谁将是问题的第一响应者？我们有办法给他们提供足够的信息来自我纠正吗？

系统内的问题总是不可避免地发生。有人会在你的应用程序中发现一个错误。我可以保证。即使你完美地编写了它，有人也会编造一个“逻辑”错误，因为他们给了你糟糕的需求…我见过这种情况发生。因此，我们必须准备好追踪这些问题，并尽快纠正它们。

# 维护目标

那么，当我们谈论维护应用程序时(至少从应用程序外部的角度来看)，我们应该瞄准的真正目标是什么呢？

## 1.系统内发生的任何问题都应该立即从消息中了解。

当出现问题时，我们应该能够准确地知道哪里出了问题以及在哪里修复它。举个例子吧，

如果您在中使用 SQL Server。NET，您会经常看到这样的错误:

> "建立与服务器的连接时，出现了一个错误。当连接到 SQL Server 时，此故障可能是由于 SQL Server 在默认设置下不允许远程连接造成的。(提供程序:命名管道提供程序，错误:40-无法打开到 SQL Server 的连接)。Net SqlClient 数据提供程序)。

作为一名开发者，你很清楚这意味着什么，对吗？它基本上是说(很可能)连接字符串不正确。这也可能意味着有一个防火墙的问题，但让我们假设你的网络家伙得到了正确的。

如果有人在日志中看到这一点，他们也可以很快确定这一点。但是，他们可能不知道如何纠正这个问题。例如，他们可能不知道连接字符串存储在`appsettings.json`或`environment variable`中。所以现在他们四处寻找连接字符串。如果异常看起来更像这样不是更好吗:

```
try {
  // ... Sql Connection Code ...
} catch (SqlException ex) when (ex.Number == 53) {
  // https://learn.microsoft.com/en-us/previous-versions/sql/sql-server-2008-r2/cc645611(v=sql.105)
  throw new InternalSqlException(@"
    Could not connect to Sql Server. 
    Please check the connection string within the 'appsettings.json' file 
    (Key: ConnectionStrings:Primary)", ex
  );
}
```

这告诉我在哪里寻找连接字符串，以及检查什么。这样完美吗？不。但是它为用户提供了一点上下文，以便在返回到开发人员之前检查他们的设置和配置。

我喜欢使用的另一个常见例子是必需配置。很多时候，在一个设置文件中可以有连接字符串、凭据、功能标志等配置。根据配置的类型，我们可能想要验证(在启动时)配置是否正确。

假设我们想要启用面部识别功能。它需要定义一个双置信水平。我们希望在启动之前确保它被正确定义，否则我们会把一切都搞砸。我喜欢这样做，以便在部署过程中，我们可以立即知道是否有问题。

```
public class FacialRecognitionFeature {
  public bool Enabled { get; set; } = false;
  public double? ConfidenceLevel { get; set; } = null;
}

public static class ConfigurationValidation {
  public static void Validate(FacialRecognitionFeature? feature) {
    if (feature?.Enabled == false)
      return;

    if (feature.ConfidenceLevel == null)
      throw new ValidationException(@"
        Facial Recognition Confidence Level is required.
        The confidence level must be between 0.01 and 0.99.
        (Key: Features:FacialRecognition:ConfidenceLevel)
      ", "Features:FacialRecognition:ConfidenceLevel");

    if (feature.ConfidenceLevel < 0.01 || feature.ConfidenceLevel > 0.99)
      throw new ValidationException(@"
        The confidence level must be between 0.01 and 0.99.
        (Key: Features:FacialRecognition:ConfidenceLevel)
      ", "Features:FacialRecognition:ConfidenceLevel");
  }
}
```

我认为这比简单地说一些像`throw new ArgumentException("Incorrect value for the Confidence Level")`或一些简单的对等词要好得多。它没有告诉你去哪里找或者值应该是什么。

现在你可能会说，“咄，科尔斯顿，这是显而易见的。”但是我似乎从来没有找到像这样的代码来帮助未来的开发人员和维护人员最大限度地调试启动问题。老实说，这并不容易。嗯……这很容易，但是我们往往很懒，对吗？我们把它拖得太久，直到有人开始有问题时才开始思考。我想有些人觉得没有必要提前做。但是如果你能预测未来需要什么，我们就不会有这个问题了…那么为什么不现在就做呢？

## 2.(观点)异常应该在离开系统之前立即被捕获。

这个可能有点主观，但是我们开始吧。

我认为，除了与外部用户(或其他系统)的交互之外，任何地方的异常处理都是多余的。例如，我强烈反对在类库中捕捉异常。只有少数特殊情况，我认为这是可以的，也是必要的，但不在正常范围内。相反，它们应该在用户或外部系统的交互点被捕获。

在#1 中，我展示了这一点。我捕捉到了一个异常，但立即将其重新抛出。这是规则的例外之一(没有双关语的意思)。我没有捕捉、记录异常，也没有告诉堆栈发生了什么？不。我在捕捉它，然后和更多的信息一起传递。这样，我仍然让顶层接口捕捉异常。

举个例子，

```
// Windows Forms
private void btnSubmit_Click(object sender, MouseClickEvent e) {
  try {
    // ... Button Logic ...
  } catch (Exception ex) {
    Logger.LogError(ex);
  }
}

// ASP.NET Core
[HttpPost("api/v1/submissions")]
public IActionResult SubmitForm([FromBody] Submission submission) {
  try {
    // ... Controller Logic ...
  } catch (Exception ex) {
    Logger.LogError(ex);
  }
}
```

为了清楚起见，我展示这些例子是为了澄清问题。但是，我通常会将这个 try-catch 部分抽象成公共处理程序，在可能的情况下，可以通过属性或全局处理程序共享这些处理程序。这当然假设它不会使应用程序不必要的崩溃。

这样做的主要原因是我们可以维护堆栈跟踪。如果你在堆栈中上下抛接，如果操作不当，你“可能”会丢失信息。另外，在我看来，您必须担心在太多的地方进行异常处理，这样才会有效。将您的异常处理集中在一小部分代码上，您可以确保以一种通用的方式进行处理。

## 3.结构化日志是你的朋友

有时例外是没有足够的信息。尤其是当您没有捕捉到生成上下文的异常时。例如，进入 SQL 语句的参数可能会导致“截断”错误，但是您不知道是哪一个。那么，我们如何捕捉这种环境，并以一种有用的方式将其添加到我们的日志中呢？

我讨厌看到的一件事是填满日志并且从未被使用过的无关错误消息(你知道到处都有类似`LogInformation`的语句)。我宁愿在我需要的时候显示我需要的信息。这需要日志记录的两个方面:

1.  捕捉上下文
2.  将上下文添加到异常日志

如果使用 Serilog，#2 相当简单，

```
Logger.LogError("Error Occurred: {Exception}.{NewLine}Context: {@Context}", 
  ex.ToString(), ContextData);
```

这将显示异常的完整堆栈跟踪和上下文对象的 JSON 解释，假设使用了 JsonFormatter。相当有用。

但是，#1 有点难。我认为有三种方法可以解决这个问题:

1.  使用内置的`Logger.BeginScope`。网芯。这使得任何`Log{Level}`语句都可以记录额外的数据。然而，它只在范围内起作用，所以冒泡并不真正起作用。另外，只有某些日志提供者支持它。
2.  使用一个定制的异常来冒泡这些信息。您可以向自定义异常中的字典添加上下文，以便它可以到达您的外部 catch 语句。然而，这意味着打破我的第二条规则，即只在一个地方抓。你需要接住球，然后再扔出去。
3.  环境上下文处理程序…

那么，我们如何解决这个问题呢？不幸的是，3 号确实是最好的方法。我们必须创建一个定制的(限定了作用域的)处理程序，在函数调用的整个生命周期中维护参数和上下文信息，然后在作用域结束时销毁它。这样做的缺点是，它只能在 web 应用程序中“轻松”地工作。对于其他类型，您必须在自己体内建立这种 DI 机制。

下面的例子是不完整的，但是可以提供我正在谈论的一个想法，

```
public interface IFunctionContext {
  void Add(string key, object value);
  IReadonlyDictionary<string, object> Context { get; }
}

public void ConfigureServices(IServiceCollection services) {
  // Will leave the InMemoryFunctionContext implementation to your imagination
  services.AddScoped<IFunctionContext, InMemoryFunctionContext>();
}

public class BasicController {
  private readonly IFunctionContext _Context;

  public BasicController(IFunctionContext context) {
    _Context = context;
  }

  public IActionResult Post(Submission model) {
    try {
      _Context.Add("Submission", model);
      // ... Business Logic...
    } catch (Exception ex) {
      Logger.LogError("Error: {Exception}{NewLine}Context: {Context}",
        ex.ToString(), _Context.Context);
    }
  }
}

public class SqlService {
  private readonly IFunctionContext _Context;

  public SqlService(IFunctionContext context) {
    _Context = context;
  }

  public DataObject RunQuery(Parameters parameters) {
    _Context.Add("SqlParameters", parameters);
    // ... SQL Query ...
    return data;
  }
}
```

你可以很快看到这会变得多么乏味。不完全是，理想的解决方案是什么？但是，我希望您也能看到这种方法的价值。通过提供更多的上下文，错误实际上意味着更多，缩短了解决问题的时间。否则，您可能不得不关闭生产数据库，打开 Visual Studio，启动调试器，添加一些断点，等等。等等。然而，如果您有这种类型的上下文，这些都是不必要的。假设您记录了该信息，那么这可能就像在数据上添加适当的检查一样简单。

但是话说回来，您还必须担心您记录的是什么类型的数据。您的参数中是否包含 PII 数据、密码或其他敏感数据？现在你也必须对此保持警惕。

有一些方法可以简化和集中这些逻辑，这样就不必在应用程序中编织一张巨大的网。但这最好在另一篇文章中讨论。无论如何，这必须经过深思熟虑，特别是如果您有一个独立于开发团队的取证团队，帮助跟踪产品中的问题。

# 结论

最终，我在这篇文章中的目标是这样说的，

> 把一些焦点放在你如何写日志消息，在文件中或其他地方，以及谁会读它们。尽可能使问题易于理解、定位和自我纠正。

如果必须的话，从自私的立场出发。忘了那些人吧。如果您可以查看日志文件并立即知道问题，难道不会让您自己的生活变得更容易吗？那不是比我们总是得到的那些更好吗？我这样做的时候，帮助很大。在许多情况下，我会在团队消息(或电子邮件)中直接将异常发送给我，看到它并给出解决方案，而不必查看代码。那是因为我添加了足够多的有用的上下文，至少对我自己是有用的。

所以给自己省点时间和精力吧。让其他人也能调试他们自己的问题，你会发现人们使用你的软件的方式有很大的不同！

下次见！