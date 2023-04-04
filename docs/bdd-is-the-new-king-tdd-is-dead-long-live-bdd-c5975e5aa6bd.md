# BDD 是新的王者。TDD 已死，BDD 万岁

> 原文：<https://blog.devgenius.io/bdd-is-the-new-king-tdd-is-dead-long-live-bdd-c5975e5aa6bd?source=collection_archive---------0----------------------->

![](img/7358d1888c0304edbf1e0a104d16ed46.png)

2016 年 4 月，我第一次接触到 BDD 框架。我认为它很棒，并一直在我的项目中使用这个框架。可以说，这是“在纸上”设计一个软件的好方法。然后针对这种设计进行编码，以确保您正在构建“正确的东西”。

抱歉，如果我的博客标题有点误导。我不相信 TDD 完全死了。当我写新代码时，有一个 TDD 的位置。然而，我的大多数测试都是使用 BDD 测试来执行的。

自 2019 年 7 月以来，我一直在从事一个名为 [Outflash](https://outflash.xyz/) 的兼职项目。我在这个项目中一直使用 BDD 框架。帮助我设计我想要的解决方案。确保我已经考虑到了所有可能发生的事情。

# 什么是 BDD？

BDD 代表**行为驱动开发**或**业务驱动开发**。这是一种从 [TDD](https://blog.testlodge.com/what-is-tdd/) (测试驱动开发)发展而来的软件开发方法。它不同于 TDD，因为每个测试都是以人类可读的格式编写的。这改善了技术团队、非技术团队和利益相关者之间的沟通。

这有助于那些不是特别懂技术的人参与设计过程。

一家企业可能会雇佣一些拥有丰富专业知识并了解特定市场的人。然而，他们可能没有技术背景来交流他们需要一个软件来做什么。BDD 通过使用简单的英语弥合了这一鸿沟。

BDD 写作会议产生技术和非技术人员都可以参考和理解的测试用例。

在 TDD 和 BDD 开发方法中，测试都是在编写任何代码之前编写的。然而，BDD 测试更关注用户或过程，并且基于系统的行为。

# BDD 是如何构建的

当编写 BDD 测试时，通常称为场景。一个测试用例(场景)通常是由以下格式构成的。

**给定**某个场景
**当**一个动作发生
**那么**这应该是结果。

例如，这将是为新的网站应用程序实现登录屏幕的场景。

**假设**用户未在登录屏幕
**上输入任何数据，当**用户点击登录按钮
**时，则**应显示正确的验证消息。

这是一个很基本的例子。希望展示 BDD 测试文件是如何构造的。在这篇文章的后面，我会给你一个更详细的真实世界的例子。

# 使用 BDD 框架的好处

使用 BDD 框架，您不需要定义“测试”(一段代码的特定结果)。你在定义“行为”。通过使用 BDD 框架，您不仅可以测试代码块是否按预期工作。您还要测试不同的代码块是否能正确地协同工作，以产生预期的结果。

使用 BDD 改善了开发人员、测试人员、产品所有者和企业其他成员之间的交流。因为测试文件是以人类可读的格式(Given，When，Then)编写的，这使得人们更容易理解。帮助接触更广泛的受众。

另一个好处是开发人员、测试人员和其他团队成员的学习曲线缩短了很多。有人可以理解需要实现什么，以及如何在很短的时间内构建一个流程。

使用 BDD 测试，在编写一行代码之前，就定义了应该如何工作和测试的验收标准。这可以防止软件团队由于缺乏理解而构建和测试错误的东西。

使用 BDD 也加快了开发过程。这听起来与直觉相反，因为看起来你写的代码量是你需要达到的结果的两倍。从我的经验来看，首先编写 BDD 场景可以帮助你减少代码量，从而使测试场景完整。BDD 还帮助开发人员坚持[坚实](https://scotch.io/bar-talk/s-o-l-i-d-the-first-five-principles-of-object-oriented-design)和[干净代码原则](https://simpleprogrammer.com/clean-code-principles-better-programmer/)。

我还发现使用 BDD 可以更容易地向我的代码添加更多的功能和新特性。要实现改进，需要在现有或新的场景中添加更多的给定内容，以确定您想要添加哪些改进或特性。

# 真实世界的例子

如果您曾经尝试过任何新的编程语言，那么您将会熟悉如何实现旧的“Hello World”示例。我个人不喜欢以 Hello World 作为起点。我知道你需要低门槛。但是，我觉得并没有体现出编程的强大。

希望下面真实世界的例子能够展示 BDD 和软件的力量。不涉及太多的技术细节。

我已经提到了我从 2019 年 7 月开始从事的项目。我的侧滑叫[外闪。Outlook.com 的#1 电子邮件增强插件](https://outflash.xyz/)。我不会详细介绍它是什么以及它是如何工作的。电子邮件增强插件的主要目标是允许用户使用 Outlook 发送大量电子邮件。

作为解决方案的一部分，我需要一个处理器来代表用户发送电子邮件。以确保它如我所愿地工作，并涵盖我需要的所有步骤。我使用了一个名为 [SpecFlow](https://specflow.org/) 的 BDD 框架。

使用框架。我记录了使用 Given When Then 成功发送电子邮件的场景。以及确保我知道在处理过程中出现问题时该怎么做。

# 初始电子邮件场景大纲

下面是我为电子邮件处理器整理的 BDD 场景的第一个版本。以确保我的代码可以代表用户检索和发送电子邮件。

```
Scenario: Email in queue is sent to a valid contact Given A new email processor And The following emails are in the queue | FromEmailAddress | ToEmailAddress | DraftId | | from@test.com | to@test.com | draftId | And The following user is retrieved | Firstname | Lastname | EmailAddress | | Phil | Test | from@test.com | And A draft email is returned And A valid contact is returned | GivenName | Surname | CompanyName | | PHH | Digital | PHH Digital | When The queued emails are sent Then The retrieve emails was called '1' times And The retrieve user was called '1' times And The retrieve message was called '1' times And The retrieve contact was called '1' times And The message was sent And Update email was called '1' times
```

如你所见，我已经用简单的英语写下了我希望处理器做什么。很容易理解我想你会同意的？

这种结构也让我受益匪浅，因为我有一个明确的重点，我想建立。它们是如何组合在一起的。应该发生什么，什么时候。清楚地定义处理排队电子邮件的预期结果。

有一点我想提一下，我并没有完全抛弃 TDD 框架。我仍然使用测试驱动开发来确保像构造函数这样的关键东西得到预期的参数。或者测试对软件内部工作至关重要的关键功能。

# 更新和改进

我只想快速向您展示向 BDD 场景中添加新功能并针对它进行编码是多么容易。

```
Scenario: Email in queue is sent to a valid contact Given A new email processor And The following emails are in the queue | FromEmailAddress | ToEmailAddress | DraftId | | from@test.com | to@test.com | draftId | And The following user is retrieved | Firstname | Lastname | EmailAddress | | Phil | Test | from@test.com | And A draft email is returned And A valid contact is returned | GivenName | Surname | CompanyName | | PHH | Digital | PHH Digital | When The queued emails are sent Then The retrieve emails was called '1' times And The retrieve user was called '1' times And The retrieve message was called '1' times And The retrieve contact was called '1' times ***And The replace placeholders was called '2' times And The unsubscribe placeholder was called And The append footer was called*** And The message was sent And Update email was called '1' times
```

在更新的场景中，我添加了 3 个步骤，用粗体和斜体突出显示。我希望能够在电子邮件模板中添加占位符，并用 Outlook 联系人记录中的信息替换它们。

当我运行测试场景时，它失败了，因为新的行没有被满足。我没有花任何时间来编写这个功能的代码，并将其部署到生产中。

这就是了。使用一个真实的例子快速概述 BDD，说明如何在您的项目中使用该框架..

感谢阅读 BDD 是新的国王。TDD 死了。BDD 万岁。我想听听你的想法。哲学（philosophy 的缩写）

如果你想阅读更多我的[技术博客文章](https://www.philliphughes.co.uk/category/tech/)，你可以[这里](https://www.philliphughes.co.uk/category/tech/)。

*原载于 2020 年 2 月 27 日 https://www.philliphughes.co.uk**[*。*](https://www.philliphughes.co.uk/tech/tdd-is-dead-long-live-bdd/)*