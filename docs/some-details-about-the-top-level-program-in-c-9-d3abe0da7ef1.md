# C# 9 中顶层程序的一些细节

> 原文：<https://blog.devgenius.io/some-details-about-the-top-level-program-in-c-9-d3abe0da7ef1?source=collection_archive---------7----------------------->

![](img/7ec47f8b25626baf10975fa6c0d2b997.png)

[https://unsplash.com/photos/QCxngnhwiE4/share](https://unsplash.com/photos/QCxngnhwiE4/share)

一艘下水后。NET 5 和 C# 9，我完全沉迷于顶级程序(又名 TLP)的功能。TLP 提供快速原型设计。NET 应用程序，使我的工作最直接的条目。

但是 TLP 只是语法糖，不是完整的解决方案。所以在选择 TLP 之前，有些细节你应该知道。

# 线程单元模型

这种细节很少见，但对 Windows 应用程序开发人员来说至关重要。通常，Main 方法不需要特定的属性，不包括 Windows 桌面应用程序区域。这就是线程单元模型，它影响了 Windows 窗体或 WPF 应用程序。

如果您将入口点设在 TLP，将很难指定线程模型。根据我的经验，用 thread 类配置线程单元模型通常效果不好。如果不将线程模型配置为单线程，Windows 窗体应用程序将无法正常运行，尤其是使用 [WebView2](https://docs.microsoft.com/en-us/microsoft-edge/webview2/) 时。

因此，如果您想在应用程序中使用 Windows 窗体，可以再次将入口点恢复到 Main 方法中。

# 把你的代码整理好写给别人看。

TLP 可以帮助你更快地编码，但是你的代码可能会很混乱。因为 TLP 允许您在逻辑代码和成员函数声明之间混合或打乱声明顺序。

例如，下面的代码是有效的和可编译的。

```
using System;int x, y;void TestFuncA() => x = 1;void TestFuncB() => y = 2;void Calculate() => Console.WriteLine(x + y);TestFuncA();
TestFuncB();
Calculate();
```

同样，这个代码也是有效的。

```
using System;int x, y;void TestFuncA() => x = 1;TestFuncA();void TestFuncB() => y = 2;TestFuncB();void Calculate() => Console.WriteLine(x + y);Calculate();
```

如果你不整理你的代码，会让别人读起来很难。所以这就是当你使用 TLP 时，最好避免大而复杂的代码的原因。

# args，隐藏变量

您可以使用参数输入编写简单的应用程序。TLP 隐藏了 args 变量，但它仍然存在。所以你可以像以前一样引用局部变量 args。

所以你可以像下面这样写你的代码。

```
// This sample code requires the [Bullseye](https://github.com/adamralph/bullseye) package.using System;
using System.Threading.Tasks;using static Bullseye.Targets;int x = default, y = default;async Task Banner()
{
    await Console.Out.WriteLineAsync("Hello, World!");
}async Task ReadX()
{
    int.TryParse(await Console.In.ReadLineAsync(), out x);
}async Task ReadY()
{
    int.TryParse(await Console.In.ReadLineAsync(), out y);
}async Task WriteResult()
{
    await Console.Out.WriteLineAsync($"{x + y}");
}Target("banner", Banner);
Target("read-x", ReadX);
Target("read-y", ReadY);Target("calc-result", DependsOn("read-x", "read-y"), WriteResult);
Target("default", DependsOn("banner", "calc-result"));await RunTargetsAndExitAsync(args);
```

# 有限的功能

与我的想法相反，TLP 不支持某些特性。例如，可以声明静态局部函数，但不能声明静态局部变量。所以如果你写静态局部函数，你的代码应该包含一个隔离的逻辑(比如一个算法)。但是您可以从入口点代码中调用函数。

另一个缺失的特性是，TLP 不会将主方法代码作为静态类发出。在我看来，局部函数是一个非静态的类成员，所以当我们使用 TLP 特性时，没有必要保留非静态成员。很遗憾，扩展方法不能存在于 TLP 代码中。