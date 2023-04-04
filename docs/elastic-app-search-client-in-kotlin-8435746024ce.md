# Kotlin 中的弹性应用程序搜索客户端

> 原文：<https://blog.devgenius.io/elastic-app-search-client-in-kotlin-8435746024ce?source=collection_archive---------1----------------------->

![](img/9ce15191a7cc065695aedec72f9d86c7.png)

应用搜索和 Kotlin

在我之前的[帖子](https://medium.com/dev-genius/elastic-app-search-in-a-nutshell-ad446e7a3293)中，我试图提供关于弹性应用搜索的亮点，它提供搜索即服务。正如帖子中提到的，Elastic 提供了多种语言的客户端实现；但是没有 Java 或 Kotlin 的实现。所以我决定实现我自己的。

我对此感到特别兴奋，因为我已经使用 Elastic Stack(主要是 Elasticsearch)很多年了，App Search 也在我的考虑范围之内。对我来说，这是一个深入应用搜索并为生态系统做出贡献的绝佳机会。顾名思义，我选择了 Kotlin 作为实现语言。你可能已经知道，Kotlin 是我最喜欢的编程语言，它在 JVM 上。

除此之外，这是我第一次设法将一个库部署到公共 Maven 仓库，并通过 [GitHub Actions](https://github.com/features/actions) 来简化这个过程。

在接下来的几节中，我将试着回顾一下 app-search-kotlin 实现的一些细节。

# 放弃

app-search-kotlin 是一个应用程序搜索的客户端实现，我作为一个附带项目开发的，不应该被视为一个产品质量，并相应地包含在项目中。

# 位置

源代码位于 [Github](https://github.com/itasyurt/app-search-kotlin/) 中。app-search-kotlin JAR 在[search.maven.org](https://search.maven.org/search?q=a:app-search-kotlin)有售。

# 和睦相处

app-search-kotlin 是使用 Elastic Stack 7 . 8 . 1 版本开发的(Elasticsearch 和 Elastic App Search)。我没有对其他 7.x.y 版本进行测试，但理论上它应该可以工作。

库是用 Kotlin 1.3 和 Java 11 开发的，没有针对其他版本进行测试。与弹性堆栈版本类似，它应该不需要或只需要很少的更改就可以工作，但是可能需要重新构建。

# 属国

实现本身有两个依赖项，它们是:

*   [杰克逊](https://github.com/FasterXML/jackson-module-kotlin)
*   [Apache Http 客户端](https://hc.apache.org/httpcomponents-client-ga/)

Jackson 用于应用搜索请求的 JSON 序列化和反序列化。注释和 Jackson mapper(用于 Kotlin)都用于此目的。

对于 HTTP 通信，选择 Apache Http 客户端。虽然有一些更容易使用的库，比如 [Fuel](https://github.com/kittinunf/fuel) 和 [Unirest](http://kong.github.io/unirest-java/) ，但是它们不允许带有主体的 HTTP GET 和 DELETE 请求。Apache HTTP 客户端可以很容易地被[扩展](https://github.com/itasyurt/app-search-kotlin/blob/8b6215f1b9333c05847a795a62a9b4e9f9e79947/src/main/kotlin/com/github/itasyurt/appsearch/client/api/util/http/HttpRequests.kt#L10)，以在这些请求中包含主体。

# 客户端 API

界面*客户端(com . github . itasyurt . App Search . Client . Client)*作为使用 App 搜索客户端的主立面。顾名思义，DefaultClient 是默认实现。

客户端由引用应用程序搜索的[API](https://www.elastic.co/guide/en/app-search/current/api-reference.html)的不同客户端对象组成，它们是:

*   引擎客户端
*   SchemaClient
*   搜索客户端
*   搜索设置客户端
*   建议客户
*   同义词客户端
*   策展客户

# 域对象

域对象主要实现为*com . github . itas yurt . app search . client . domain*包下的[数据类](https://kotlinlang.org/docs/reference/data-classes.html)。域类用 Jackson 注释修饰，用于 JSON 序列化/反序列化。

App 搜索文档被认为是*映射<字符串，对象>T17，这是[别名](https://kotlinlang.org/docs/reference/type-aliases.html)为*文档*类型。*

# 使用

您可以用一个基本 URL 和 API 键实例化一个客户端。一旦客户端成功，就可以通过客户端对象访问子 API。下面你可以看到一个执行搜索的示例代码片段。(而且你很容易理解我是一级方程式赛车的超级粉丝)。

更详细的文档可以在[自述文件](https://github.com/itasyurt/app-search-kotlin/blob/master/README.md#usage)中找到。

# 局限性和缺失的功能

正如我在免责声明部分提到的，目前***app-search-kot Lin***算是我的一个爱好项目。初始版本中不包含某些 API。但是在未来的版本中，它们很容易实现:

*   API 日志 API
*   日志设置 API
*   分析 API
*   点击 API
*   凭据 API
*   元引擎 API

此外，当前版本只能在 HTTP 上工作，不支持 HTTPS。

# Java 兼容性

因为实现是在 Kotlin/JVM 中；它可以与 Java 互操作。然而，一些特性，比如命名参数的默认值，或者类的 getter 属性，对于 Java 是不可用的。为了更方便地使用 Java，下一步可能是使实现对 Java 更友好；或者为 Java 实现一些实用程序(比如构建器)。

# 试验

在单元测试中使用 [MockServer](https://www.mock-server.com/) 测试实现。简而言之，部署一个模拟应用程序搜索请求和响应的模拟服务器，并根据预期的请求和响应测试实现。

# 构建和发布

神器被发布到 search.maven.org 的[。当用 GitHub 动作创建一个发布时，发布被触发。](https://search.maven.org/search?q=a:app-search-kotlin)

你可以在 [build.gradle.kts](https://github.com/itasyurt/app-search-kotlin/blob/master/build.gradle.kts) 和相关的 [GitHub 动作](https://github.com/itasyurt/app-search-kotlin/blob/master/.github/workflows/clean_build.yml)中找到更多信息。

# 结论

在这篇博文中，我试图记录我为创建弹性应用程序的客户端实现所做的工作。你可以在 [GitHub](https://github.com/itasyurt/app-search-kotlin/) 中看到完整的文档和源代码。

这对我来说是一个非常愉快的尝试，因为这是我第一次从零开始实现一个 API 客户端(更好的是对于我非常喜欢的弹性堆栈)；这也是我第一次发布我的项目，简化工作流程。所以这对我来说是一次很好的学习经历。

正如我之前提到的，这是一个业余爱好项目，有许多缺陷和不完整的功能。如果您有任何意见、建议或贡献，请随时联系。