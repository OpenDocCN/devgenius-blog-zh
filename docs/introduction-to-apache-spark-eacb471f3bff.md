# Apache Spark 简介

> 原文：<https://blog.devgenius.io/introduction-to-apache-spark-eacb471f3bff?source=collection_archive---------12----------------------->

![](img/5d0bbe49b3697d63d47ec142b6c08d3b.png)

画

A pache Spark 是一个开源数据处理引擎，使用简单的**编程结构，跨各种计算机集群**实时存储和处理数据**。**

# **关键火花特征:**

*   *快:*处理——Spark 处理数据的速度比 Hadoop 快 100 倍。
*   *内存计算:*数据存储在 RAM 中
*   *灵活:*支持多种语言(Java、Scala、Python、R)
*   *容错:*处理工作节点故障，确保数据丢失等于 0

# Apache Spark 的组件

## 火花核心

Spark 核心是大规模并行和分布式数据处理的基础引擎

它负责:

*   内存管理
*   故障恢复
*   调度、分发和监控作业
*   与存储系统交互

**火花本身没有自带存储！**

当我们谈论 Spark Core 时，我们必须提到什么是 ***RDDs*** 。

RDDs(弹性分布式数据集)是可以并行操作的容错元素集合。

Spark 核心嵌入了很多**rdd。**

我们可以对**rdd:**执行两种动作

## **转换:**

它总是创造新的 RDD。

这些类型的动作被认为是转换

*   加入
*   地图
*   过滤器

## 行动:

不创建新的 RDD，而是在现有的基础上执行操作。

动作示例:

*   数数
*   减少
*   第一

这是 Apache Spark 的第一个也是最基本的组件——Spark Core。

其他组件包括:

## Spark SQL

用于结构化和半结构化数据处理的框架组件。

## 火花流

快速可靠地处理数据流。

## 火花 MLlib

简化机器学习算法的部署和部署。

## GraphX

是图形计算引擎和数据存储。

# 火花建筑

Spark 采用了名为[主从](https://en.wikipedia.org/wiki/Master/slave_(technology))的架构。

它由一个在主节点上运行的驱动程序和多个在集群中的工作节点上运行的执行器组成。

# Pyspark 演示

Pyspark 是 Python 中 Apache Spark 的接口。

以下是展示如何使用 pyspark 的笔记本示例:

示例笔记本

# 摘要

Spark 在数据世界中仍然是相关的。任何它将是。至少学习它的基础知识是完全值得的。

进一步阅读:

[Spark:权威指南:大数据处理变得简单](https://www.amazon.pl/Spark-Definitive-Guide-processing-simple/dp/1491912219/ref=asc_df_1491912219/?tag=plshogostdde-21&linkCode=df0&hvadid=504486200565&hvpos=&hvnetw=g&hvrand=13856618504355694651&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=1011615&hvtargid=pla-318947193480&psc=1)由[比尔·钱伯斯](https://www.amazon.pl/s/ref=dp_byline_sr_book_1?ie=UTF8&field-author=Bill+Chambers&search-alias=books)和[马泰·扎哈鲁](https://www.amazon.pl/s/ref=dp_byline_sr_book_2?ie=UTF8&field-author=Matei+Zaharu&search-alias=books)完成

spark 文档是寻找许多问题答案的好地方