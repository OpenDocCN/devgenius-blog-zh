# 数据工程面临的挑战

> 原文：<https://blog.devgenius.io/challenges-faced-in-data-engineering-747580460574?source=collection_archive---------2----------------------->

每个组织和数据工程师都应该知道这些。

![](img/ef41bd33399371f8ad04a723cd161589.png)

照片由[卡丹&彼得](https://unsplash.com/@dankapeter?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

我们了解了什么是[大数据工程](/big-data-engineering-b7de4f57e0e8)、[大数据平台](/big-data-platforms-for-data-engineering-5c9016638ef8)、[数据存储](/datalake-datawarehouse-and-datamart-7b465200387)和[最佳实践](/data-lake-warehouse-mart-principles-and-guidelines-ac3f293e9a16)来构建它们。在本文中，我们将探讨工程师和组织在大数据世界中面临的普遍挑战。随着数据量、速度和种类的增加，以及技术和技术以意想不到的速度发展，他们经常面临这样或那样的挑战。

**来自多个来源的数据整合:**

随着数据来源的增加，特别是如果它们之间有一些共同点，那么将它们集成到一起以达到粒度和一致性是一个很大的挑战。

**即使对于大数据平台，数据也可能太多而无法处理:**

组织和数据工程师最近必须处理比以往更多的数据，而且没有任何饱和的迹象。当然，数据越多，对组织越有利，但如果数据超出预期，就会产生大问题。

**数据工程师的不断学习:**

近年来，我认为已经成为我们作为数据工程师面临的最大挑战之一。随着数据的增长，其存储和处理需求也在增长，有 n 多种平台、处理引擎、框架、工具等在不断创新，这迫使数据工程师时刻准备着。

**数据管道的支持和维护:**

随着数据源和数据类型的增加，对数据管道数量的需求也在增加。这些管道的支持和维护也是如此。在这种情况下，最重要的是有一个一致的设计模式和适当的自动化，如果发生错误，可以简化调试和维护。

**性能和可扩展性问题:**

随着数据的增加，对分析、建模、仪表板、报告等的需求将显著增加。如果没有使用正确的平台和工具，这将导致性能和可伸缩性问题。对于基础架构团队来说，扩展存储和处理需求极具挑战性且非常耗时。因此，数据工程团队应该负责提前做出正确的决策，以避免任何此类情况。

**数据质量:**

基于数据构建的报告、仪表盘和模型的准确性完全取决于数据的质量。如何定义和衡量数据质量有不同的方面。它们是:完整性、一致性、符合性、准确性、完整性和及时性。这些问题可以在摄取/ETL 作业时解决，也可以通过调度作业来解决，调度作业可以定期检查随时间加载的数据的这些方面。

**数据治理:**

这是任何数据工程工作中最重要的过程之一。负责团队将确保数据工程师适当遵守政策、战略和合规性法规。但是，当数据以超出预期的速度增长时，这些数据治理需求可能会成为工程师的障碍。因此，保持这种微妙的平衡无疑是一个巨大的挑战。

**数据安全:**

与治理一样，安全方面(如静态或传输中的数据加密、访问机制、数据完整性等)是任何数据工程工作中要考虑的最重要的因素。但是，如果处理不当，可能会产生许多合规和监管问题，影响数据存储的声誉。

**数据可访问性问题:**

这是一种不寻常的情况，即使我们从如此多的来源加载了数据，也可能会出现在需要时得不到数据的问题。这可能是因为 ETL/ELT 流程有问题，或者存在错误的访问控制。

**战略不明确:**

当数据工程团队和业务之间有一个清晰的策略时，业务将在数据上蓬勃发展。但经常发生的情况是，要么数据工程师不清楚他们为什么要带来一些数据，要么业务部门不清楚如何处理这些数据，从而白白浪费了资源和精力。

**人为因素/失误:**

我们可能拥有复杂/先进的工具和技术，可能拥有所有可能的自动化，但整个过程中的人为因素是无法避免的。由于人类容易犯错误，这在数据世界可能会造成一些不可逆的和昂贵的损失。

**变革阻力:**

一些遗留的程序和系统持续存在几乎令人不舒服。他们在湍急的河流中扮演着岩石的角色。但是面对一个不断变化的行业，有时这些系统会带来一些问题，这些问题可以通过一点软件升级来解决。

**缺乏对海量数据的正确理解:**

当缺乏足够的理解时，公司的大数据计划就会失败。员工可能不知道什么是数据，数据的存储、处理、重要性和来源。数据专业人员可能知道发生了什么，但其他人可能不清楚。这可能会导致数据存储中的大量数据完全未被使用或被过度使用。

**分析麻痹问题:**

这是软件工程领域的一个普遍问题，由于工具和技术的革新速度很快，为正确的工作选择正确的工具已经成为一项复杂的任务。在数据世界中，类似的任务有如此多的选择，这变得更加复杂。这在数据工程团队中产生了分析瘫痪问题，这可能会延迟开发工作，或者使他们最终选择一个完全错误的工具。

我希望已经涵盖了数据团队通常面临的大部分挑战，如果有我遗漏的任何常见挑战，请评论。既然我们已经了解了数据工程的更一般的概念，接下来我们将在接下来的文章中开始研究具体的主题。

参考资料:

[https://www . theseattledataguy . com/9-挑战-数据-工程师-面子-数据-工程-咨询/#page-content](https://www.theseattledataguy.com/9-challenges-that-data-engineers-face-data-engineering-consulting/#page-content)

[https://www.xenonstack.com/insights/big-data-challenges](https://www.xenonstack.com/insights/big-data-challenges)

[https://medium . com/data-science-at-Microsoft/common-data-engineering-challenges-and-thes-solution-DD 51872812 AC](https://medium.com/data-science-at-microsoft/common-data-engineering-challenges-and-their-solution-dd51872812ac)

[https://www.velvetech.com/blog/data-engineering-challenges/](https://www.velvetech.com/blog/data-engineering-challenges/)。

[](https://medium.com/membership/@guru.nie) [## 通过我的推荐链接加入媒体

### 阅读 Gururaj Kulkarni(以及媒体上成千上万的其他作家)的每一个故事。您的会员费直接…

medium.com](https://medium.com/membership/@guru.nie)