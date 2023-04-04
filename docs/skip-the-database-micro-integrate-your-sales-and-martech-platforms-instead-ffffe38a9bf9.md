# 跳过数据库，“微集成”您的销售和 MarTech 平台

> 原文：<https://blog.devgenius.io/skip-the-database-micro-integrate-your-sales-and-martech-platforms-instead-ffffe38a9bf9?source=collection_archive---------10----------------------->

孤立的数据对销售和营销团队来说是死刑判决。

几乎每个 B2B 企业都运行营销和销售软件，如 Salesforce、Marketo 和 Adobe Analytics。不幸的是，这些工具不能开箱即用地相互通信。这导致数据孤岛，使得基于账户的个性化等关键营销策略面临挑战。

克服这个挑战的最好方法是什么？当然是整合你的数据。

说起来容易做起来难。

通常，数据工程师通过使用 ETL(提取、转换、加载)工具将来自销售和营销系统的数据集中到一个数据库中来实现这一点。在集中数据后，工程师将数据转换成销售和营销团队可以使用的数据集。这一过程不仅费时费钱，而且需要对所涉及的所有系统的复杂性有广泛的了解。

我职业生涯的大部分时间都是用这种方法从零开始构建营销和销售数据流。作为一名营销分析专家，我个人接触过在最低技术粒度水平上使营销和销售系统协同工作的各个方面。

在亲身经历了这类工作所涉及的大量技术“繁重”和挫折之后，我决定开发一种方法来完全跳过它，我称之为“微集成”。

微集成从一个系统获取数据，并将其推送到另一个系统——就这么简单。然后，数据在两个系统中都是可用的和可操作的。如果营销人员想要确定一项活动产生了多少 MQLs(营销合格线索),微集成可以使该数据在她已经使用的系统中可用。她不再需要数据分析团队来处理检索数据并将其放入控制面板的复杂工作。微集成避免了数据存储开销，避免了劳动密集型的数据转换，并且比 ETL 方法花费的时间更少。

使用案例:一位营销总监想要分析哪些活动在创造新的销售渠道方面最有效。

典型的数据集中化流程:数据工程团队、数字网络团队、营销团队和 salesforce 管理员开会收集需求并确定项目范围。web dev 团队与 Salesforce 团队合作，在网站上放置 JavaScript 标签，将 Salesforce IDs 推入 Adobe Analytics，将 Experience Cloud IDs 推入 Salesforce。然后，数据工程团队将编写一个管道，接入 Adobe Analytics 和 Salesforce API，并将数据推入数据库。然后，数据被转换并加载到仪表板或电子表格中。最后，营销总监有了一个可用的数据集。

微集成:营销总监在 Datajoin 会见微集成专家。她选择对她的需求最关键的用例。数据在 1-2 周内开始在 Salesforce 和 Adobe Analytics 之间自动流动。

通过专门化传统的数据处理过程，微集成最小化了需要数据的营销人员或销售人员与实际得到数据之间的摩擦。

过去 20 年来，我一直在开发数据和分析工具，旨在解决营销和销售团队面临的最严峻挑战。欲听更多关注我上 [*推特*](https://twitter.com/samfonoimoana) *和* [*领英*](https://www.linkedin.com/in/sam-fonoimoana) *。要了解更多关于微集成的信息，请访问*[](https://datajoin.com/)*[*datajoin.com*](http://datajoin.com)*

**在我的* [*类型共享社交博客*](https://typeshare.co/samfonoimoana/posts/skip-the-database-micro-integrate-your-data-instead--u56q) 上阅读这篇文章和更多内容*