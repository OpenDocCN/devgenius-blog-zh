# 拉勒维尔 7.x — P3:第一条路线

> 原文：<https://blog.devgenius.io/laravel-7-x-p3-first-route-98b8ac4edcfc?source=collection_archive---------4----------------------->

![](img/c72bd7cd6534a9d14b884b89115855e6.png)

任何框架的主要工作都是处理请求。重要的是要知道用户将试图从你的站点检索页面，或者向你的站点提交数据。当请求进入 Laravel 框架时会发生什么？Laravel 是如何能够检索到用户想要的内容并返回给用户的？这个话题超出了本文的范围。

我们想知道的是，当用户请求一个特定的页面时，比如“关于”页面，我们如何一致地向他/她提供“关于”页面的内容？

在我们创建第一条路由之前，我们应该知道 Laravel 是一个 MVC 框架。不要过度思考这个术语；它只代表模型-视图-控制器。把*模型*想象成你的数据库表。*视图*是返回给用户的 HTML/CSS(甚至 JavaScript)内容。如果你正在看页面，那就是*视图*。Laravel 将 HTML 返回到您的浏览器，您的浏览器能够将脚本翻译成 HTML/CSS 代码的可视化表示。*控制器*与*模型*和*视图*进行通信。它首先接收来自用户的请求。如果数据库中存储了任何数据，*控制器*告诉*模块*检索数据。*模块*将数据返回给*控制器*，*控制器*将数据传递给*视图*。一旦*视图*生成，*控制器*将*视图*返回给用户。

但是*控制器*首先是如何被调用的呢？如果我们可以有数百个控制器，Laravel 如何知道调用哪个*控制器*？通过查看路由文件(routes/web.php)中指定的路由。你有完全的控制权。您可以为任何类型的 HTTP 请求创建路由，并将其存储在 routes 文件中。一些框架，比如 [CodeIgniter](https://codeigniter.com/userguide3/general/routing.html) ，有一个系统自动为你做这件事(*尽管你也可以选择在那里创建你自己的路线*)。虽然这很好，但是对于更高级的应用程序，您将希望完全控制您的 routes 文件。一旦您创建了几条路由，您将很快意识到控制路由是多么强大。

如果您查看您的 *routes/web.php* 文件，您将会看到您已经有了一个路由。那是主页的路线。它是当有人访问基本 URL 并省略了 URL 中的任何其他参数:[http://example.com/](http://example.com/)

该路由声明我们将要处理的请求类型是 GET 请求。通俗地说，当用户键入 URL 时，他或她是在请求页面，或者得到页面(这是一种过于简单化的说法)。第一个参数是需要服务的路线。如果没有其他 URL 参数，正斜杠/就是正在调用的路由。要了解有关 HTTP 请求的更多信息，请访问:

[](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods) [## HTTP 请求方法

### HTTP 定义了一组请求方法来指示对给定资源要执行的操作。虽然…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods) 

第二个参数是闭包回调。如果您不熟悉 PHP 中的闭包(匿名函数)或回调，我有关于这两方面的文章。

[](https://medium.com/dev-genius/php-7-x-p39-anonymous-functions-75f6f99fa571) [## PHP 7.x — P39:匿名函数

### 匿名函数是没有名字的函数。

medium.com](https://medium.com/dev-genius/php-7-x-p39-anonymous-functions-75f6f99fa571) [](https://medium.com/dev-genius/php-7-x-p42-callback-functions-6552477f2a87) [## PHP 7.x — P42:回调函数

### 是时候结束这个话题了。我通过令人痛苦的细节来简化这个概念。

medium.com](https://medium.com/dev-genius/php-7-x-p42-callback-functions-6552477f2a87) 

在[的最后一篇文章](https://medium.com/dev-genius/laravel-7-x-p2-modifying-the-welcome-view-3a00367cc27)中，我指定了位于*resources/views/welcome . blade . PHP*中的 *welcome* 视图是 Laravel 提供给我们的路线中返回的内容。为什么 return 和 not *echo* out 响应？Laravel 被中间件包围着，还不准备将内容回显出来。一旦一切完成，Laravel 将发送响应。我们将在适当的时候讨论所有这些概念。

我们不必返回一个视图；我们可以只返回 HTML 或者一个字符串。因此，让我们创建我们的第一条路线。我们将为“关于”页面创建一条路线。当用户访问[*【http://example.com/about】*](http://example.com/about)*时，他或她将被提供关于页面。在我们的本地环境中，这将是[http://127 . 0 . 0 . 1:8000/about。](http://127.0.0.1:8000/about.)我们只想返回字符串*关于页面*包裹在*<【h1】>*标签中。*

*如果您从项目的根目录运行命令 *php artisan serve* ，并导航到[*http://127 . 0 . 0 . 1:8000/about*，](http://127.0.0.1:8000/about,)您将看到 *about* route 提供的内容。*

*![](img/2b927e22d14d875a0291e915e9c9dbf9.png)*

*如果您一直在关注，并且想要在主屏幕上添加到*关于*页面的链接，请打开*index.blade.php*视图并创建一个附加链接。*

*现在，您的主页上会有一个链接，单击该链接可以转到“关于”页面。*

*![](img/1b2e71f06c6ad19780f920bdacb7fdfe.png)**[](https://github.com/dinocajic/laravel-7-youtube-tutorials) [## dinocajic/laravel-7-YouTube-教程

### 运行以下命令 composer install NPM install CP . env . example。env 或复制您的。环境文件 php artisan…

github.com](https://github.com/dinocajic/laravel-7-youtube-tutorials)* *![](img/88ff0f8a3052609ff0a53e675338c89e.png)*

*迪诺·卡伊奇目前是 [LSBio(生命周期生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 主管。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。*

*你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。*

*[*阅读迪诺·卡吉克(以及媒体上成千上万其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。*](https://dinocajic.medium.com/membership)*