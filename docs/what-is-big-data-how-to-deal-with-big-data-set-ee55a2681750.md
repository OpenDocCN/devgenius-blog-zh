# 什么是大数据？如何处理大数据集？

> 原文：<https://blog.devgenius.io/what-is-big-data-how-to-deal-with-big-data-set-ee55a2681750?source=collection_archive---------22----------------------->

使用流行的 Python 库

![](img/50baa348d197f9a46dd377b99bfc6d42.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Usukhbayar Gankhuyag](https://unsplash.com/@usukbayer?utm_source=medium&utm_medium=referral) 拍摄

大数据是指以 5v 为特征的数据类型(参见[天文和对地观测大数据中的知识发现](https://www.sciencedirect.com/book/9780128191545/knowledge-discovery-in-big-data-from-astronomy-and-earth-observation)):

*   卷—大量生成的数据。
*   速度—快速生成的数据。
*   多样性——以多种方式生成的数据。
*   有效性—数据需要努力确定其实际质量或可信度。
*   价值—数据需要努力确定它们在指定上下文中的实际含义。

大数据来自多种来源(交易处理系统、客户数据库、文档、电子邮件、医疗记录、互联网点击流日志、移动应用和社交网络)，它们包括多种类型的数据格式，如机器生成的数据(网络和服务器日志文件以及来自制造机器上的传感器的数据)、工业设备和物联网设备(参见 [TechTarget-大数据示例](https://www.techtarget.com/searchdatamanagement/definition/big-data#:~:text=examples%20of%20big%20data))。

Archive Org 保存了互联网上的大量数字媒体，包括网页、音频、视频等。Kaggle [提供超过 50，000 个公共数据集和 400，000 个公共笔记本](https://www.kaggle.com/general/260690)。福布斯列出了 [33 个全球数据源](https://www.forbes.com/sites/bernardmarr/2016/02/12/big-data-35-brilliant-and-free-data-sources-for-2016)。AwesomeData 提供了 36 个以主题为中心的数据源。所有这些数据源都为数据科学家提供了执行多种大数据处理的新机会。但是他们如何处理这些大量的、多样化的信息呢？

大数据处理涉及的步骤与事务或数据仓库环境中的数据处理非常相似(参见大数据时代的中的[数据仓库),包括四个步骤:](https://www.sciencedirect.com/book/9780124058910/data-warehousing-in-the-age-of-big-data)

1.  收集数据。
2.  分析数据。
3.  处理数据。
4.  分发数据。

大数据处理任务也可以从数据库的角度来看(参见[ETL 中的数据质量——初步研究](https://www.sciencedirect.com/science/article/pii/S1877050919314097))，它由三个任务组成:

1.  摘录。
2.  转变。
3.  加载。

最近几年，Python 编程在大数据处理领域越来越受欢迎(参见[社会计算的高级数据挖掘工具和方法](https://www.sciencedirect.com/book/9780323857086/advanced-data-mining-tools-and-methods-for-social-computing))。

支持 ETL 任务的一些有用的 Python 库如下:

1.  摘录:
    *请求
    * Wget
    * URL lib
    * beautiful soup
    * Scrapy
    *熊猫
2.  transform:
    * Numpy
    * Scipy
    * Matplotlib
    * Scikit-Learn
    * tensor flow
    * Keras
    * py torch
    * PyDoop
    * Pyspark
3.  加载:
    * SQL 数据库
    * NOSQL 数据库
    *其他

大数据处理通常是通过一个叫做 Jupyter 的开发环境来完成的。这个名字实际上是大数据处理中三种编程语言的杜撰词，即 Julia、Python 和 R(参考 [I Python，You R，We Julia](https://blog.jupyter.org/i-python-you-r-we-julia-baf064ca1fb6) )。

开发人员有很多方法可以运行 Jupyter:

1.  直接安装在本地机器上。
2.  在 Docker 容器中运行。
3.  在云端运行。

在云中运行 Jupyter 是开发人员快速处理大数据的最简单方式，而无需在他们的计算机中安装任何东西。数据学校列出了六个提供 Jupyter online 的云平台:

*   [装订器](https://mybinder.org/)
*   [Kaggle 内核](https://www.kaggle.com/)
*   [谷歌联合实验室(Colab)](https://colab.research.google.com/)
*   [微软 Azure 笔记本](https://visualstudio.microsoft.com/vs/features/notebooks-at-microsoft/)
*   [CoCalc](https://cocalc.com/)
*   [数据记录](https://datalore.jetbrains.com/)

kdkin 在他们的[前 5 名免费云笔记本](https://www.kdnuggets.com/2022/04/top-5-free-cloud-notebooks-2022.html)列表中又增加了几款:

*   [深度提示](https://deepnote.com/)
*   [Paperspace 渐变](https://www.paperspace.com/gradient)
*   [亚马逊 SageMaker](https://studiolab.sagemaker.aws/)

这么多选择留给开发者去决定。但是，如果一个人已经是 Gmail 的注册用户，那么谷歌可乐只需点击一下[https://colab.research.google.com/](https://colab.research.google.com/)。