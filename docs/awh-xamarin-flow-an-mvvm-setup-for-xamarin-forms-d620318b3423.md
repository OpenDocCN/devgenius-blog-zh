# AWH Xamarin 流——xa marin 的 MVVM 设置。形式

> 原文：<https://blog.devgenius.io/awh-xamarin-flow-an-mvvm-setup-for-xamarin-forms-d620318b3423?source=collection_archive---------2----------------------->

![](img/8646ad3f0c0ea74780d75aa6149d1e5a.png)

Xamarin。Forms 是一个强大的工具包，可以在。网络生态系统。它快速、稳定，并且依赖于成熟的开发栈。不幸的是，它只是一个工具包，而不是一个全功能的框架。这使得启动新的应用程序或保持现有应用程序的可维护性变得困难，因为没有“正确”的方法来组织代码。

幸好使用了一些常见的。NET 包和技术，很容易在 Xamarin.Forms 中构建一个简单的、可扩展的、可单元测试的 MVVM 设置。 [AWH](http://awh.net) Xamarin Flow 是我们在 [AWH](http://awh.net) 开发的设置，使从头构建一个应用程序变得容易，并保持它的可维护性。

# 示例应用程序

我已经创建了一个[样本 Xamarin。Forms app](https://bitbucket.org/awhcode/awh-xamarin-flow) 使用 [AWH](http://awh.net) Xamarin 流设置，并已将代码放入公共回购中，因此您可以关注这篇文章。我建议真正深入研究代码，因为我不会涵盖流程的每个方面。相反，我将提请注意一些关键部分，看看它们为什么重要，以及您可能希望如何调整流程以满足您的应用程序的需求。

# 启动和主机构建器

流程的核心是启动类。通常情况下，在 Xamarin 中。每个平台都会初始化 app 类的一个新实例。在 Android 上，这是在 MainActivity.cs 中完成的，而在 iOS 上，这是在 AppDelegate.cs 中完成的。此方法将创建一个新的 HostBuilder(从 Microsoft。Extensions.Hosting NuGet package)，它将完成注册服务和配置之类的工作，然后返回 App 类的一个实例。然后，我们可以在每个平台的项目中调用这个方法，而不是直接创建 App 实例。

使用 HostBuilder 可以让我们利用其他。NET 应用程序，如依赖注入。它还接受一个 Action 参数，该参数将让每个特定于平台的项目注册可以使用本机平台 API 的服务。如果您的应用程序不需要本机服务，那么您可以简单地从 Initialize 中移除参数，并移除。使用该参数的 ConfigureServices()行。

让我们看看使用 HostBuilder 的两个主要优点:配置和依赖注入。

**配置**

在流中使用 HostBuilder 的主要优势之一是能够使用熟悉的 appsettings。*.用于配置的 json 文件。我们可以加载嵌入在共享项目中的 JSON 文件，并将它们注册为配置值的来源。然后可以使用 IConfiguration 实例将这些值注入到任何需要它们的类中(尽管您可能希望创建一个包装器服务来使您的值具有强类型)。

```
// The GetManifestResourceStream functions cannot be executed from within the
// ConfigureHostConfiguration's configureDelegate because that causes a "Stream
// was not readable" ArgumentException.
var assembly = Assembly.GetExecutingAssembly();
var environment = Constants.Environment.Current;
using var appSettingsStream = assembly.GetManifestResourceStream("XamSample.appsettings.json");
using var appSettingsEnvironmentStream = assembly.GetManifestResourceStream($"XamSample.appsettings.{environment}.json");
​
var host = new HostBuilder()
    // (Removed some code for brevity)
    .ConfigureHostConfiguration(config =>
    {
        config.AddJsonStream(appSettingsStream);
        config.AddJsonStream(appSettingsEnvironmentStream);
    })
    // (Removed some code for brevity)
```

使用常量。环境类允许根据项目的当前构建配置更改包含的文件。例如，在一个 API 驱动的应用程序中，我们可能有不同的 URL 用于开发、测试、UAT 和生产 API。将每一个都作为不同的构建配置，将使我们能够构建应用程序，以便它可以引入正确的设置来连接到正确的 API。

您的应用程序可能不需要这样的配置文件，或者它可能不需要特定于环境的文件。您可以自定义加载什么文件，甚至如何加载。有许多选项可用于加载配置，但我们更喜欢使用 appsettings.json，因为它是最常见的模式。

**依赖注入**

在流程中使用 HostBuilder 的另一个主要优点是，我们可以使用依赖注入。ASP.NET 核心推广了一种构造函数注入风格，任何有经验的人都应该非常熟悉。NET 开发者。通过在 HostBuilder 实例上使用 ConfigureServices 方法，我们可以注册我们的应用程序的所有服务、视图和视图模型，以便它们可以接收依赖项，因此它们可以是依赖项。我们甚至注册了我们的 App 类，这样它就可以接收 NavigatorService 的一个实例(稍后将详细介绍)。

```
public static App Initialize(Action<HostBuilderContext, IServiceCollection> configureNativeServices)
{
    // (Removed some code for brevity)
    var host = new HostBuilder()
        // (Removed some code for brevity)
        .ConfigureServices(configureNativeServices)
        .ConfigureServices(ConfigureServices)
        // (Removed some code for brevity)
        .Build();
​
    App.ServiceProvider = host.Services;
​
    return App.ServiceProvider.GetService<App>()
        ?? throw new Exception("The App service provider isn't set up properly.");
}
​
private static void ConfigureServices(HostBuilderContext context, IServiceCollection services)
{
    // Custom services for this app. Each service should have a matching
    // interface so it can be mocked for unit tests.
    services.AddSingleton<IPageService, PageService>();
    services.AddSingleton<INavigatorService, NavigatorService>();
​
    // Add Pages and their ViewModels as transients so they can be retrieved using DI
    services.AddTransient<ItemListPage>();
    services.AddTransient<ItemListViewModel>();
    services.AddTransient<ItemDetailPage>();
    services.AddTransient<ItemDetailViewModel>();
​
    // Finally register the App so it can access the NavigatorService
    services.AddSingleton<App>();
}
```

我们做的一件值得注意的事情是将 App 类的静态 ServiceProvider 属性设置为已注册服务的完整集合。当我们需要在 PageService 中动态地拉取页面和查看模型时，以及在我们无法访问普通构造函数注入的罕见情况下，这是非常有用的。我们还使用它来检索我们注册的 App 类的 singleton 实例，该实例将被返回到平台项目，以便在应用程序启动时加载。

关于服务生存期的一个注意事项:通常，我们有[三种注册类型](https://docs.microsoft.com/en-us/dotnet/core/extensions/dependency-injection#service-lifetimes)可供选择(单例、作用域和瞬态)，但是因为我们不在请求/响应环境中(像 web 应用程序或 API)，我们不能使用作用域服务，所以只应该注册单例或瞬态。

在 [AWH](http://awh.net) Xamarin Flow 中有一个坚实的依赖注入设置，有助于保持我们的服务、视图模型和其他类的松散耦合。这反过来又使单元测试变得容易得多，因为我们可以更容易地模拟依赖关系。作为奖励，您可以包括 [Xamarin。Essentials.Interfaces](https://www.nuget.org/packages/Xamarin.Essentials.Interfaces) 包，其中包括 Xamarin 中包含的设备功能的已生成接口。必需品包。这个包使得代码的可测试性变得更加容易。

# 视图-视图模型关系

任何 MVVM 设置的关键方面之一是视图与视图模型之间的关系。你需要确保他们是正确配对和正确的分离他们之间。这通常使用“视图模型定位器”类来完成，该类将视图映射到视图模型。在 [AWH](http://awh.net) Xamarin Flow 中，我们使用 PageService(在下一节中详细介绍)来加载页面并找到正确的视图模型。但服务只接收页面类型，而不接收视图模型类型。为了保持它们之间的链接，我们使用了一个名为 IPageWithViewModel 的接口，每个页面都应该实现这个接口:

```
public interface IPageWithViewModel
{
    Type ViewModelType { get; }
}
```

然后，每个页面都在代码隐藏类中实现接口，并设置它所需的视图模型:

```
public partial class ItemDetailPage : ContentPage, IPageWithViewModel
{
    public Type ViewModelType => typeof(ItemDetailViewModel);
​
    public ItemDetailPage() => InitializeComponent();
}
```

这可以确保我们可以轻松地为页面获得正确的视图模型，同时保持视图和视图模型松散耦合。我们希望视图不直接处理视图模型，而只知道它需要什么视图模型。对于视图模型，它应该对使用它的视图一无所知。

**基础视图模型**

如果深入挖掘代码，您可能会在 ViewModels 文件夹中注意到一些奇怪的东西:有两个基本视图模型。我们已经把 BaseViewModel 和 BasePageViewModel 分开了。BaseViewModel 类包含我们对 INotifyPropertyChanged 的实现，这是我们在 XAML 的绑定工作所必需的。BasePageViewModel 类包含一个 NavigatorService 实例和两个生命周期方法:InitializeAsync 和 RefreshAsync。当视图模型表示比整个页面更小的可视数据对象时，分离这些对象的好处就来了。这可能是一个视图模型，它表示 ListView 中的一个项目，您仍然需要 INotifyPropertyChanged goodness，但不需要 NavigatorService。

当加载(或返回)页时，生命周期方法对于正确调用异步方法非常有用。不需要使用任何欺骗从构造函数调用异步方法，也永远不需要使用异步 void 方法。这是一个特别困难的问题，特别是对于新的 Xamarin 开发人员来说，这些方法是 Flow 的一个非常有价值的方面。

同样值得注意的是我们是如何实现 INotifyPropertyChanged 的。SetProperty 方法是处理带有支持字段的属性的最干净、最简单的方法，同时还可以根据需要调用 PropertyChanged 事件。对于视图模型上的任何给定属性，这种实现需要最少的样板代码:

```
private string _title = string.Empty;
public string Title
{
    get => _title;
    set => SetProperty(ref _title, value);
}
```

# 页面和导航服务

前面提到的页面服务在 [AWH](http://awh.net) 主流中扮演“视图模型定位器”的角色。它负责按类型获取页面的实例，获取页面视图模型的实例，并将视图模型设置为页面的绑定上下文。它使用 App 类的 ServiceProvider 属性完成所有这些工作，因此它依赖于我们在 Startup.cs 中设置的依赖注入。

```
public Page GetPage<PageType>()
{
    if (App.ServiceProvider == null)
        throw new InvalidOperationException("App.ServiceProvider is null and has not been set up correctly");
​
    var page = App.ServiceProvider.GetService<PageType>() as Page;
​
    if (page == null)
        throw new ArgumentException("Unable to locate page in ServiceProvider", nameof(PageType));
​
    if (page is IPageWithViewModel pageWithViewModel)
    {
        var viewModel = App.ServiceProvider.GetService(pageWithViewModel.ViewModelType) as BasePageViewModel;
​
        if (viewModel == null)
            throw new ArgumentException("Unable to locate page view model in ServiceProvider", pageWithViewModel.ViewModelType.Name);
​
        page.BindingContext = viewModel;
    }
​
    return page;
}
```

大多数时候，你的应用程序不会(也不应该)直接处理页面服务，而是与导航服务交互。

NavigatorService 在很大程度上依赖于 PageService，并充当 Xamarin 的包装器。窗体 NavigationPage 实例。它提供了基本应用程序导航所需的方法，如在导航堆栈中推送和弹出页面，以及模式页面的类似方法。不过，根据你的应用程序的用户界面，导航服务可能并不完美。您可能需要一个不同的服务来充当类似的角色，但包装了一个 TabbedPage 或一个弹出页面。在 [AWH](http://awh.net) Xamarin 流程的所有部分中，这一部分可能需要对任何特定的应用程序进行最多的更改。

[AWH](http://awh.net) Xamarin 流使得构建和维护 Xamarin 变得容易。表单应用程序使用类似的技术。NET 应用程序。还有其他框架，但是这种设置实现了大部分相同的功能，同时减少了您必须处理的“黑盒”库的数量。通过使用一些标准的软件包，我们已经拼凑出了一个我们自己的可靠的、可扩展的框架。

这个设置是由汤米·埃利奥特创建的，他是 AWH[的软件开发团队负责人。非常感谢他将设置放在一起，帮助我创建示例项目，并帮助我理解 Xamarin。他还创造了这个名字，这比一直称它为“我们的设置”要好得多。](http://awh.net)

## ——安德鲁·莫斯卡迪诺，AWH[公司的软件开发人员。我们正在帮助企业通过技术推动增长。](http://awh.net)