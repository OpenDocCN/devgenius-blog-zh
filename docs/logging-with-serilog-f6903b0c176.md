# 用 Serilog 记录

> 原文：<https://blog.devgenius.io/logging-with-serilog-f6903b0c176?source=collection_archive---------0----------------------->

## 如何配置 Serilog 并将日志分割成不同的文件

![](img/7139fcd4e3937a2feb3d45104076ecab.png)

在这篇文章中，我将首先向你展示一些配置 Serilog 登录的基础知识。NET/ASP。NET 应用程序。然后转到如何拆分或隐藏日志的主题。源代码可以在[我的 GitHub 库](https://github.com/dotnet-labs/SerilogFilterDemo)找到。

# 系列配置基础

Serilog 是一种著名的测井工具。NET 和 ASP.NET 应用程序。我们可以使用下面一行代码轻松创建一个全局共享的日志记录器。

```
Log.Logger = new LoggerConfiguration().CreateLogger();
```

以这种方式创建的记录器将与我们的应用程序具有相同的生命周期。您还可以使用一个`using`语句来创建一个短期日志记录器，但是这种用例很少。

Serilog 可以使用简单的 C# API 直接在代码中配置记录器，也可以从设置文件中加载外部配置。对于最低配置，我们需要将日志接收器连接到全局静态日志记录器，以便可以将消息写入某个位置。例如，我们可以添加一个控制台接收器来记录日志事件，如下所示。

```
Log.Logger = new LoggerConfiguration()
    .WriteTo.Console()
    .CreateLogger();
```

在创建了全局记录器之后，我们需要告诉。NET 或 ASP.NET 的日志，以便。NET 或 ASP.NET 可以通过管道将消息发送给 Serilog。否则，如果不向主机分配日志提供者，那么由`ILogger<T>`记录的消息就没有出口。要将 Serilog 注册为日志提供者，我们可以如下调用`IHostBuilder`上的`UseSerilog()`方法。

此后，我们可以使用中的日志系统将消息记录到所需的接收器中。我们也可以在任何地方直接使用全局记录器。下面一行显示了全局记录器的示例用法。

```
Log.Information("Application Starts");
```

Serilog 提供了多种汇([链接](https://github.com/serilog/serilog/wiki/Provided-Sinks))。我们可以根据需要添加它们。例如，要将日志保存到文件中，我们可以如下所示将文件接收器附加到日志记录器。

```
Log.Logger = new LoggerConfiguration()
    .WriteTo.Console()
    .WriteTo.File("log.txt", rollingInterval: RollingInterval.Day)
    .CreateLogger();
```

# 拆分或隐藏日志

有时，我们可能希望对每个接收器的日志消息类别进行更细粒度的控制。例如，我们希望将具有不同日志级别的消息记录到不同的文件中，以便将错误和警告从低重要性消息中突出出来。另一个例子，我们希望将后台作业的消息记录到一个不同于记录常规例程的文件中。或者我们希望抑制一部分消息以降低噪声水平。对于这些用例，Serilog 允许我们使用过滤器来设置日志管道，以包含或排除某些日志事件，从而将日志划分到不同的接收器中。例如，以下配置创建了一个控制台记录器，它将输出所有消息，以及一个子记录器，它根据行`Filter.ByIncludingOnly(...)`中定义的标准只将某些事件写入`log.txt`文件。

```
Log.Logger = new LoggerConfiguration()
    .WriteTo.Console()
    .WriteTo.Logger(lc => lc
        .Filter.ByIncludingOnly(...)
        .WriteTo.File("log.txt"))
    .CreateLogger();
```

大多数关于使用`Filter`的例子都集中在介绍使用强大的 SQL 式语法的过滤表达式。[过滤器表达式文档](https://github.com/serilog/serilog-expressions)给出了全部细节。我喜欢过滤器表达式，因为它在 XML 或 JSON 文件的配置中提供了很大的灵活性。

这里我想介绍另一种过滤日志事件的便捷方式:通过在`LogContext`中包含/排除属性。

例如，一个方法有两个依赖项`_myService1`和`_myService2`。

```
public async Task MyMethod()
{
    _logger.LogInformation("foo bar start");
    await _myService1.Foo();
    await _myService2.Bar();
    _logger.LogInformation("foo bar end");
}
```

现在我们想把这个方法下的所有执行消息写到一个名为`foobar.txt`的单独文件中。我们可以使用 filter express 通过类名计算出 LogContext。然而，如果我们在方法中引入另一个依赖项，这将是不可伸缩的。一种更简单的方法是按属性名和/或属性值进行过滤。

出于演示的目的，我们可以将一个属性推入日志上下文，以便该方法执行路径中的所有日志事件都将继承该属性的键和值。下面的代码片段显示了一个示例。

`using`语句创建一个作用域，并确保所需的属性不会泄露给其他方法。在该范围内，所有日志事件都将包含属性。因此，我们可以根据键值对过滤这些日志事件。请注意，您可以为所有属性键值对创建一个类，以避免神奇的字符串/值。

下面的代码片段显示了一个过滤示例。

[要点链接](https://gist.github.com/changhuixu/826642193dd7dc23080ea7274d9515c3)

通过上面的配置，普通日志(日志事件中没有属性`foobar`)将被保存到`log.txt`文件中，而具有属性`foobar`的日志将被保存到`foobar.txt`文件中。您还可以基于键和值编写更复杂的过滤器。

今天到此为止。我们已经讲述了 Serilog 配置的一些基础知识，并且学习了一些过滤日志的技巧。同样，你可以在[我的 GitHub 库](https://github.com/dotnet-labs/SerilogFilterDemo)中找到源代码。

希望有帮助。感谢阅读。