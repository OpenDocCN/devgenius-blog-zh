# 关于向客户提供代码的非技术性指南

> 原文：<https://blog.devgenius.io/a-non-technical-guide-about-bringing-code-to-your-customer-a51696b4d1f7?source=collection_archive---------8----------------------->

部署管道听起来非常技术性和复杂。但是，你可以理解 5 分钟引导一个产品走向生产的旅程。

![](img/a470c9823d3039e10914e92949f2210e.png)

> 开发和运营应该齐头并进。

开发和运营应该齐头并进。

将代码部署到产品中，这实际上意味着什么？部署描述了将代码转移到在服务器上运行的环境中(而不是在本地计算机上)。这可以是[生产环境](https://dev.to/flippedcoding/difference-between-development-stage-and-production-d0p)或[登台环境](https://dev.to/flippedcoding/difference-between-development-stage-and-production-d0p)，其目的是在用户与之交互之前尽可能精确地复制生产环境以测试您的特性。把它想象成你的游乐场，在向用户公开软件之前，你可以在那里安全地运行软件。

# 运输经常定期增加

您只想部署高质量的代码，避免缺陷和故障。为了总是给你的用户提供最好的体验，你应该采取简短的反馈循环，并不断调整你的产品。因此，你需要能够改变运行系统。

软件开发和运营，运行软件的维护，不应该相互分离。否则你如何从用户反馈中学习并调整你的产品？这是 [DevOps](https://searchitoperations.techtarget.com/definition/DevOps) 方法的基础，该方法将开发和运营结合起来，并为整个产品生命周期带来一个整体视角。

为了确保良好的代码质量，测试至关重要。因此，让我们讨论一下部署管道，以理解代码进入生产的不同可能步骤。

## 部署管道

一个[部署管道](https://www.bmc.com/blogs/deployment-pipeline/)由有序的步骤(或阶段)组成，以尽可能高效和安全地将代码投入生产，并测试代码质量。部署代码变更意味着在生产环境中替换整个或部分代码库。在早期，这是一个耗时的手动过程。

> 更广泛地说，部署管道的工作是检测任何会导致生产中出现问题的变化。— [马丁·福勒](https://martinfowler.com/bliki/DeploymentPipeline.html)

因此，为了实现安全快速的代码部署，有必要[尽可能自动化这些步骤(一些步骤/阶段不需要任何人工交互)并将它们并行化](https://martinfowler.com/bliki/DeploymentPipeline.html)。就像手动测试或复杂测试一样，例如，通过整个工作流程的测试，不会不必要地延长过程。

那么，部署管道通常包含什么呢？

1.  发生变更时触发构建服务器的代码存储库
2.  构建将代码转换成机器代码的服务器
3.  x 运行测试的测试和试运行环境的数量
4.  代码被部署并最终向公众发布的生产环境

这听起来很专业，但是让我们把这些术语说得更清楚些。

**代码存储库**
把代码存储库想象成一个共享的篮子，开发者可以把他们的新代码放在里面，从而让从事同一产品的其他开发者也可以访问。这个“代码[提交](https://en.wikipedia.org/wiki/Commit_%28version_control%29#:~:text=In%20version%20control%20systems%2C%20a,kept%20in%20the%20repository%20indefinitely.)”在存储库中创建了一个新的代码版本，并被归档在那里(可以把它想象成一个带有版本号的文件夹)。像这样，万一新版本有缺陷之类的，总能找到以前的版本。

**构建服务器**
存储库中的新版本通常会触发[构建服务器来“编译”](https://www.techopedia.com/definition/3759/build)代码。这意味着代码正在被转换成机器代码(带有 1 和 0 的代码)。现在它不再仅仅是一堆线，而是可以被计算机阅读和使用。

在这个服务器上，你也可以运行一些第一测试，比如[单元测试](https://medium.com/python-in-plain-english/everything-you-need-to-know-about-unit-and-integration-tests-99302d487510)，确保单独代码段的纯粹功能性被给出，代码本身没有缺陷。如果代码通过了测试，服务器会给出一个“绿色”的反馈，或者拒绝构建并发送一个构建错误。在此之前，开发人员还可以在自己的机器上进行手动静态测试，只需使用代码，不需要服务器环境。

**中转环境**
可以是一个或几个测试目的不同的环境。目标应该是拥有尽可能少的环境，因为它们需要时间来设置和维护。另一方面，并行运行不同类型的测试(在不同的环境下)可以节省时间，比如[负载测试](https://www.guru99.com/load-testing-tutorial.html)(测试大量用户同时交互时系统的反应)和[集成测试](https://medium.com/python-in-plain-english/everything-you-need-to-know-about-unit-and-integration-tests-99302d487510)。

**生产环境** 如前所述，在生产环境中，最终用户可以与应用程序进行交互。[这并不一定意味着每个已经部署到生产中的代码对最终用户都是可用的](https://www.youtube.com/watch?v=uS1zn4Uluds)。

有了所谓的[功能切换](https://martinfowler.com/articles/feature-toggles.html)，您可以将代码部署到生产环境中，并对用户“隐藏”它。这样，您可以通过运行较慢的测试来节省时间，因为您可以部署较小的代码片段，这降低了复杂性。

这样，在没有向最终用户提供缺陷软件的风险的情况下，更容易定期部署到生产中。只有当所有测试都通过时(绿色)，该功能才会被[发布](https://www.youtube.com/watch?v=uS1zn4Uluds)并对最终用户可见。

部署管道可能是什么样子:

![](img/710491885570c55ff7c9f2a478e45ce6.png)

部署管道草案——由 Alexa Lautenbacher 设计

现在让我们仔细看看部署管道的自动化方面。为了使软件部署顺利而容易地进行，您希望代码尽可能地在管道中自主运行。这有助于您经常部署，并且无所畏惧。三个主要习惯描述了上述步骤之间的自动化:连续集成、连续交付和连续部署。让我向你解释一下它们的含义。

## 持续集成(CI)

CI 描述了将代码定期推入共享代码库中。这使得团队中其他开发人员所做的变更变得透明。因此，每当开发人员创建一个工作件时，将他们的代码变更与共享存储库(也称为主分支)中的代码合并是他们的习惯。(提供与主分支合并的新代码被称为“拉请求”当另一个开发人员已经检查并接受它时，合并就完成了，一个新的版本出现在主分支中。).

系统会在接受拉取请求之前自动通知相关开发人员，以防止合并冲突。这样，矛盾的代码或缺陷可以在早期被识别，并且各自的开发人员可以立即坐在一起解决问题。

此外，每次代码提交到主分支时，都会自动构建代码(从构建服务器)。由于这总是触发测试(比如单元测试)，CI 也简化了测试。

*所以，竞争情报发生在我们的管道之初。*

## **连续交货(CD)**

那么，CI 和 CD 的区别是什么？CD 描述了沿着部署管道的自动化，直到代码准备好被部署到生产环境。这意味着，每当您将代码提交到存储库中，并且代码通过了管道中的质量测试，您就获得了一个可部署的发布候选。如果代码通过了测试，它将被自动部署到下一个环境中，并且在最终部署到生产环境之前只需要人工批准。

*CD 发生在除生产部署之外的整个部署过程中。*

## 持续部署

连续部署(实际上也是 CD)向自动化更进了一步。除了自动代码构建和从构建服务器到登台环境的部署之外，如果所有测试都顺利通过，它还会自动将代码部署到生产环境中。

*持续部署贯穿整个部署流程，包括部署到生产。*

现在，您已经了解了如何通过部署管道定期交付软件增量的基础知识。定期和经常部署，每天几次，需要良好的设置:

*   一个健壮的基础设施，具有能够处理大量代码的快速管道，
*   熟练的团队成员分享他们的 t 形知识，以改善他们的交付
*   最重要的是，利益相关者和团队成员之间对特性有一个良好的共同理解。

为了进一步完善您对产品生命周期中测试活动的理解，我们现在已经过渡到敏捷测试象限的右侧(参见本系列的第二篇文章)。因此，下一篇文章将向您介绍如何在生产中进行测试，以确定产品是否真的做了它应该做的事情，以及它是否真的为您的客户创造了价值(Q3)。什么是探索性测试，它是如何工作的？您如何监控您的代码以尽早发现问题？

# 本系列的其他文章

![](img/b5f834cb3fc55d05f964a0f3ab3cb47d.png)

敏捷测试策略——图片作者[亚历山德拉·奥利维拉](https://www.linkedin.com/in/aoliveira11/)

*   [文章 1:每个人都能理解的敏捷软件测试要点指南](https://medium.com/@lautenbacheralexa/a-guide-through-the-essentials-of-agile-software-testing-that-everyone-can-understand-789999c89585)
*   [文章 2:如何用一个简单的框架找到正确的测试策略](https://medium.com/swlh/how-to-find-the-right-testing-strategy-with-an-easy-framework-9e496a8b7768)
*   [第 3 篇:你成功测试的超能力:创造共享的理解](https://medium.com/swlh/your-superpower-for-successful-testing-creating-a-shared-understanding-61d6d95c7570)
*   [文章 4:关于单元和集成测试你需要知道的一切](https://medium.com/python-in-plain-english/everything-you-need-to-know-about-unit-and-integration-tests-99302d487510)
*   [第 5 篇:](https://medium.com/dev-genius/a-non-technical-guide-about-bringing-code-to-your-customer-a51696b4d1f7) [关于将代码带给客户的非技术性指南](https://medium.com/dev-genius/a-non-technical-guide-about-bringing-code-to-your-customer-a51696b4d1f7?source=your_stories_page-------------------------------------)
*   [第六条:如何发现自己的产品是否真的创造了价值](https://medium.com/dev-genius/how-to-find-out-if-your-product-really-creates-value-829337de593f?source=your_stories_page-------------------------------------)

# 参考

*   [构建](https://www.techopedia.com/definition/3759/build)
*   提交
*   [马丁·福勒的部署管道](https://martinfowler.com/bliki/DeploymentPipeline.html)
*   [Dan Merron 著《软件工程中的部署管道(CI/CD)](https://www.bmc.com/blogs/deployment-pipeline/)
*   [玛格丽特·劳斯的 devo PS](https://searchitoperations.techtarget.com/definition/DevOps)
*   [mile CIA McG 的开发、阶段和生产之间的差异](https://dev.to/flippedcoding/difference-between-development-stage-and-production-d0p)
*   [皮特·霍奇森的专题切换](https://martinfowler.com/articles/feature-toggles.html)
*   [系列文章的主要来源:敏捷测试概要:Janet Gregory 和 Lisa Crispin 的简介](https://www.goodreads.com/book/show/48516589-agile-testing-condensed)
*   [负载测试](https://www.guru99.com/load-testing-tutorial.html)
*   [Lisa Crispin 的敏捷测试象限](https://lisacrispin.com/2011/11/08/using-the-agile-testing-quadrants/)
*   [Joshua Kerievsky 的发布与部署](https://www.youtube.com/watch?v=uS1zn4Uluds)
*   [什么是持续集成？由埃里克 Minick](https://www.youtube.com/watch?v=1er2cjUq1UI)

# 关于“敏捷软件测试——像我这样的非技术人员的见解”系列文章

在这些孤立的时候，我抓住机会学习更多关于软件测试的知识。如果没有技术背景，这可能很有挑战性。因此，我读了 Janet Gregory 和 Lisa Crispin 写的《T21》[*【敏捷测试概要:简介】*](https://www.goodreads.com/book/show/48516589-agile-testing-condensed) *这本书，并仔细阅读了书中描述的内容。为了让你更容易快速获得最重要的见解，我总结了我从这本书和一些其他来源学到的东西，并希望在一系列短文中与你分享。它们是按照产品开发阶段组织的，你可以一篇一篇地阅读以获得一个整体的概述，也可以选择你最感兴趣的主题。我相信这将对你有所帮助，因为它帮助我在没有技术背景的情况下对测试世界有了更多的了解。*