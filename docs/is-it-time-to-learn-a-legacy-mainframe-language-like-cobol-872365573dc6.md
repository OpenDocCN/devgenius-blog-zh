# 是时候学习像 COBOL 这样的遗留大型机语言了吗？

> 原文：<https://blog.devgenius.io/is-it-time-to-learn-a-legacy-mainframe-language-like-cobol-872365573dc6?source=collection_archive---------5----------------------->

## 尽管年代久远，像 COBOL 这样的遗留大型机语言仍然为许多财富 500 强和政府部门所用。我们更深入地研究相关性。

![](img/8d7bba76f100cb754cadad4785753918.png)

通用面向商业的语言 ( **COBOL** )被认为是**遗留的大型机语言**。它是在近 60 年前开发的，并逐渐被更新、更通用的语言所取代，如 Java、C 和 Python。很少有大学开设 COBOL 课程。

然而，据路透社报道，2017 年，**如今仍在使用的银行系统中，有 43%是基于 COBOL 构建的，从未被取代**。更进一步， **80%的当面交易使用 COBOL** ，95%的 ATM 刷卡依赖 COBOL 代码。[根据 IBM 的 COBOL 队长 Tom Ross](https://www.share.org/blog/cobol-a-cornerstone-language-of-the-mainframe) 的说法，几乎所有的财富 100 强公司都通过 COBOL 应用程序运行他们的大部分数据处理，从新开户、记账、工资、预算到库存、客户交易、保险索赔和会计。

超过一半的美国州，包括加利福尼亚和纽约，仍然依赖于几十年来基于 COBOL 语言的大型机系统。当[新冠肺炎](https://www.codemotion.com/magazine/video/machine-learning-communities-in-europe-during-covid-19/)让人们呆在家里的时候，新泽西洲甚至在 4 月份召集懂 COBOL 的程序员[以回应因新冠肺炎关闭而导致的就业保险索赔猛增 1600%。(具有讽刺意味的是，年老的 COBOL 程序员很可能是死于新冠肺炎的高危人群之一)。](https://www.youtube.com/watch?v=HSVgHlSTPYQ)

# 大型机遗留语言在 COBOL 之外仍然存在

我最近和 Brandon Edenfield 谈过遗留语言的挑战，他是现代系统公司和高级 T21 公司的应用程序现代化的总经理。他注意到在全球的大型机中隐藏着数百种晦涩难懂的语言:

COBOL 实际上是大型机库中最受普遍支持和理解的过程语言。诸如 Natural、CA Gen、CA Telon、PL/I、ADS/Online 和 Assembler 等语言使公司面临更大的风险，因为人才库正在以与 COBOL 相同的速度萎缩，但要小得多。雪上加霜的是，这些语言中的大多数都需要支付昂贵的许可费，这使得本已天文数字般的大型机维护成本更加高昂。”

随着时间的推移，这些语言几乎没有变化。据布兰登所说，

Broadcom(收购了 Computer Associates，即 CA Gen 和 CA Telon 中的“CA ”)在这两个包中添加了一些支持，以支持定制的 web 服务，并围绕 Eclipse 框架迁移了更多的工具集。但是从功能上来说，**它们都和 30 年前差不多。Software AG 扩展了 Natural 及其名为 Adabas 的数据库，以生产和消费 API 并在大型机 zIIPs 上运行，但它们只是权宜之计。历史上，汇编程序被用来在大型机上定制 COBOL 等更抽象的语言不支持的功能，这种情况虽然少得多，但仍然存在。”**

布兰登解释说，“CA Telon 主要用于保险行业。CA Gen 在许多系统中使用，在欧洲各行业中出现得更多，尽管美国客户也存在。Natural 经常被保险公司和关键基础设施系统使用，其活跃客户最集中在南非。**汇编程序**可用于大型机的任何地方，但在核心银行应用中更为常见，因为它与机器代码非常接近，这使它能够非常快速地执行交易活动。”

# 人才过期的问题

这些语言每隔几年就会得到支持和轻微的扩展，但是它们“寿终正寝”的威胁是实实在在的。在对新冠肺炎的回应中，Broadcom 承认“大型机 IT 部门也不能免受人员限制的挑战。**资源短缺**使得在支持终止前完成产品预定升级的机会有限。”作为回应，他们[为 Broadcom 2020 财年停止服务的所有大型机产品版本提供了](https://community.broadcom.com/mainframesoftware/communities/community-home/digestviewer/viewthread?GroupId=2383&MessageKey=f9f0d299-57b9-4c78-af8b-9cacdd53c01f&CommunityKey=c581dd20-140f-4708-b335-eaacc07d6802&tab=digestviewer&ReturnUrl=%2Fmainframesoftware%2Fbrowse%2Fallrecentposts)有限支持。

像现代的[开发者社区](https://www.codemotion.com/magazine/dev-hub/community-manager/you-can-help-create-and-nurture-good-developer-communities/)、**一样，大多数遗留的大型机语言保持活跃的** [**社区**](https://www.codemotion.com/magazine/dev-hub/community-manager/) **论坛**，但是根据 Brandon 的说法:

"**大多数时候，希望和祈祷你的专家不会退休**。我曾与一家钢铁公司合作，该公司花费数百万美元培训了几名 Natural 顾问，因为除了负责现场系统的人之外，他们根本找不到懂这种语言的人。有一些公司专门从事 CA Gen，并会以高昂的价格提供专业知识，但这是一个极其不稳定的情况。这种知识的缺乏、将概念转化为现代开发人员易于理解的内容的能力的缺乏以及宏观需求的缺乏是对使用它们的公司的最大威胁。”

# Linux 基金会的开放大型机项目可以提供帮助

[Open Mainframe Project](https://www.openmainframeproject.org/) 是一个开源项目，支持大型机社区之间的协作，以开发共享的工具集和资源。为了响应公共部门官员的号召，他们创建了:

1.  [**COBOL 程序员论坛**](https://community.openmainframeproject.org/c/calling-all-cobol-programmers/15)——愿意成为志愿者或可以被雇佣的开发人员和程序员可以发布他们的个人资料。无论是积极找工作的，退休的有技能的退伍军人，成功完成 COBOL 课程的学生，还是想做志愿者的专业人士。雇主可以根据需要连接这些资源。
2.  [**COBOL 技术论坛**](https://community.openmainframeproject.org/c/cobol-technical-questions/16) —一个专门针对 COBOL 技术问题的新论坛，将由有经验的 COBOL 程序员监控。程序员可以快速学习新技术，并利用广泛的专业知识来解决常见的问题和挑战。
3.  [**开源 COBOL 培训**](https://github.com/openmainframeproject)——一个新的开源项目，在 COBOL 培训材料上进行合作。IBM 提供该课程是为了回应其与客户和高等教育机构的合作。

# 为什么要学习传统的大型机语言？

学习传统大型机语言有几个**好处**:

根据 Micro Focus 的 Derek Britton 的说法，COBOL 是“最容易学习和阅读的语言”。他断言 COBOL 是“执行速度最快、性能最高的语言”。COBOL 具有 38 位数字的算术精度、强大的数据操作、排序能力、高性能和健壮的错误管理。因此，COBOL 在各个行业都保持着无可匹敌的稳健记录。

Tom Ross 注意到“COBOL 不仅能轻松处理金钱，还能处理 XML 文档和 JSON 文本。它可以与 Java 和 C 程序以及 IBM 和其他公司提供的许多其他应用程序开发工具进行互操作。”

Compuware 公司的首席执行官克里斯托弗·欧玛利建议说:“简单的供求法则使掌握大型机知识在经济上更有价值。尤其是与更受欢迎的平台(如移动平台和网络平台)上广泛可用的商品化技能相比。”

此外，**增加你的维护知识和能力与其他学习机会并不对立**。相反，它创造了一种技能组合，随着年长从业者退休，这种组合将变得越来越重要。任何巩固其知识的人也将享受维护和现代化现有传统大型机的挑战。

*如果你想了解更多关于如何在 COBOL 职业生涯中过渡的信息，或者你只是对这个话题好奇，不要错过参加我们即将于 2020 年 10 月举行的* ***Codemotion 在线技术大会*** *的机会！领取您的* ***免费票*** *并查看议程* [*点击这里*](https://events.codemotion.com/conferences/online/2020/codemotion-online-tech-conference/) *！*

*原载 9 月 4 日为*[*code motion*](https://www.codemotion.com/magazine/dev-hub/backend-dev/learn-cobol-mainframe/)