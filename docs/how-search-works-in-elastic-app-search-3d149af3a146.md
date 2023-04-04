# 在弹性应用搜索中搜索如何工作

> 原文：<https://blog.devgenius.io/how-search-works-in-elastic-app-search-3d149af3a146?source=collection_archive---------2----------------------->

![](img/c19ddddbc869a0a06e40badbd2a9a1de.png)

弹性应用搜索

首先，对标题中的杂音表示抱歉；对我来说，决定一个标题来同时保留搜索和应用程序搜索有点困难。在我以前的帖子中；我曾简单提到过[弹性应用搜索](https://medium.com/dev-genius/elastic-app-search-in-a-nutshell-ad446e7a3293)和[我的 Kotlin 客户端实现。](https://medium.com/dev-genius/elastic-app-search-client-in-kotlin-8435746024ce)在这篇文章中，我计划做一个更深入的探索，并分享我在弹性应用搜索中的发现。

# 什么怎么会？

[Elastic App Search](https://www.elastic.co/app-search/) 是一款在 Elasticsearch 基础上简化应用搜索的解决方案。因此，应用搜索的操作由 Elasticsearch 集群支持和处理。因此，发送到 App Search 的每个请求都被翻译成适当的 Elasticsearch API 调用(查询、索引或其他)，然后 Elasticsearch 响应以同样的方式进行处理和转换。

不幸的是，弹性应用搜索不是开源的，所以不可能从源代码中理解它是如何工作的；但幸运的是，它与 Elasticsearch 的通信是通过 HTTP 进行的；因此，拦截 HTTP 请求以了解其工作原理是可能的。

为此，我考虑在应用搜索和记录的请求和响应之间部署一个 HTTP 代理。(我使用了 [Passthru](https://medium.com/dev-genius/passthru-monitoring-http-traffic-via-a-very-simple-proxy-implementation-with-zuul-ebe0f6f2c386) ，但是任何具有日志记录功能的 HTTP 代理都可以做到这一点)。虽然 App Search 和 Elasticsearch 之间的交流比我最初预期的要多，但还是有可能提取请求来了解 App Search 是如何工作的。

除此之外，能够访问和查询 App Search 使用的 Elasticsearch 集群对检查集群非常有帮助。Kibana 或任何其他工具在这里可能会派上用场。

# 示例使用案例

为了理解底层机制，让我们考虑一个简单的用例。假设我们有一个应用程序搜索引擎，它用一个简单的模式存储城市的信息。 ***name*** 和 ***country*** 都是字符串，假设我们在引擎中有以下文档:

现在我们试着在文档中搜索一下 ***【英国】*** 这个词。我们的应用程序搜索请求看起来很简单，如下所示:

下面你可以看到应用搜索的回复。正如您所注意到的，前两个结果在 ***【英国】*** 上有精确匹配，第三个在*上得到较弱的匹配。*

*让我们在接下来的章节中剖析这个简单的场景，并尝试探索在幕后发生了什么。*

# *引擎与指数*

*在 Elasticsearch 中， ***索引*** 是组织数据的主要构造；而应用搜索使用的概念是 ***引擎*** ，这是对弹性搜索索引的抽象。在上面的例子中，我们在一个名为 ***cities 的引擎中组织我们的数据。****

*当我们检查 Elasticsearch 中的索引名称时，我们会看到没有名为 ***城市*** 或类似名称的索引。应用搜索以不同的方式处理引擎索引抽象。这时，HTTP 日志就来帮忙了。在每次搜索操作之前，可以在 HTTP 日志中观察到如下所示的 Elasticsearch 查询:*

*App Search 首先向*发送查询。ent-search-acta stic-engines _ v9*存储引擎元信息的索引。与下面类似的响应显示了应用程序搜索引擎的重要信息(注意，为了简洁起见，响应片段中的大多数字段都被删除了)。*

*在上面的代码片段中，字段 ***id*** 特别重要。此字段是将应用程序搜索引擎与弹性搜索指数相关联的唯一标识符。在本查询中，id 为***600d 87864 ee 35962d 34 CFA 3***的 ***城市*** 引擎。*

*我们可以使用下面的 curl+GREP 命令查找具有这个独特引擎<id>的索引:</id>*

```
*curl localhost:9200/_cat/indices | grep <ID>*
```

*有三个指标与我们的 ID 相关:*

*   *。ent-搜索-文档-后端-*
*   *。ent-搜索-文档-后端-<id>-文档类型标识-外部标识-唯一-约束</id>*
*   *。ent-搜索-引擎-*

*这里我们可以推导出一个 App 搜索引擎是由后台(至少)三个 Elasticsearch 指数管理的。*

# *搜索*

*我们已经看到，一个引擎映射到至少三个弹性搜索指数。显然，前两个用于管理文档存储和唯一约束。第三个(。ent-search-engine)看起来像是用于实际搜索的索引。*

*同样，HTTP 日志是我们的朋友。在上面的例子中，我们在 App Search 中查询了关键词 ***“英国*** ”。当我们在日志中搜索关键字时，我们可以找到对*的查询。ent-搜索引擎*索引。*

*在这里，你可以看到一个复杂的弹性搜索查询，与我们极简的应用程序搜索查询相比，它有子字段、提升和突出显示。应用程序搜索的美妙之处在于，这些细节是从客户端抽象出来的，有许多选项可以定制和调整。*

*查询响应与我们在应用程序搜索响应中预期的非常相似，因此我跳过了。*

# *结论*

*在这篇文章中，我们探讨了 Elastic App Search 的一些内部情况，检查了 App Search 和 Elasticsearch 之间的 HTTP 流量。我们可以将主要发现总结如下:*

*   *应用搜索将引擎信息存储在名为*的元索引中。ent-search-acta stic-engines _ v9**
*   *每个引擎对应于弹性搜索集群中的多个指数*
*   *应用程序搜索使用。搜索查询的搜索引擎索引。*

*在接下来的文章中，我将尝试阐述一些其他应用程序搜索功能的细节。*