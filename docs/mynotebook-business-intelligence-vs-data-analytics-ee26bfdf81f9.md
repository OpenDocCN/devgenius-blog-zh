# 我的笔记本:商业智能与数据分析

> 原文：<https://blog.devgenius.io/mynotebook-business-intelligence-vs-data-analytics-ee26bfdf81f9?source=collection_archive---------1----------------------->

本周的笔记总结了数据分析和商业智能的交集。我将简要讨论这两个术语，并强调其中的相似之处和不同之处。

![](img/fdcdca42f9c0048355b09cfe1e61f455.png)

图片来自[https://www . inz ATA . com/data-science-vs-business-intelligence-what-the-difference/](https://www.inzata.com/data-science-vs-business-intelligence-whats-the-difference/)

D 数据分析(DA)被描述为数据科学，它是检查数据集以发现趋势并从数据中得出有关信息的结论的过程。该术语也适用于商业智能(BI)和其他高级分析。数据分析的目标是提供洞察，为企业以及科学、政府和教育等其他领域的决策提供信息。

数据分析改进流程，帮助识别机会和趋势，推出新产品，服务客户，并做出数据驱动的决策。数据分析方法包括 ***探索性数据分析(EDA)*** ，旨在发现数据中的模式和关系；以及 ***验证性数据分析(CDA)*** ，指的是应用统计技术来确定关于数据集的假设是真是假。一些文本将四种类型的数据分析确定为

***描述性分析:*** 提供对过去事件的客观的、基于事实的描述；x 发生了，然后 Y 发生了。

***诊断分析学:*** 旨在理解为什么会发生，超越过去发生的事情；Y 不是因为 X 而发生，而是 Z 是 Y 发生的原因。

***预测分析:*** 利用历史数据预测趋势，对未来做出预测；因为 Y 因为 Z 而发生，我们预测 Y 会在未来发生，因为 Z 正在发生

***规定性分析:*** 旨在提供实现特定目标的可操作步骤；为了防止 Y 发生，我们必须采取行动 a。

![](img/6f4e50c853f82cf365035588e9c7af3b.png)

图片来自[https://ppce xpo . com/blog/business-intelligence-vs-data-analytics](https://ppcexpo.com/blog/business-intelligence-vs-data-analytics)

B 商业智能(BI)包括 ***策略和技术*** ，企业使用这些策略和技术来识别、发现和分析商业数据，以做出明智的决策。商业智能工具允许利益相关者访问各种类型的数据——过去的数据、当前的数据、内部数据、来自第三方的数据、结构化和半结构化数据，以了解业务绩效。

> 商业智能 ***利用数据仓库*** 获取基线信息。数据仓库(DW)是来自多个数据源的历史数据的存储库，这些数据源合并到一个中央系统中，以帮助企业通过业务分析和报告做出明智的决策。它作为事实的单一来源，通常被称为企业数据仓库(EDW)。

BI 帮助企业提高绩效并获得竞争优势。BI 工具还可以自动化数据收集、分析和可视化过程，从而节省宝贵的时间和精力。商业智能中涉及的步骤描述如下:

## 从多个数据源收集和转换数据

BI 技术使用 ETL 过程来聚合来自多个数据源的结构化和非结构化数据。数据在存储到中央位置(数据仓库)之前经过转换和重新建模，以便于查询和分析。

## 发现趋势和矛盾

BI 工具通常包括几种类型的数据建模和分析，用于进一步探索数据、预测趋势和提出建议。

## 使用数据可视化呈现调查结果

BI 报告使用数据可视化，使调查结果更易于理解和共享。它通常涉及使用交互式仪表板、图表、图形和地图来帮助利益相关者查看实时业务绩效。

## 实时根据洞察采取行动

在业务和相关活动的背景下，查看当前数据并将其与历史数据进行比较，使公司能够迅速从洞察转向行动。它增强了实时调整和长期战略变化，适应市场变化，纠正供应问题，并解决客户问题。

> 虽然数据分析经常利用商业智能中常见的演示功能(如仪表板和报表)，但它们不是数据分析的基本方面，而是有用的附加功能。

企业非常依赖数据分析，但数据分析虽然在企业中被大量使用，但在没有业务数据的情况下也能很好地运行。

![](img/1b04208e451369b268c0a30987824b83.png)

商业智能和数据分析的区别

让我简单介绍一下企业数据仓库及其架构

![](img/17067ef90c58d688e3b2cc8f173d3954.png)

数据仓库架构框架的图像

EDW 是一种经过优化的关系数据库，用于存储数据以便进行后期分析。它在物理上或虚拟上和功能上独立于用于日常操作和交易的主数据库管理系统。数据仓库是为创建特定的企业范围的报告而保留的，这些报告通过提供关于业务的综合和及时的知识来支持决策。

数据仓库从多个数据源中提取数据，然后从数据源中提取*(OLTP—联机事务处理)*【转换】* ，然后 ***加载*** 到 OLAP —联机分析处理。运营数据存储(ODS)的保留期比 OLTP 长，用于运营和业务报告。暂存环境充当临时存储，在非高峰时段将数据加载到其中，以避免业务中断。*

*数据集市是数据仓库的一种简单形式，它专注于业务的一个功能领域，如销售、营销和生产。它通常由组织内的一个部门构建和控制。数据源可以是内部操作系统、中央数据仓库或外部数据源。有两种类型的数据集市:相关的和独立的数据集市。*

*未来更多关于数据集市的信息。*

**参考文献**

*Owolabi，S.A .商业智能基础(2022)*

*o .西奥博尔德,《初学者数据分析》( 2019 年)*

*[https://www.ibm.com/topics/business-intelligence](https://www.ibm.com/topics/business-intelligence)*

*[https://power bi . Microsoft . com/en-ca/what-is-business-intelligence/](https://powerbi.microsoft.com/en-ca/what-is-business-intelligence/)*

*[https://career foundry . com/en/blog/data-analytics/business-intelligence-vs-data-analytics/# what-is-business-intelligence](https://careerfoundry.com/en/blog/data-analytics/business-intelligence-vs-data-analytics/#what-is-business-intelligence)*

*[https://hevo data . com/learn/business-intelligence-vs-data-analytics/](https://hevodata.com/learn/business-intelligence-vs-data-analytics/)*