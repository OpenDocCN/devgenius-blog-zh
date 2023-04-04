# Tableau 快速提示:动态图表标题

> 原文：<https://blog.devgenius.io/tableau-quick-tip-dynamic-chart-title-b86a1585df81?source=collection_archive---------7----------------------->

创建一个下拉菜单来更改 x 轴和 y 轴。根据选择更改图表标题。

![](img/699e9203675b12e403ee909125856b3d.png)

# 目录

> [简介](#3cb4)
> [数据&工作簿](#89c7)
> [创建条形图](#c104)
> [创建参数](#d614)
> [创建计算字段](#324a)
> [设计图表](#0019)
> [最终结果](#cb37)
> [视频教程](#d5d6)
> [最终文字](#1561)

# 介绍

Data Analyst 有时会为不知道如何使用 Tableau Desktop 的人(查看者)创建一个仪表板。我们希望允许用户使用下拉菜单来切换 x 和 y 轴，以提供便利。

除此之外，我们希望图表标题说明用户选择了哪个维度或度量。

# 数据和工作簿

数据:[g 驱动链接](https://docs.google.com/spreadsheets/d/1o-KaA43TD6Q_w_vnyEOIzaR6xMTmR9nd/edit?usp=sharing&ouid=105166131516527752739&rtpof=true&sd=true)

工作簿: [Tableau 公共链接](https://public.tableau.com/app/profile/foo.chee.chuan/viz/DynamicChartTitle/DynamicChartTitle)

Youtube: [车志川](https://www.youtube.com/channel/UC73tyElpkCE_6cbZxMLKnkw)

# 创建条形图

![](img/220eaf5ddcd5a409ce42d97050b010d9.png)

创建条形图

# 创建参数

参数用于在 Tableau 中创建下拉列表。因此，我们需要创建两个参数，一个用于尺寸，另一个用于测量。

![](img/fa7df16da73273b5a8d84c9b7b0900a8.png)

下拉列表选择尺寸和测量

![](img/12880786edc56a678752d36b37f8c0f5.png)

创建参数

**参数 1:选择尺寸**

![](img/f2635db60dbbea01eeda2c4263a9da80.png)

参数 1:选择维度

**参数 2:选择测量**

![](img/9ac6e7a75212bf20d441842dee7061a7.png)

参数 2:选择衡量标准

![](img/f9bd94ceab767c99f6923ffcc8add7d7.png)

"显示参数"创建下拉菜单

# 创建计算字段

![](img/a136a6b58edb77acec600b99eab74f5b.png)

创建计算字段

当用户在“**选择维度**”下拉菜单中选择“**类别**”时，该计算字段将等同于“**类别**字段。也就是说，我们需要在下拉列表中列出所有值，以便当用户选择这些字段中的任何一个时，图表可以根据选择而改变。同样的逻辑也适用于“**选择度量**”。

![](img/ef1f252ecf5a577e15078b8804d6e802.png)

创建计算字段:选择维度

![](img/5bdd0794c63b5a4a8fe9fa2506b6202d.png)

创建计算字段:选择度量

> *注意，每个聚合需要单独指定为**选择度量**

# 设计图表

将尺寸和测量替换为“**选择尺寸**”和“**选择测量**

![](img/481ba76c455cc875f706daccedf2245a.png)

拖到行和列架上

尝试在下拉列表中选择不同的值。图表应该根据您的选择动态变化。

我们希望通过使图表标题、字段标签和轴标题动态化来改善用户体验。

![](img/e9258450b86a7893154f364a28863b02.png)

动态特征

**动态图表标题**

1.  双击图表标题
2.  移除默认
3.  使用如下所示的“插入”按钮插入参数
4.  改变参数的颜色。改变颜色前，记得突出显示参数

![](img/8c96a7c33b7369f88c5c1c9c37b7095d.png)

动态图表标题

**动态字段标签**

1.  隐藏字段标签

![](img/11c88f4b90da233c256ade14fe83b8aa.png)

2.将“选择维度”参数拖到行中。** *将其置于“选择尺寸”字段之前。*

![](img/33cf0ac54260ab3ea9743b5045b3fce3.png)![](img/9962c075c84b981df9e36b90490af9d9.png)

右键单击要格式化的字段标签

![](img/12483a604a0c61dce4a3418a72cc75c2.png)

居中对齐

**动态轴标题**

1.  删除轴标题

![](img/65b57d7abd377a8e56c8f31b06cca0ad.png)

右键单击->编辑轴

![](img/61fa96d846857c202b3498571ce7018b.png)

删除轴标题

2.将“选择度量”添加到列

![](img/7faeca8b67f5f3780de21cc51ff8b756.png)

将“选择度量”添加到列

3.隐藏字段标签

![](img/b8611c5bda07270444d3f716b0408156.png)

右键单击->隐藏列的字段标签

# 最后结局

此时，你已经创建了一个动态视觉它的特性会根据用户的选择而改变。

![](img/0b056e5b29fd8e465c72ea5c5143bd1a.png)

动态项目以红色方框突出显示

# 视频教程

Youtube 上的视频教程

# 最后的话

谢谢你把这篇文章读到最后。如果你想收到类似的内容，请关注并订阅我的媒体账户。直到下一个，再见:)

![](img/f614f14bea52db82ee1b1b7f63fadc52.png)

照片由[封泥](https://unsplash.com/@milestogobeforeisleep?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/farewell?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上