# 每个 Java 开发人员都知道的 8 个问题

> 原文：<https://blog.devgenius.io/8-problems-every-java-developer-knows-4f65339e0c00?source=collection_archive---------0----------------------->

## 来自我对 Java 的浅薄经验的问题

![](img/fb6375fb41de83a3c52041b279dbf900.png)

来自 [Pexels](https://www.pexels.com/photo/man-with-hand-on-temple-looking-at-laptop-842554/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的 Andrea Piacquadio 的照片

用 Java 开发带来了自己的问题。问题可能是由于糟糕的工具、缺乏经验或者项目本身。

你每天都要面对它们。无论是破建。或者是安全问题。他们存在。

Java 开发人员面临的最常见的问题是什么？让我们找出答案。也找出解决办法。

# 服务器启动

服务器启动缓慢。即使在本地。

你可以缓解这个问题。使用 [JRebel](https://plugins.jetbrains.com/plugin/4441-jrebel-and-xrebel-for-intellij) 。这将在更改后立即重新加载您的类。

这并没有解决测试的问题。对詹金斯来说，启动服务器需要时间。

你的公关失败有很多原因。不是你的错。你可能会受到很多因素的影响。这会导致发展缓慢。

是的，Spring Boot 更快。这并不意味着你会穿弹簧靴工作。企业系统不会经常改变。您可能正在开发 Spring 框架或 Hybris。

你使用旧的系统。你对此无能为力。如果行得通，一个企业就没那么在乎了。

# 春天安全

注意安全。错误的配置会导致安全漏洞。

我将列出一些您可能面临的安全问题。大部分问题都可以在[这里](https://www.checkmarx.com/supported-coding-languages/java-security-vulnerabilities-and-language-overview/)找到。

*   向客户端公开服务器数据
*   不当授权
*   保留默认设置

尽量减少这些问题。嵌入静态代码分析。例如，您可以集成这个[工具](https://www.checkmarx.com/static-code-analysis-sca/?utm_source=javacodegeeks&utm_medium=sponsorship&utm_campaign=sponsored%20post&utm_content=security%20malpractices)。

为了提高安全性，在代码审查上变得冷酷无情。检查 Spring 安全性更改。你复习彻底。你创建安全的应用程序。

# 测试

![](img/2565f8f32c3fae8e5418381e7d4985c6.png)

[来源](https://undraw.co/search)

你和 J2EE 一起工作吗，雅加达版？你在企业联盟。

企业不让开发者犯错。因此需要大量的测试。我是说很多。

几种环境。很少 QA 团队。和您自己的测试套件。

你将需要大量的 QA 框架。

用 [Selenium](https://www.selenium.dev/) 进行自动化测试。或者你可以使用[猎鹰](https://falconframework.org/)。

**你需要行为测试吗？**你学[黄瓜](https://cucumber.io/)和[小黄瓜](https://cucumber.io/docs/gherkin/)。

这可能是一种负担。你需要开发软件。你也需要测试，无论是之前还是之后。

您可以重用现有的测试。重用别人的测试，检查别人做了什么。你可以学习黄瓜测试，主要是通过重用现有的测试。

对于定制的东西，你可能会面临问题。求学长帮忙。深入文档。

# 黯然失色

![](img/bea7ffddf95d2132ffab6061c446dd0e.png)

[来源](https://undraw.co/search)

不惜一切代价避免日食。很多整合问题。对 Java 开发者的不良支持。

IntelliJ 没有问题，有很大的支持，插件广泛可用。

# 春天

很多新概念要学。没有前端的变化快。还是很多概念。Spring 集成、安全性和 MVC 概念。

即使工作了几年，你还是会跌跌撞撞地陷入未知。

例如看[弹簧集成](https://spring.io/projects/spring-integration#overview)。您可能会被庞大的文档淹没。未知领域确实会出现问题。

# 雄猫

Tomcat 的问题大多围绕配置。您可以学习它，但是很可能您需要大量的配置。

注意了。Tomcat 负责 app 运行。你需要小心。

# Java 进化

难以把握新的变化。你掌握了 Java 8 的特性。新的出现了。现在 Java 16 出现了。更重要的是，功能出现了。

这里看版本[之间的变化](https://www.marcobehler.com/guides/a-guide-to-java-versions-and-features#_java_features_8_14)。这仅供参考，并不是说你追逐新的和闪亮的 Java。坚持基础，逐步升级。

尽管发展很快，Java 8 仍然在发展。我确实从 Java 8 升级到了 Java 11。你确实看到了很多问题。

即使你从 Java 8 升级到 Java 11。你面临着巨大的变化。内部类不再可用。或者库改变包，名字，以及其他类似的问题。

> 较新的 Java 版本现在每 6 个月发布一次。因此，Java 16 计划于 2021 年 3 月发布，Java 17 计划于 2021 年 9 月发布，依此类推。在过去，Java 发布周期要长得多，长达 3-5 年。— [来源](https://www.marcobehler.com/guides/a-guide-to-java-versions-and-features)

![](img/b2f9094c38dadb80ab4f2935c57e2abc.png)

[来源](https://www.marcobehler.com/guides/a-guide-to-java-versions-and-features)

# 业务逻辑

![](img/48c95d6551cc8d791c49451bb055aa86.png)

[来源](https://undraw.co/search)

你面临着大量遗留代码。没有商业分析师几乎无法理解。大块的逻辑，有时没有适当的管理。

大型团队。人们离开了。留下糟糕的代码。甚至是错误的。

有了好的项目管理，这不是问题。可能是坏消息。实施好的错误管理，好的问题跟踪，你会做得很好。

# 结论

现在你知道我们每天面对什么了。

从测试到应用服务器，以及 Java 版本。确实出现了很多问题。

这一想法的关键要点是:

*   了解更多关于春天的信息
*   坚持 Java 基础知识
*   与 Java 保持同步
*   了解更多关于测试和预防常见问题的信息
*   实施良好的项目管理

# *今天就加入灵媒吧！*

***你为什么要*** [***订阅***](https://zivce.medium.com/membership?source=responses-----f49b64432202---------------------respond_sidebar-----------) ***？*** 率先抛弃微服 Chrome 模式。其次，你会接触到很多精彩的故事。你可以从实用程序员书架上读到大约 100 本书。你可以看到来自 Pinterest 团队的障碍、非常有用的提示和很好的建议。你可以阅读关于谷歌云的最新进展。

**这就是你每月**[**【5 美元(两杯咖啡)**](https://zivce.medium.com/membership?source=responses-----f49b64432202---------------------respond_sidebar-----------) **得到的。你可以花 [$5](https://zivce.medium.com/membership?source=responses-----f49b64432202---------------------respond_sidebar-----------) 阅读整个实用程序员库。**

免责声明:$2 出 [$5](https://zivce.medium.com/membership?source=responses-----f49b64432202---------------------respond_sidebar-----------) 直接支持我，为你送上精彩话题。