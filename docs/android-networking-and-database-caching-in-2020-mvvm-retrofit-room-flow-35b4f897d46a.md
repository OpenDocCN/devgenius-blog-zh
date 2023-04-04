# Android 网络和数据库缓存(MVVM+翻新+房间+流量)

> 原文：<https://blog.devgenius.io/android-networking-and-database-caching-in-2020-mvvm-retrofit-room-flow-35b4f897d46a?source=collection_archive---------0----------------------->

![](img/7e4a999175d1e38d5ed6227dd406bd21.png)

MVVM 是安卓开发者的新宠。它提供了很多好处，如干净的架构、代码可维护性等。然而，巨大的权力伴随着巨大的责任。你必须维护网络、数据库等等。如果你想在一个地方处理所有这些事情，那么你会得到难看的代码。幸运的是，我们现在已经有了解决方案。在本文中，我将介绍我所面临的问题以及我的库将如何解决这些问题，因为它将帮助您编写简洁、干净的代码，并对您的代码有更多的控制。

我遇到的第一个问题是，如何在一个地方处理数据库和网络？如果网络调用失败，在哪里处理改造错误响应？如果网络调用失败，从哪里获取数据？等等。

第二个问题是，我想使用最新的技术，所以我选择了 kotlin flow 而不是 livedata。那么如何用 flow 来组织所有这些东西呢？有什么办法处理这些事情吗？我决定为它写一个新的库。所以我是这样实现的。

在我们开始探索之前，实现这个库必须满足三个要求:

*   Dao 方法必须返回`Flow<ModelClass>`
*   Api 方法必须返回`Flow<ApiResponse<ModelClass>>`
*   我们需要添加 *FlowCallAdapterFactory* 作为改型生成器中的 CallAdapterFactory，让改型知道如何将响应转换成我们的`Flow<ApiResponse<>>`包装器。(它是作为库的一部分实现的)

我们将认为我们的网络调用在任何时候都有三种状态:成功、错误或加载。所以我们需要创建一个如下的类。

现在，我们将考虑我们 API 客户端(在本例中是[改进](https://github.com/square/retrofit))将返回三个响应之一:

*   成功响应—当网络呼叫成功时
*   错误响应—当网络调用失败时(包括改造引发的异常)
*   空响应—当成功响应没有正文(HTTP 204)时，我们将使用这个类

所以我们的实现看起来像这样

现在是真正的瑰宝，网络资源文件

在 NetworkBoundResource 文件中，我们在函数内部传递一些函数(Kotlin 的高阶函数),这些函数将决定在特定情况下应该做什么。下面是我们在 networkBoundResource 函数中传递的每个类的快速解释:

*   *fetchFromLocal* —它影响来自本地数据库的数据
*   *shouldFetchFromRemote* —它决定是否应该发出网络请求或使用本地持久数据(如果可用的话)(可选)
*   *fetchFromRemote* —执行网络请求操作
*   *processRemoteResponse* —在将模型类保存到数据库之前，处理网络响应的结果，如保存某些头值(可选)
*   *saveRemoteData* —将网络请求的结果保存到本地永久数据库
*   *onFetchFailed* —它处理网络请求失败场景(非 HTTP 200..300 个响应、例外等)(可选)

现在当使用它时，这是存储库类的样子

我们的视图模型类看起来像这样，

现在我们可以在我们的 UI 中观察这个变量，并基于它驱动 UI。

你不需要做所有这些花里胡哨的东西，因为它在 Github 上有库。您只需要在您的存储库和视图模型类中实现您的项目特定的代码。

您可以在 Github 上找到基于这个库构建的示例应用程序。

[](https://github.com/hadiyarajesh/flower) [## hadiyarajesh/花

### 超级酷的 Android 库，轻松管理数据库缓存和网络。它帮助你处理所有的情况…

github.com](https://github.com/hadiyarajesh/flower) 

如果这有助于你写出简洁明了的代码，请点击“鼓掌”按钮😃