# Java 变体之间的困惑？这里有一本入门！

> 原文：<https://blog.devgenius.io/confused-between-java-variants-heres-a-primer-a66250f8a3e8?source=collection_archive---------5----------------------->

## Java 基础

## Java SE，Java EE，Jakarta EE，J2EE，J2SE，Java 平台！！！！

![](img/a47253e27ffcaa5ee67af157ba3f21f5.png)

马克西米利安·魏斯贝克尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

无论您是初学者还是有经验的 Java 程序员，在某些时候您可能会对 Java 术语感到困惑。这种混乱的原因之一，是因为 Java 有如此多的变体。Java 如此受欢迎的原因之一，也是因为它有如此多的变种。在本文中，我将试着澄清一些困惑，并提供 Java 平台的当前状态。

# 这一切是如何开始的— JDK1.0

1990 年，Sun Microsystems 在很少工程师的情况下启动了这个项目，目的是开发新技术，为 Sun 正在开发的下一代智能设备编程。他们想要一个可以轻松移植到所有类型设备的平台。在放弃了几个选项之后，比如扩展 C++和将 C 与其他语言相结合，这个小组决定开发一种新的编程语言。Java 已经开始作为 C 和 C++编程语言的替代品。他们最初将它命名为 Oak，但由于商标问题，不得不将其改为 Java。Java 于 1995 年在 SunWorld 大会上首次发布，JDK (Java 开发工具包)1.0 于 1996 年发布。在这一点上，JDK 是一个简单的软件开发工具包(SDK)，允许开发者开发，调试和监控 Java 程序。

# Java 平台——J2SE、J2EE 和 J2ME(1998-2004 年)

1998 年，在 JDK 1.1 之后，Java 被重新命名为 Java 2，因为它是平台中的一个大变化。命名为 Java 2，全称 Java 2 软件开发工具包，缩写为 Java 2 SDK 或 J2SDK。它创造了 3 种不同的 Java 变体，以支持不同的应用。

*   J2SE : Java 2 平台，标准版— *面向桌面和服务器环境。*
*   J2EE : Java 2 平台，企业版— *面向企业应用，具有支持分布式计算和 web 服务的特性。*
*   J2ME : Java 2 平台，微型版— *面向移动和嵌入式设备。*

通过创建这些变体，Sun 能够单独开发每个应用程序，能够服务于从嵌入式设备到分布式系统的各种平台。

版本是 J2SE 1.2，J2EE 1.2 和 J2ME 1.2。类似的惯例一直延续到 2004 年。

# 重命名—更干净的名称？(2004–2010)

在 2004 年，他们删除了名字中的 1，而不是 J2SE 1.5，新名字是 J2SE 5.0。一年后，它从名字中去掉了 2，并明确表示为 Java。所有版本的名字都相应的改成了 Java SE，Java EE，Java ME。Sun 简化了平台名称，以更好地反映 Java 平台内置的成熟度、稳定性、可伸缩性和安全性。开发工具包也从“Java 2 SDK”恢复为“JDK”这个名字。运行时环境已经从“J2RE”回复到“JRE”

这些名字是-

*   Java SE : Java 平台，标准版
*   Java EE : Java 平台，企业版
*   Java ME : Java 平台，微型版

并且版本有 Java SE 6，Java EE 5。

# 进入甲骨文(2010–2018)

2010 年，甲骨文收购了太阳微系统公司，使太阳成为甲骨文的全资子公司。但是，关于命名，没有什么真正改变。它继续使用类似的命名惯例。

# Java SE

一般来说，Java SE 更新通常每 3-4 年进行一次，有时稍早，有时稍长(Java SE 6 和 7 之间有 5 年的间隔)。从 Java SE 9 开始，Oracle 改变了发布周期。它引入了长期支持(LTS)版本，每 3 年发布一次，以及更频繁的非 LTS 版本，每 6 个月发布一次。这似乎是一个更好的方法，可以更快地移动并增加更频繁的更新。一旦新功能发布，任何以前的非 LTS 版本都将被视为被取代。例如，Java SE 9 是非 LTS 发行版，并立即被 Java SE 10(也是非 LTS 发行版)取代，而 Java SE 10 又立即被 Java SE 11 取代。然而，Java SE 11 是 LTS 发行版，因此它受到支持，即使 Java SE 12(非 LTS)已经发行。

# Java EE 到 Jakarta EE : Oracle 到 Eclipse

2017 年，甲骨文将 Java EE 赠送给 Eclipse foundation，试图使其完全开源。但是，由于 Java 这个名字被授权给 Oracle，很遗憾，Eclipse 不得不寻找一个非常流行的 Java 名字的替代物。Eclipse 考虑了许多备选方案，但最终选择了“Jakarta”和“Enterprise profile”。如果你还记得 Jakarta，它是 Apache 基金会的一个开源 Java 项目，开始于 1999 年，结束于 2011 年。这与 Jakarta EE 无关，但是由于这个名称在 Java 社区中使用得很好，所以对它有了更多的喜爱，这导致了这个名称在 Java EE 中的重用。2019 年，Jakarta EE 8 发布，这是 Eclipse 下的第一个版本，采用了新的命名约定。

# OpenJDK

如果你是用 Java 编程的，那么很有可能你一定遇到过甚至使用过 OpenJDK。OpenJDK(顾名思义)是 Java SE 开源实现，最初发布于 2007 年，当时 Sun Microsystems 还拥有 Java。如果没有商业许可，2019 年 1 月之后发布的 Oracle Java SE 8 不可用于商业、商业或生产用途。然而，OpenJDK 是完全开源的，可以免费使用。这两者之间没有真正的技术差异，因为 Oracle JDK 的构建过程是基于 OpenJDK 的。说到性能，Oracle 在响应能力和 JVM 性能方面要好得多。由于稳定性对企业客户的重要性，它更加关注稳定性。相比之下，OpenJDK 将更频繁地发布版本。因此，我们可能会遇到不稳定的问题。根据社区反馈，我们知道一些 OpenJDK 用户遇到了性能问题。Oracle JDK 完全由 Oracle Corporation 开发，而 OpenJDK 由 Oracle、OpenJDK 和 Java 社区开发。

# 摘要

有了这些，我希望你对这些名字有一个清晰的概念，它是如何随着时间的推移而改变的，以及事情的现状。随着 Java 的广泛使用，在许多平台上，你会遇到许多其他的东西，但是永远记住基础知识。