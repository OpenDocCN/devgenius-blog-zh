# 视频游戏销售的探索性数据分析(EDA)

> 原文：<https://blog.devgenius.io/exploratory-data-analysis-eda-of-video-game-sales-142fcba2d5be?source=collection_archive---------3----------------------->

![](img/944eae130704439d5176e447db527972.png)

Igor Karimov 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 介绍

从早期的吃豆人到最新的互动游戏，视频游戏已经成为最受欢迎的娱乐方式之一。选择合适的视频游戏控制台来满足个人的游戏需求对于充分发挥玩家的游戏体验至关重要。无论是寻求竞争优势的铁杆游戏玩家，还是想要吸引叙事世界的富有表现力的讲故事者，不同的游戏机都会给游戏玩家带来不同的东西。

视频游戏行业包括开发、出版和发行视频游戏的公司，以及生产硬件的公司。根据 [**8marketcap**](https://companiesmarketcap.com/video-games/largest-video-game-companies-by-market-cap/) 报道，一些最大的视频游戏开发商包括电子艺界、育碧娱乐公司(Ubisoft Entertainment Inc .)、动视暴雪公司(Activision Blizzard Inc .)、Take-Two 互动软件公司(TTWO)、Square Enix 控股有限公司(9684JP)和索尼电脑娱乐公司(SNE)。

今天，让我们来探讨一下游戏行业这些年来是如何发展的，并分析不同地区的不同销售模式。

# 下载数据集

首先，我们需要下载相关的数据集。我们将使用的数据集来自 [**Kaggle**](https://jovian.ai/outlink?url=https%3A%2F%2Fwww.kaggle.com%2Fgregorut%2Fvideogamesales) 。该数据集包含销量超过 100，000 份的视频游戏列表。数据集中的每一行都代表一个游戏，并包含以下功能:

*   **排名:**整体销售排名
*   **名称:**运动会名称
*   **平台:**游戏发布的平台(即 PC、PS4 等。)
*   **年份:**游戏发布的年份
*   **流派:**游戏的流派
*   **发行商:**游戏的发行商
*   北美销售额:北美销售额(百万)
*   **EU_Sales:** 欧洲销售额(百万)
*   日本的销售额(以百万计)
*   **其他 _ 销售额:**世界其他地区的销售额(百万)
*   **全球销售:**全球总销售额。

我们将使用 **opendatasets** 库直接下载数据集。

# 数据准备和清理

我们现在有了视频游戏销售额。csv 文件在我们的系统中。下一步是将它读入 python 数据帧，以便于操作。然后，我们将深入研究数据，并在此过程中执行任何必要的数据清理。为此，我们将利用 python 的[熊猫](https://pandas.pydata.org/pandas-docs/stable/user_guide/10min.html)库。

熊猫的`**。info()** `方法将为我们提供关于数据帧的列名、索引数据类型和非空值的信息。

我们可以看到有 **16598** 条目。那可是好多游戏啊！似乎还缺少一些数据。让我们使用`**。isnull()** `方法提取具有缺失值的特征及其总数。

有两个要素缺少值。由于只有 58 行缺少**发布者**(并且考虑到我们的数据量很大)，删除这些条目不会对我们的分析产生太大影响。我们可以用数据的中位年份填写缺失的**年份**列。我们通过使用`**来实现这些。dropna()** `和`**。fillna()** `分别方法。

# 探索性分析和可视化

既然我们的数据是干净和完整的，让我们进入下一个里程碑:探索性分析和可视化。

## 统计分析

用 python 的`**。describe()** `方法，我们可以对数据的数字列执行统计分析，并收集以下事实:

1.  我们正在研究从 1980 年到 2020 年的视频游戏销售数据
2.  一款游戏的全球平均销售额为 538，426 美元
3.  与其他地区相比，北美的平均游戏销量最高
4.  一款游戏的全球最高销售额达到了 8274 万美元。

## 探索年份和全球销售额之间的关系

从下面的散点图中，我们可以看到大多数游戏的全球销售额都低于大约 1500 万美元，一些特别好的游戏甚至超过了这个数字。我们甚至注意到一些异常值，特别是 2006 年发布的一款游戏取得了突破。

## 每个平台的游戏总数

**PlayStation 2** 和**任天堂 DS** 在其平台中拥有最多的游戏名称。事实上，它们在数量上几乎相等，这使得看起来更加有趣。

## 每种类型的游戏总数

动作类似乎是可以找到最多游戏名称的类型，体育类、杂耍类和角色扮演类紧随其后。益智游戏似乎是最不受游戏开发者欢迎的。

## 探索特征之间的相关性

根据下面的热图，所有销售列都应该相互关联。其他列之间似乎没有显著的相关性。

## 问:全球销量最高的 10 款游戏是什么？

## 问:销售额最高的前 5 种游戏类型是什么？

## 问:哪个游戏在每个地区和全球范围内销量最高？

## 问:哪些出版商的销售额最高？

## 问:卖出了哪些 PC-FX 游戏？

# 结论

基于我们的分析，我们可以得出以下结论:

*   **Wii Sports** 于 2006 年发布，在全球范围内销量最高。北美和欧盟地区都对此表示赞同。然而，这在其他地区是一个不同的故事，随着**口袋妖怪红/口袋妖怪蓝**在日本占据主导地位，以及**侠盗猎车手:圣安地列斯**在其他地方
*   **前 10 名游戏中有 50%** 使用了 **Wii 平台**，使其成为最成功的主机之一。这可能是因为 Wii 系统利用了动作控制的优势，是当时最具创新性的游戏机之一
*   **动作、体育和射击**是最受欢迎的游戏类型，拥有很高的游戏标题和很高的全球销量
*   任天堂抢走了全球销量最高的发行商的位置。不足为奇的是，排名前 10 的游戏都是任天堂发布的！
*   PC-FX 游戏机的武库中只有一个游戏名称，而且只在日本发行。有趣的是，这并不是销量最少的游戏。

# 参考

查看以下资源，了解有关本笔记本中使用的数据集和工具的更多信息:

*   **Kaggle 视频游戏销售数据集**:[https://www.kaggle.com/gregorut/videogamesales](https://www.kaggle.com/gregorut/videogamesales)
*   **熊猫用户指南**:[https://pandas.pydata.org/docs/user_guide/index.html](https://pandas.pydata.org/docs/user_guide/index.html)
*   **Matplotlib 用户指南**:[https://matplotlib.org/3.3.1/users/index.html](https://matplotlib.org/3.3.1/users/index.html)
*   **Seaborn 用户指南&教程**:[https://seaborn.pydata.org/tutorial.html](https://seaborn.pydata.org/tutorial.html)
*   **opendatasets Python 库**:[https://github.com/JovianML/opendatasets](https://github.com/JovianML/opendatasets)