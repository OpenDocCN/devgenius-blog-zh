# Laravel 依赖注入——用我自己的话说。

> 原文：<https://blog.devgenius.io/laravel-dependency-injection-on-my-own-words-8811004ab17c?source=collection_archive---------4----------------------->

![](img/d126e80e0c8b386828fff5db3d56c7c4.png)

[疾控中心](https://unsplash.com/@cdc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](/s/photos/injection?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍照

我在这篇文章中写的东西实际上可以在 Laravel 关于[服务容器](https://laravel.com/docs/7.x/container)的官方文档中读到。然而，不知何故，我花了相当长的时间才理解那里所呈现的东西。所以我写了这篇文章来帮助自己以一种更简单的方式理解它——在我看来。这对我有效，我希望对你也有效。

# 依赖注入

例如，假设:

*   您的代码需要一个来自类`HelpSpotService`的对象
*   要从类`HelpSpotService`创建一个对象，你需要在它的构造函数中传递一个类`HelpSpot\API`的对象。
*   要从类`HelpSpot\API`创建一个对象，你需要在它的构造函数中传递一个类`HttpClient`的对象。

通常，如果没有依赖注入技术，您将会做如下事情:

```
function yourMethod()
{
    $httpClient = new HttpClient(["timeout" => 50]);
    $helpSpotAPI = new HelpSpot\API($httpClient);
    $helpSpotService = new HelpSpotService($helpSpotAPI); // do something with helpSpotService
    $helpSpotService->doSomething();
}
```

如果你碰巧在很多地方使用了类`HelpSpotService`的对象，那么它会给你很多行代码，而这些代码实际上并不是你的主要应用程序逻辑的一部分。当依赖链很长时，这有时会让人不知所措。这就是依赖注入发挥作用的地方。

有了依赖注入，不再是从创建一个`HttpClient`的实例开始，而是最终创建`HelpSpotService`的实例，您只需对框架说“我需要一个`HelpSpotService`类的实例，请为我创建一个”，如下所示。

```
function yourMethod()
{
    $helpSpotService = $app->make('HelpSpotService'); $helpSpotService->doSomething();
}
```

然后，Laravel 会自动为您创建对象。

你可能会问，这怎么可能呢？laravel 如何知道如何创建所有需要的依赖关系来创建一个类`HelpSpotService`的对象？好吧，拉弗尔只知道如果你定义它。我们描述如何在服务容器中解决依赖性。

# 服务容器

我们通过“注册”每个依赖项来定义如何在服务容器中解析依赖项。看看下面的代码，这是 Laravel 文档的一个片段:

```
$this->app->bind('HelpSpot\API', function ($app) {
    return new HelpSpot\API($app->make('HttpClient'));
});
```

您可以在`app/Providers/AppServiceProvider.php`中的`register`方法内编写代码。现在，只要你在代码中需要一个`HelpSpot\API`对象，你只需输入:

```
$api = $app->make('HelpSpot\API');
```

看到你不需要费心制作一个`HttpClient`来制作对象，Laravel 为你处理它。

现在你可能想知道`HttpClient`是如何解决的？这和我们制作`HelpSpot\API`的过程是一样的，应该有定义`HttpClient`如何被解析的代码。例如，代码可能如下所示:

```
$this->app->bind('HttpClient', function ($app) {
    return new HttpClient(["timeout" => 50]);
});
```

所以要创建一个`HelpSpot\API`的对象，Laravel 会自动寻找它是如何解析的，并发现它需要先解析一个`HttpClient`的对象。Laravel 然后重复相同的过程来寻找如何解析`HttpClient`，将`HttpClient`对象返回到`HelpSpot\API`对象创建中，最终创建一个`HelpSpot\API`对象并将其返回到您的代码中。

回到第一个例子。为了对`HelpSpotService`进行依赖注入，您需要像下面这样定义`HelpSpotService`是如何被解析的。

```
$this->app->bind('HelpSpotService', function ($app) {
    return new HelpSpotService($app->make('HelpSpot\API'));
});
```

就是这样。我希望这有意义，现在你知道它是如何工作的了。