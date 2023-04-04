# 低代码开发会导致安全问题和数据泄露吗？

> 原文：<https://blog.devgenius.io/will-low-code-development-lead-to-security-problems-and-data-breaches-83a9375d4087?source=collection_archive---------3----------------------->

## 低代码开发的弱点是缺乏经验的开发人员不了解安全性

![](img/4dd5b4ffdd66c34af981f7fc4b514a03.png)

照片由 [PhotoMIX 公司](https://www.pexels.com/@wdnet?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从 [Pexels](https://www.pexels.com/photo/man-wearing-gray-and-red-armour-standing-on-the-streets-226746/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 拍摄

> 在软件安全成为紧急情况之前，认识到它是一个问题

低代码开发有可能比传统开发更快、更便宜地创建软件。没有被谈论的是，这种软件将会带来与传统的基于代码的软件相同的安全风险。

安全性内置于低代码开发工具中，但它必须被理解和配置。公民开发者可能没有意识到安全问题，或者没有配置安全的经验。

你看不到的一击将你击倒，公民开发者将专注于创建软件，而不是应用安全最佳实践。

如果低代码开发没有与 IT 部门和最佳实践协作完成，那么有经验的开发人员就不会定义指导方针。这将产生潜在的安全漏洞和数据泄露。

这不是一个公民开发者的问题，这是一个普通的开发问题，不同的是有经验的开发者(不总是)检查安全性是否到位。没有经验的初级/公民开发者甚至不会意识到安全是他们的责任和应该做什么。

# **微软 Power App 门户曝光 3800 万条记录**

微软的低代码开发工具 Microsoft Power Apps 成为头条新闻，因为一个错误配置的 Power 应用程序将 3800 万条记录暴露在互联网上。

[错误配置的 Microsoft Power 应用程序暴露了 3800 万条记录。雷德蒙的建议？RTFM](/38 million records exposed by misconfigured Microsoft Power Apps. Redmond’s advice? RTFM)

RTFM 代表阅读翻页手册:-)

关键报价

> “包括微软在内的 47 家政府实体和隐私公司因错误配置 Windows 巨头的 Power 应用程序而在网上暴露了 3800 万条敏感数据记录，这是一种低代码服务，有望提供一种构建专业应用程序的简单方法。”

# **是什么导致了这个问题？**

UpGuard 公司的一名分析师发现，OData API for Power Apps 门户网站可以允许匿名访问数据库记录。

Power Apps 门户是一个面向互联网的门户。该门户由微软托管，并与微软 Dataverse 集成。

简而言之，如果以某种方式配置，面向互联网的 Power Apps 门户可以允许通过匿名(例如，非用户)OData 查询来访问存储在多个数据源(SharePoint、Microsoft 365、Dynamics 365、SQL Server 等)中的数据。

如果不希望数据可见，则需要选中布尔型字段-将表权限布尔型启用为真。

这是大多数人采用默认值的那些讨厌的配置值之一，即与世界共享数据。

这位分析师看到许多公司使用 Power Apps 门户网站，而许多公司没有设置这一领域。结果是，他们可能会提供大量个人和其他数据，以便在面向公众/互联网的门户网站上进行查询。

面向对象的程序设计系统(Object-Oriented Programming System)

# 通过设计

这不是一个错误或安全漏洞，这是一个按照设计工作的配置。

UpGuard 的报告清楚地表明了这一点— [设计:微软 Power 应用程序的默认权限如何暴露了数百万人](https://www.upguard.com/breaches/power-apps)

如果你想知道如何解决这个问题(我敢肯定有很多开发者和公民开发者有点紧张)，那么读一下这个——[提示#1407:如何保护 Power Apps 门户网站不成为新闻](/Tip #1407:%20How%20to%20secure%20Power%20Apps%20portal%20from%20making%20the%20news)

# **低码警告**

低代码允许公民开发者或任何开发者使用组件来创建软件，并用更容易理解的查询来配置它们。

像微软的 Power Platform 这样的低代码开发工具的重点都在应用程序的创建上，但是开发人员做的不仅仅是创建软件。

低代码开发人员(公民开发人员)仍然是开发人员，需要掌握开发中的其他任务。

*   收集需求(提炼、理解)
*   维护软件
*   记录软件
*   部署
*   安全性(数据安全性、个人数据、安全角色等)
*   数据源(数据库、文件、保留、备份等)
*   与其他系统集成
*   更广泛的环境和其他软件

营销不仅仅是发送电子邮件，开发也不仅仅是编写代码或构建应用程序。

公民开发人员和开发人员不是安全方面的专家，这就是为什么 IT 部门会参与进来，渗透测试会在软件上运行。确保最佳实践得以实施，并且软件不会因为设计缺陷或错误而变得安全。

公民开发人员将会有知识上的差距，这就是为什么他们不应该在没有指导的情况下创建应用程序并将其部署到生产环境中。

如果公司创建自己的低代码开发团队，而没有让有经验的开发人员或 IT 部门参与创建标准和最佳实践，就会出现问题。

安全缺陷对于传统开发来说是一样的，但是它强调了一个公民开发人员需要了解的领域。低代码开发工具的复杂性，人们将需要专攻个别的低代码工具。

公司将需要单独的团队来监督和维护低代码工具，因为没有人能够捡起它们并理解细节。

这不是一个低代码问题，而是一个开发问题。低代码开发工具不会取代开发人员，它们会创造一个不同的开发人员群体。创建和维护生产应用程序的一组缺乏经验的开发人员。

# **软件是软件是软件**

从没有经验的开发人员那里释放大量的应用程序有潜在的安全漏洞，这就是为什么不可避免的低代码革命必须由有经验的开发人员和 IT 部门领导。

[低代码开发会扰乱软件开发，但这需要软件开发人员去做](/Low-Code Development Will Disrupt Software Development but It Will Need Software Developers to Do It)

低代码开发是一个强大的工具。像所有的工具一样，它们需要作为长期战略的一部分来使用，并为软件创建后发生的事情制定计划。

低代码开发不会取代开发人员，它会创造更多没有经验的开发人员。公民开发者需要资深开发者的指导和领导，因为像所有初级开发者一样，他们也会犯错误。

软件是如何创造的并不重要；它需要安全、维护和支持。低代码软件需要经历与传统软件相同的清单、团队和流程。

# 进一步阅读

*   [开发者必须考虑低代码应用的安全性](https://searchsoftwarequality.techtarget.com/news/252488389/Developers-must-consider-low-code-app-security)
*   [4 低代码和无代码开发的安全问题](https://www.csoonline.com/article/3404216/4-security-concerns-for-low-code-and-no-code-development.html)
*   [为什么到 2024 年，低代码开发工具不会导致 80%的软件由公民开发者创建](/why-low-code-development-tools-will-not-result-in-80-of-software-being-created-by-citizen-ad6143a60e48)