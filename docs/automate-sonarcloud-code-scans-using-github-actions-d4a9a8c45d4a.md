# 使用 GitHub 动作自动扫描 SonarCloud 代码

> 原文：<https://blog.devgenius.io/automate-sonarcloud-code-scans-using-github-actions-d4a9a8c45d4a?source=collection_archive---------2----------------------->

![](img/f42f4daead4db5da453eddfec84b5a71.png)

# 背景

在早先的[帖子](https://www.handsonarchitect.com/2020/08/how-to-improve-code-quality-using.html?WT.mc_id=DOP-MVP-5003170) / [YouTube 视频](https://youtu.be/7UsMweLh-ms?WT.mc_id=DOP-MVP-5003170)中，我已经演示了如何使用 GitHub Super Linter 和 GitHub action 来自动化代码排列过程。在这篇文章中，我们将探索如何使用 GitHub 动作，通过使用 [SonarCloud](https://sonarcloud.io/) 来自动化静态代码分析。

# 声纳云

你可能听说过 SonarQube。它为不同的编程语言提供扫描器。SonarCloud 是一种云服务，它扫描代码库以发现错误、漏洞和代码气味。在撰写本文时，支持 24 种主流编程语言，包括:

*   C#
*   Java 语言(一种计算机语言，尤用于创建网站)
*   计算机编程语言
*   Java Script 语言
*   以打字打的文件
*   去
*   科特林和其他人

SonarCloud 提供了对代码多个维度的详细分析。这些有助于识别开发人员的常见错误，并确保代码的高质量。SonarCloud 还会给出一个指标，说明修复所有报告的问题和消除技术债务需要多少时间。不同的维度是

*   可靠性
*   安全性
*   可维护性
*   新闻报道
*   重复

SonarCloud 也有质量关口和质量概况。在扫描文件时，质量配置文件可用于细化应用于代码的规则。

# 使用 GitHub 动作自动扫描代码

在视频中，我们可以看到如何使用 GitHub Action 来自动执行使用 SonarCloud GitHub Action 的代码扫描。

# 结论

SonarCloud 通过执行静态代码分析，提供了非常好的代码库分析。规则集可以根据语言和组织策略进行定制。GitHub 动作使得自动化工作流程变得非常容易。结合 GitHub action 和 SonarCloud 的强大功能，我们可以以自动化的方式获得关于代码的最新信息。我希望这篇文章对你有用。

直到下一次，带着激情编码，追求卓越

*原载于 2020 年 9 月 7 日 https://www.handsonarchitect.com**[*。*](https://www.handsonarchitect.com/2020/09/automate-sonarcloud-code-scans-using.html)*