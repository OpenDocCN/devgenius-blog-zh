# 在 Microsoft Power BI 上按月和按年比较销售额

> 原文：<https://blog.devgenius.io/comparing-sales-by-month-and-year-on-microsoft-power-bi-fee01fe0d857?source=collection_archive---------5----------------------->

在这篇文章中，比较了去年和今年，以及上个月和本月在微软智能仪表板上的显示。Excel 中的 Power Query 用作数据收集和一些 ETL (Extract Transform Load)操作。其他步骤包括 dashboard 和 DAX 计算，用于创建 Power BI 中当前和去年或上个月的度量。与这个职位的目标是贡献，以便使电子商务项目的销售报告容易。

![](img/e470917ba5ecb258c47db04b17b68bbe.png)

1.  **电力查询中的数据采集**

T 他的工具同时用在 Power BI 和 Excel 上。在这篇文章中，在 Excel 的幂查询中创建的模型。你也可以访问微软文档使用电源查询。[【1】](https://learn.microsoft.com/en-us/power-query/power-query-ui)。关系数据库管理系统(RDBMS)产生于结构化数据。该系统包含模式和表。在这个项目中涉及到结构化数据作为表中的两个特征。日期和金额列是特性，它们存储在 Excel 文件中。因此，它可以在超级查询中处理。这个幂查询有几个步骤。

单击获取数据并启动超级查询编辑器选项。

您也可以在 Power BI 中遵循相同的步骤。因为 Power BI 具有与 Excel 相同的 Power 查询框架。

![](img/2c203ab7cb82e790d523cc0058102dcd.png)

有一些 ETL 处理。日期类型的转换和金额列的小数转换。你可以在屏幕右边看到一些编辑。

![](img/758c4277bfa1ecfe3707daee3cfdbad7.png)

另一层是关闭和加载。当您完成更改和一些操作处理后，您可以选择编辑过的表。

![](img/ca93a828efb5b0da73b893edfe319f22.png)

**2。DAX 与 Power BI 仪表板步骤**

这一步包括一些达克斯公式。

名为“CurrentMontSales”测量区域给出了本月日期间隔内总销售额的信息。

如果这一天不等于月初，如 2022 年 11 月 1 日，销售额总和将给出本月和昨天的总结果区间开始日期。

如果这一天的日期等于月初，销售额总和将给出昨天和昨天前一个月的总结果区间。

此外，您还可以使用网站中的导游 EDATE 功能。[【2】](https://learn.microsoft.com/en-us/dax/edate-function-dax)

“CurrentYearSales”测量区域显示了当前年度的总销售额。

按时间间隔过滤的昨天和今年年初(2022 年 1 月 1 日)的总销售额。

在 DAX 中创建的“上个月销售”衡量区域，用于在一个月销售之前进行演示。

如果这一天不等于月初，则金额值返回一个月前的间隔日期和一个月前的开始日期。

“LastYearSale”度量区域获取关于上一年开始和前一年的昨天日期之间的时间间隔的销售额的信息。

Power BI dashboard 展示了 DAX 公式，您可以看到今年/去年和本月/上月总销售额的对比。

![](img/2b5bd5400386530b066d11a511bf8dda.png)

此外，你可以访问 Youtube 频道，了解这份对比销售报告。

**结论**

简而言之，这篇文章展示了数据摄取到 Power 查询和数据操作处理、DAX 公式和与仪表板比较的步骤。上一年/月与本年/月的销售额与图表进行比较。此项目常用于在电子商务业务中进行可视化。

**参考文献**

[1][https://learn . Microsoft . com/en-us/power-query/power-query-ui](https://learn.microsoft.com/en-us/power-query/power-query-ui)

[https://learn.microsoft.com/en-us/dax/edate-function-dax](https://learn.microsoft.com/en-us/dax/edate-function-dax)