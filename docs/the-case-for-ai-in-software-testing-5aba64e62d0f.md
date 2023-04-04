# 人工智能在软件测试中的应用

> 原文：<https://blog.devgenius.io/the-case-for-ai-in-software-testing-5aba64e62d0f?source=collection_archive---------4----------------------->

好得难以置信？

![](img/fbd27e49375c7878ccf21f65d70a0b05.png)

布雷特·乔丹在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 背景

2017 年的某个时候，我开始做一项关于人工智能在测试和开发中的应用的调查。这引发了我关于这个主题的一系列演讲(见[幻灯片](https://www.slideshare.net/HeemengFoo/an-introduction-to-ai-in-test-engineering))。这使我坚信，尽管这种技术的采用在当时仍处于萌芽状态，但它有望成为帮助我们解决日益复杂的软件系统测试问题的关键。这导致了在湾区成立了一个名为“测试和开发中的人工智能”的 Meetup 小组(我们后来将其重命名为测试中的人工智能和测试人工智能)。

# 有什么问题？

下面是一个典型的敏捷/精益开发过程:

1.  产品团队梦想着构成产品的特性
2.  产品团队定义 MVP
3.  开发团队构建 MVP
4.  该产品是狗喂养的内部
5.  产品被发送给 beta 测试人员进行反馈
6.  产品被运送到生产现场(伴随着崩溃管理、遥测等)。)
7.  产品团队收集指标和真实的用户反馈，以完善产品功能
8.  产品团队设计 A/B 测试来测试改进的假设
9.  转到 3

现在，为了快速、高质量地执行这个流程，并且不使您的用户感到沮丧或烦恼，您需要**持续测试**。这意味着:

*   尽早测试并坚持测试
*   每次代码提交时测试

为此，您需要:

1.  自动化、频繁的集成测试
2.  完全回归运行。如有必要，进行夜间回归测试。
3.  24/7 测试周期
4.  如果发现了一个 bug，尽快修复它(如果不是立即修复)，不要让它在代码库中溃烂
5.  签入的每一行代码都必须包含测试

这里的共同主题是测试自动化，很多很多。

## 测试自动化的现实是什么？

**单元测试**

除非在持续集成(CI)管道中有代码覆盖门来阻止代码合并，通常是由于交付的时间压力，团队可能会跳过这一步并招致技术债务。

**模块级测试**

这通常取决于是否有足够的开发人员资源。

**API 等级测试**

取决于是否有测试资源。

**用户界面/客户端级测试**

*   通常落后功能 0.5 或 1 秒
*   UI 测试经常是“古怪的”或者不可靠的(因为各种各样的原因)
*   当用户界面快速变化时，测试工程师感到沮丧
*   许多团队求助于使用手动测试作为权宜之计，但是最终成为永久的解决方案
*   一些 UI 框架不太适合测试自动化，例如，每个版本都有新的 UI 标识符(比如 CSS 选择器)

在 Codefresh 进行的一项调查中，五分之四的开发人员认为缺乏测试自动化是代码交付的障碍[1]。在另一项调查中，美国和英国金融服务领域 43%的开发人员表示，他们发现很难达到他们经理设定的单元测试覆盖率[2]。另一个数据点是，组织每年浪费大约 3000 亿美元来修复糟糕的代码[3]。在同一份报告中，它说只有 6%的受访者将所有测试自动化。

在之前的一篇文章中，我也提到过大多数开发人员要么没有兴趣，要么没有足够的动机去编写测试。再加上现在大多数服务器端软件架构都变得微服务化，你就有了一个复杂的问题。

# AI/ML 能有什么帮助？Roomba 类比

那么人工智能能帮上什么忙呢？嗯，我喜欢用我所谓的“Roomba 类比”你们中有多少人喜欢用吸尘器打扫房间？我真的怀疑你们中的许多人。然而，这是绝对必要的卫生和整体清洁。有了机器人吸尘器，你可以让它跑来跑去吸走灰尘，而不需要太多的人工干预。这并不意味着你根本不需要吸尘，只是意味着你可以减少吸尘频率，把注意力放在难以触及的地方。

这类似于 AI 在测试 ie。让 AI 或 ML 引擎做大部分隔离 bug 的工作，这样你的测试团队或工程师就可以专注于那些需要更多努力来设计测试的测试。这只是一个用例。还有其他几个。

# 人工智能如何应用于测试过程的一些例子

## 识别和隔离移动应用崩溃(NimbleApp)

在之前的一篇文章中，我提到过像来自 [Headspin](https://www.headspin.io/) 的 NimbleApp 这样的工具可以用来在每次 PR 之后自动抓取你的应用程序以识别崩溃，这样测试工程师就可以专注于更有价值的测试。这就像我之前提到的 Roomba 不知疲倦地寻找崩溃。这使得工程师可以从事更具挑战性的测试，如自动内存泄漏检测(iOS)或清除 JNI 集成问题(Android)。

## 静态代码分析中的 AI(deep code . AI)

符号人工智能是一种使用规则来指导决策的技术。在 80 年代，人们对这种基于规则的专家系统寄予厚望，希望它成为日本第五代计算机的基础。不幸的是，这些目标过于雄心勃勃(他们试图对某些领域的所有规则进行编码)，而且计算并不像今天以云为中心的商用计算模型那样便宜。

然而，这些年来情况发生了变化:(a)计算变得更加便宜和商品化，以及(b)更好地理解了这种工具最适合定义良好和结构化的领域。

一个例子就是 [deepcode.ai](http://deepcode.ai) 。他们使用符号人工智能来执行静态代码分析，这不仅应用了良好编码实践的规则，还克服了污点分析的许多缺点，以隔离代码中可能的攻击媒介。

## 使用用户流量和 ML 来计算覆盖率并自动生成测试自动化(ProdPerfect)

测试管理的一个问题是:我们同等重视每一个测试。这当然不是真的，一些测试比其他测试更容易引起用户错误，仅仅是因为一些功能很少被使用。您可以想象一下，如果您的回归套件在数千个测试用例中运行，并且每个测试用例需要几分钟的时间来运行。 [ProdPerfect 的方法是在您的网站上安装仪器，获取点击流数据，并使用数据分析和机器学习来确定哪些是最重要的测试关键用户流。然后生成测试代码，自动生成端到端的测试套件。](http://prodperfect.com)

## 使用 AI/ML 来决定在部署失败的情况下是否执行回滚(Harness.io)

连续部署(CD)的一个问题是:您如何知道您是否执行了错误的部署并需要立即执行回滚？ [Harness.io](http://harness.io) 旨在通过使用来自 APM 工具(如 Sumo Logic、New Relic、AppDynamics)的可观测性数据来确定是否需要自动回滚，从而解决这个问题。

## 可观察性和预测(Splunk)

Splunk 是目前最流行的系统观察工具之一。基本用例是日志聚合和分析。但是，它们还提供了一个机器学习工具包(MLTK ),允许用户将标准的 ML 算法(如线性和逻辑回归、K 均值聚类等)应用于 Splunk 收集的日志数据。最近，Splunk 还推出了 MLTK 智能助手，使您能够更轻松地执行标准的 ML 操作，如对日志数据进行预测、异常值检测和聚类。

## Bug 分类

在另一系列文章中，我探索了使用深度神经网络和其他传统机器学习算法将吉拉错误分类到错误优先级(例如，主要，次要)的可能性。这些条款如下:

*   [使用 Tensorflow 构建可部署的吉拉 Bug 分类引擎](https://levelup.gitconnected.com/building-a-deployable-jira-bug-classification-engine-using-tensorflow-745667c72607)
*   [使用亚马逊 Sagemaker 构建可部署的吉拉 Bug 分类引擎](https://towardsdatascience.com/building-a-deployable-jira-bug-classification-engine-using-amazon-sagemaker-827fda8d0460)
*   [使用 Google AutoML 建立一个可部署的吉拉 Bug 分类引擎](https://towardsdatascience.com/building-a-deployable-jira-bug-classification-engine-using-google-automl-a0497ad8c475?source=your_stories_page---------------------------)

# 所有这些听起来都很奇特，但是真的有人在用吗？

答案是肯定的。在今年 7 月对 Meetup 集团成员进行的一项调查中:

*   55%的受访者表示，他们正在测试中使用某种形式的人工智能
*   23%的人使用人工智能进行测试生成
*   46%的人使用它进行异常检测、故障分配和重构
*   23%的人用它来进行代码分析
*   30%的人使用它进行测试优化

# 哪里可以找到更多信息

我激起你的兴趣了吗？你可以通过以下方式找到更多信息:( a)加入 attesting.org[的 AI 软件测试协会(AISTA)或者(b)加入我们在](http://attesting.org) [AI in Testing and Testing AI](https://www.meetup.com/AI-in-Testing-and-DevOps/) 的 Meetup 小组。

期待在我们的下一次聚会上见到你！

# 参考

[1]提高测试自动化结果的三个关键成功因素，Hans Buwalda & Tuan Truong，InfoQ，2020 年 1 月 29 日—[https://www . InfoQ . com/articles/Success-Test-Automation/#:~:text = As % 20 developers % 20 explore % 20 ways % 20 to，implement % 20 continuous % 20 testing % 20 in % 20 devops。](https://www.infoq.com/articles/success-test-automation/#:~:text=As%20developers%20explore%20ways%20to,implement%20continuous%20testing%20in%20DevOps.)

[2]研究发现金融服务开发人员难以达到测试目标，Jason Caines，软件测试新闻，2020 年 2 月 17 日—[https://www . Software Testing News . co . uk/Study-founds-financial-services-developers-fight-to-match-test-targets/](https://www.softwaretestingnews.co.uk/study-finds-financial-services-developers-struggle-to-meet-test-targets/)

[3]质量、速度与 DevTestOps 方法不相互排斥，Nancy Kastl，信息周刊，2020 年 6 月 11 日—[https://www . information week . com/devo PS/Quality-Speed-Not-mutual-Exclusive-with-DevTestOps-Approach/a/d-id/1337985](https://www.informationweek.com/devops/quality-speed-not-mutually-exclusive-with-devtestops-approach/a/d-id/1337985)