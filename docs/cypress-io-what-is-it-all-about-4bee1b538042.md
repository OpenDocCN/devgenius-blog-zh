# 赛普拉斯:这是怎么回事？

> 原文：<https://blog.devgenius.io/cypress-io-what-is-it-all-about-4bee1b538042?source=collection_archive---------4----------------------->

![](img/03cc56a4370b69942462e96af61c7dc0.png)

Cypress.io 是一个用户友好的测试自动化工具，用于端到端测试、UI 测试、回归套件、集成和单元测试。它安装简单，不需要对您的代码做任何更改。Cypress.io 以在构建 web 应用程序时快速实时地编写测试而自豪。

Cypress.io 于 2015 年 12 月由一个叫做 [drewlanham](https://medium.com/u/a6131e55d62c?source=post_page-----4bee1b538042--------------------------------) 的人首次推出。

我知道这个神奇的自动化工具，因为我采访了很多公司，问我是否有使用这个工具的经验，我从来没有听说过它，因为我工作过的所有公司和我做过的自由职业者都要求 selenium framework。所以我就想，为什么不学点新东西，和其他人分享知识呢。

所以我要做的是每当我了解到一些关于它的新东西时，就写一系列关于它的故事或博客。但是现在，让我告诉你一些有趣的细节。

好吧！第一件有趣的事情是将它与最常用的自动化框架 Selenium 进行比较。

那么柏树的利弊是什么呢

## 赞成。

*   **完整的框架**(当你在你的系统中安装 cypress 时，它是一个完整的框架，但是对于 selenium，当你安装 selenium 时，你仍然需要安装其他工具来创建你的框架
*   **非常快**(比 Selenium 快，因为 Cypress 用的是 Javascript，JS 是浏览器唯一的母语)
*   更稳定(因为您不需要安装 web 驱动程序或依赖项，并且因为使用 cypress，您可以在浏览器的同一个运行循环中执行测试)
*   不需要扎实的编程技能(因为随着对 cypress 命令的理解以及在哪里放置它们来执行平滑的测试，这对任何人来说都是容易的)
*   测试和模拟 API(Selenium 中没有提供这个特性)
*   不需要测试环境(因为当你测试的时候你不会打断任何人，所以你不需要 SIT，赛普拉斯的 UAT 环境)

## 缺点。

*   没有 IE 和 Safari 支持(它目前正在进行跨浏览器测试，这意味着在未来的更新中将支持这一点)
*   异步代码(对于那些来自面向对象语言如 Java 的人来说会很难，但是你会学到一种用 Cypress 测试的新方法)
*   没有移动自动化(但是我们可以测试移动 web 浏览器的一些功能，测试在浏览器中开发的移动应用程序，比如使用 Ionic framework)
*   单个域或单个选项卡(这意味着您将对每个选项卡执行一个测试周期，这已经是 Selenium 中的测试人员自己完成的过程，以使测试报告更加容易和清晰)

如果你想知道如何安装一个新的框架和工具并开始你的测试，那么下面是方法

1.  前往[https://www.cypress.io/](https://www.cypress.io/)
2.  点击“立即下载”链接下方的“开始使用”链接，将导航至[https://docs . cypress . io/guides/getting-started/installing-cypress](https://docs.cypress.io/guides/getting-started/installing-cypress)
3.  从那里，您可以看到安装 cypress 的步骤，从那里您将遵循您的系统的步骤

如果你是那些喜欢在使用新工具之前学习课程的人，那么你会很高兴有一门课程是用 Cypress tool 完成的，这是在 Udemy 中找到的，由 Artem Bondar 在下面的链接中演示

[https://www . udemy . com/course/cypress-we B- automation-testing-from-zero-to-hero/](https://www.udemy.com/course/cypress-web-automation-testing-from-zero-to-hero/)

现在，我将把这些信息留给你，这里是一个附加的

[](https://docs.cypress.io/guides/references/configuration) [## 配置| Cypress 文档

### 首次打开 Cypress Test Runner 时，它会创建 cypress.json 配置文件。这个 JSON 文件用于…

docs.cypress.io](https://docs.cypress.io/guides/references/configuration) 

所以在上面的链接中，你将会了解这个工具是如何工作的，配置，以及 Cypress 中的所有东西，让你从端到端的自动化测试中获得乐趣。

> 我们不断前进，打开新的大门，做新的事情，因为我们好奇…而好奇心不断引领我们走向新的道路。华特·迪士尼说。

本系列的下一个故事将是我在 Cypress 实际进行自动化测试时学到的东西，也是如何安装和创建我们的第一个测试。

[](https://husseinbaashen.medium.com/subscribe) [## 每当侯赛因·巴申发表文章时，就收到一封电子邮件。

### 每当侯赛因·巴申发表文章时，就收到一封电子邮件。通过注册，您将创建一个中型帐户，如果您还没有…

husseinbaashen.medium.com](https://husseinbaashen.medium.com/subscribe) 

*更多内容尽在*[*blog . devgenius . io*](http://blog.devgenius.io)*。*