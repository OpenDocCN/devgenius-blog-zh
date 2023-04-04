# 不同 S3 桶存储类别的使用

> 原文：<https://blog.devgenius.io/usage-of-different-s3-bucket-storage-classes-e505d649c496?source=collection_archive---------11----------------------->

![](img/db26479a30bf763d1bf899315f6456ce.png)

封面图像

# S3 服务的特点

S3 是 AWS 上最受欢迎的**存储**服务。**存储在这些存储桶上的数据的可用性和持久性**非常出色，大多数情况下具有 99.999999999%的持久性和 99.99%的可用性。

**持久性**意味着数据永远不会丢失或受损。

可用性意味着 thad 数据可以被快速访问。

可以通过以下方式将对象上传到 S3 存储桶:

*   管理控制台(网络浏览器)
*   CLI
*   编程方式(使用编程语言)

# S3 存储类

## 标准

特点:

*   存储在这些存储桶类中的数据跨多个[az—可用性区域](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html)存储
*   对影响整个可用性区域的事件具有弹性
*   低延迟和高吞吐量

耐用性:99.999999999%

可用性:99.99%

使用建议:经常访问的数据

## S3 智能分层

特点:

*   存储在这些存储桶类中的数据跨多个[az—可用性区域](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html)存储
*   自动将您的数据移动到最经济高效的存储类别
*   成本节约是自动化的
*   没有运营开销，没有生命周期费用，没有检索费用，也没有最低存储期限

耐用性:99.999999999%

可用性:99.9%

建议使用:访问模式未知的数据

## S3 标准-IA(不经常访问)

特点:

*   存储在这些存储桶类中的数据跨多个[az—可用性区域](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html)存储
*   与 S3 标准相同的低延迟和高吞吐量性能
*   比 S3 标准便宜

耐用性:99.999999999%

可用性:99.9%

推荐使用:数据访问不频繁，但需要时可以快速访问

## S3 一区-IA(不经常访问)

特点:

*   类似于 S3 标准 IA，但数据仅存储在一个 [AZ —可用性区域](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html)
*   成本比标准 IA 低 20%
*   存储在此存储类中的数据可能会丢失

耐用性:99.999999999%

可用性:99.5%

使用建议:数据访问不频繁，但需要时可以快速访问。存储在该类中的数据应该是可重新创建的，因为它可能会丢失。

## 亚马逊 S3 冰川即时检索

特点:

*   存储在这些存储桶类中的数据跨多个[az—可用性区域](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html)存储
*   长期数据存储和存档，降低成本
*   与 S3 标准具有相同性能的毫秒级数据检索

耐用性:99.999999999%

可用性:99.9%

使用建议:归档需要立即访问的数据，如医学图像、新闻媒体资产或用户生成的内容归档。

## 亚马逊 S3 冰川灵活检索

特点:

*   存储在这些存储桶类中的数据跨多个[az—可用性区域](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html)存储
*   成本比 **S3 冰川即时检索**低 10%
*   数据不是即时访问的，而是在几分钟到几小时的时间范围内免费批量检索

耐用性:99.999999999%

可用性:99.99%

建议使用:每年访问 1-2 次并异步检索的数据

## 亚马逊 S3 冰川深层档案

特点:

*   存储在这些存储桶类中的数据跨多个[az—可用性区域](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html)存储
*   成本最低的存储类别，旨在长期保留数据
*   检索时间在 12 小时以内

耐用性:99.999999999%

可用性:99.99%

使用建议:长期数据存档每年访问 1-2 次。为符合法规要求而检索的数据

## S3 前哨

*   在内部提供对象存储
*   单一存储类别
*   使用 SSE-S3 和 SSE-C 加密
*   跨多个设备和服务器存储数据

使用建议:需要本地存储的数据。

# 总结

AWS 为您的数据存储提供了广泛的解决方案。您应该仔细考虑哪种 S3 存储产品最适合您的业务需求。

这里是 AWS 文档的[链接](https://aws.amazon.com/s3/storage-classes/)，在这里你可以找到更多关于 S3 存储类的信息和性能图表。