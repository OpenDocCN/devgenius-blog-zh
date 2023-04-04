# 实现高级高效 MongoDB 分页的正确方法

> 原文：<https://blog.devgenius.io/the-right-way-to-make-advanced-and-efficient-mongodb-pagination-8bfccac9baf0?source=collection_archive---------1----------------------->

如何在没有额外依赖的情况下进行干净的分页。基本上用于 Node.js，但也可以用于任何其他语言和平台。

![](img/44065be7b1c023eba53fbf41913f148e.png)

由 [Olga Tutunaru](https://unsplash.com/@otutunaru?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

曾几何时，我们有一个足够复杂的项目(拼车和打车应用),包括 stack Node.js 和 MongoDB。我们选择这个堆栈是因为客户更喜欢它，我们的团队也很了解它，同时它看起来也是一个很好的项目任务套件。

一切都很棒，用户数量超过了 12000 人，活跃司机的数量接近 300 人。一年之内，乘坐次数超过了两百万次。

但是一旦我们需要创建一个管理面板来控制和监视主应用程序中的所有进程(从业务的角度来看)。很大一部分需求是拥有各种实体的高级列表，并对它们进行绑定统计。

因为我们用的是[猫鼬](https://www.npmjs.com/package/mongoose)，作为 ODM，首先我们看一下它的插件。其中最流行的与分页相关的是

[](https://www.npmjs.com/package/mongoose-paginate) [## 猫鼬-分页

### mongose 的分页插件注意:这个插件只适用于 Node.js >= 4.0 和 Mongoose > = 4.0。将插件添加到…

www.npmjs.com](https://www.npmjs.com/package/mongoose-paginate) [](https://www.npmjs.com/package/mongoose-paginate-v2) [## 猫鼬-分页-v2

### 一个基于光标的自定义分页库，用于带有可自定义标签的 Mongoose。

www.npmjs.com](https://www.npmjs.com/package/mongoose-paginate-v2) [](https://www.npmjs.com/package/mongoose-aggregate-paginate) [## 猫鼬-聚合-分页

### mongose-aggregate-paginate 是一个易于为聚合添加分页的 mongose 插件。这个插件可以用在…

www.npmjs.com](https://www.npmjs.com/package/mongoose-aggregate-paginate) [](https://www.npmjs.com/package/mongoose-aggregate-paginate-v2) [## 猫鼬-聚合-分页-v2

### 一个基于光标的自定义聚合分页库，用于带有可自定义标签的 Mongoose。如果您正在寻找基本的…

www.npmjs.com](https://www.npmjs.com/package/mongoose-aggregate-paginate-v2) 

另一个需求是可以按需选择特定页面，因此使用“ *previous-next* ”的方法，如分页，基于游标的方法立即被禁止——它的*mongose-paginate-v2*和*mongose-aggregate-paginate-v2*库。

最古老的，也可能是最简单的用法是*mongose-paginate*——它使用简单的搜索查询、限制、排序和跳过操作。我想这是简单分页的一个很好的变体——只需安装一个插件，向您的端点添加几行代码，就这样——工作就完成了。它甚至可以使用 mongoose 的“*populate*”——模拟 SQL 世界中的连接。从技术上讲，它只是对数据库进行额外的查询，这可能不是您想要的方式。更有甚者，当你有一个稍微复杂一点的查询时，任何数据转换都是完全不可用的。我只知道在这种情况下通常使用它的一种方法——首先创建 [MongoDB 视图](https://docs.mongodb.com/manual/core/views/)——从技术上讲，它是预先保存的查询，MongoDB 将其表示为只读集合。然后使用 mongose-paginate 对这个视图进行分页。不错——您将在视图下隐藏复杂的查询，但是我们有一个如何解决这个问题的更好的主意。

**MongoDB 聚合框架来了！**

你知道，我想，当 Aggregation Framework 发布的时候，对于 MongoDB 社区来说，这是真正的一天。可能它允许你能想到的大多数查询。因此，我们考虑将*mongose-aggregate-paginate*投入使用*。*

**但是接下来的两件事让我们失望了:**

**这个插件需要什么？**我的意思是——它有助于解决哪些没有这个插件就无法解决的任务。看起来它只是你的项目中的一个额外的依赖，因为它不会带来任何利润，甚至不会节省你的时间…

**内部代码库，以及进行查询的一般方法**。这个库通过 *Promise.all* 对数据库进行**两次**调用并等待响应。第一—获取查询结果，第二—计算查询返回的总记录数，没有 **$filter** 和 **$limit** 阶段。它需要这个来计算总页数。

我们如何避免对数据库的额外查询？最糟糕的是，我们需要将所有聚合管道运行两次，这在内存和 CPU 使用方面的成本非常高。更有甚者，如果收集量很大，文档往往只有几兆字节，这会影响磁盘 I/O 的使用，这也是一个大问题。

好消息是——聚合框架有一个特定的阶段可以解决这个问题。是 **$facet:**

> *处理多个* [*聚合管道*](https://docs.mongodb.com/manual/core/aggregation-pipeline/#id1) *在同一组输入文档的单个阶段内。每个子管道在输出文档中都有自己的字段，其结果存储为文档数组。*

[关于$facet stage 的 MongoDB 文档](https://docs.mongodb.com/manual/reference/operator/aggregation/facet/)。

用于分页的聚合管道将具有下一个形状:

$刻面舞台结构(来自[https://docs.mongodb.com/](https://docs.mongodb.com/))

此外，可以通过针对特定情况进行定制来改进分页管道。下面列出了一些提示:

*   在所有可能的过滤器( *$match* stages)之后，运行所有不直接影响最终分页结果的操作。像$project 或 *$lookup* 这样的阶段不会改变结果文档的数量或顺序。尝试一次剪切尽可能多的文档。
*   尽量让你的模型更加自给自足，以避免额外的 *$lookups* 。复制一些数据或者做预计算字段很正常。
*   如果你有一个非常大的管道，处理很多数据，你的查询可能会使用超过 100MB。在这种情况下，需要使用 ***allowDiskUse*** 标志。
*   遵循聚合管道性能[优化指南](https://docs.mongodb.com/manual/core/aggregation-pipeline-optimization/)。这个建议可以帮助你提高查询效率。
*   从技术上讲，您可以在应用程序代码端进行动态查询，这取决于您可以添加、删除或修改特定阶段的条件。它可以加快你的查询速度，而且使你的代码更有说服力。

因为 NDA，我不能给你展示真正的数据库模式和真正的查询。但是让我给你看一个这种分页的小例子。

假设您有两个集合— **统计数据**和**驱动数据**。*驱动*集合足够静态，考虑到不同文档中字段的类型和数量。但是*统计*是多态的，可以随着业务需求的更新而改变。此外，一些驱动程序可能有大量的统计文档和历史记录。所以你不能作为司机的子文件进行统计。

因此代码和 MongoDB 查询将具有下一个形状:

如您所见，使用*聚合框架*和 *$facet* 阶段，我们可以:

*   进行数据转换和复杂查询；
*   从多个集合中提取数据；
*   在一个查询中获取分页元数据(总计、页数、页数),而无需执行额外的查询。

关于这种方法的**主要缺点**，我想只有一个是主要的——***更高的开发和调试过程的复杂性，以及更高的进入门槛*** 。它包括性能故障排除、各种阶段的知识以及数据建模方法。

因此，基于 MongoDB 聚合框架的分页并不是一种银弹。但是经过多次尝试和尝试之后——我们发现这个解决方案覆盖了我们所有的情况，没有任何效果，也没有与特定库的高度耦合。