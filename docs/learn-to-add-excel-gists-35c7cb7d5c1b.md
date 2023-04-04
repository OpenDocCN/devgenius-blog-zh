# 学习添加 Excel 表格

> 原文：<https://blog.devgenius.io/learn-to-add-excel-gists-35c7cb7d5c1b?source=collection_archive---------7----------------------->

## GITHUB GIST

## 如何在中型文章中添加可搜索的表格？

![](img/3d794ee5728935455220a406145a4ce2.png)

来自[佩克斯](https://www.pexels.com/photo/person-writing-on-notebook-669615/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)的[卢卡斯](https://www.pexels.com/@goumbik?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)的照片

# 我一直想知道如何将表格数据添加到一个中等的故事中？

人们可以考虑用竖线`|`和连字符`-` 创建表格，如下所示

```
| First Header  | Second Header |
| ------------- | ------------- |
| Content Cell  | Content Cell  |
| Content Cell  | Content Cell  |
```

## 最近发现 Github Gist 两者都支持”。tsv“&”。csv "文件类型在表格中显示信息。

任何*。csv* 或*。提交给 GitHub gist 的 tsv* 文件自动绘制一个交互式表格，带有标题和行数。

> 默认情况下，第一行被假定为标题行。

## 请在下面找到表格结构的样子:

## 首先，学习如何将 GitHub Gist 添加到文章中。

[](https://medium.com/@singhsukhpinder/an-alternative-way-to-add-multiple-spaces-or-use-tab-key-on-medium-story-82a3abdb51c8) [## 添加多个空格的另一种方法，或者在中页上使用 tab 键

### 由于 medium story 不支持一个以上的空格或缩进，下面是添加空格或缩进的另一种方法。

medium.com](https://medium.com/@singhsukhpinder/an-alternative-way-to-add-multiple-spaces-or-use-tab-key-on-medium-story-82a3abdb51c8) 

# Github Gist 支持以下格式:

*   CSV(逗号分隔值)
*   TSV(制表符分隔值)

# 逗号分隔值

对于 CSV 格式，数据集格式规则。

*   逗号分隔这些值。
*   每行必须有相同的列数。
*   文件扩展名必须是"。CSV "

请在下面找到一个样本数据集:

```
Day,Time,Direction,Premium
Weekday,morning rush,inbound,x 2.00
Weekday,morning rush,outbound,x 1.00
Weekday,daytime,inbound,x 1.50
Weekday,daytime,outbound,x 1.50
Weekday,evening rush,inbound,x 1.00
Weekday,evening rush,outbound,x 2.00
Weekday,overnight,inbound,x 0.75
Weekday,overnight,outbound,x 0.75
Weekend,morning rush,inbound,x 1.00
Weekend,morning rush,outbound,x 1.00
Weekend,daytime,inbound,x 1.00
Weekend,daytime,outbound,x 1.00
Weekend,evening rush,inbound,x 1.00
Weekend,evening rush,outbound,x 1.00
Weekend,overnight,inbound,x 1.00
Weekend,overnight,outbound,x 1.00
```

## 现场演示

![](img/71780d0dd221341098f4de12f56a392c.png)

# 制表符分隔的值

对于 TSV 格式，数据集格式规则。

*   制表符分隔这些值。
*   每行必须有相同的列数。
*   文件扩展名必须是"。tsv "

请在下面找到一个样本数据集:

```
Day Time Direction Premium
Weekday morning rush inbound x 2.00
Weekday morning rush outbound x 1.00
Weekday daytime inbound x 1.50
Weekday daytime outbound x 1.50
Weekday evening rush inbound x 1.00
Weekday evening rush outbound x 2.00
Weekday overnight inbound x 0.75
Weekday overnight outbound x 0.75
Weekend morning rush inbound x 1.00
Weekend morning rush outbound x 1.00
Weekend daytime inbound x 1.00
Weekend daytime outbound x 1.00
Weekend evening rush inbound x 1.00
Weekend evening rush outbound x 1.00
Weekend overnight inbound x 1.00
Weekend overnight outbound x 1.00
```

## 现场演示

![](img/db8d7fd2eb23e7395bd6fed5a843398d.png)

## 特征

*   增加可读性。
*   易于管理。
*   像搜索和行号这样的功能可以提高生产率。
*   他们很容易与其他人分享。
*   可水平滚动，用于更大规模的数据集。

> 感谢您的阅读。我希望你喜欢这篇文章..！！