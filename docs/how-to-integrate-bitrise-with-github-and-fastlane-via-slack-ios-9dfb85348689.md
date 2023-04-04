# 如何通过 Slack Part 1 (iOS)将 Bitrise 与 Github 和 fastlane 集成

> 原文：<https://blog.devgenius.io/how-to-integrate-bitrise-with-github-and-fastlane-via-slack-ios-9dfb85348689?source=collection_archive---------14----------------------->

![](img/ac4f383e9927194ae4b483efe1d87809.png)

马库斯·温克勒在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

我们所有人都花了很多时间来运行单元测试，另一个花了很多时间来发送用于 QA 的 TestFlight 或在 App Store 上发布构建，所以这里有 3 个部分来拯救我们的生活。

**1-** 如何将你在 Github 上的项目与 [Bitrise](https://www.bitrise.io/) 集成，以节省运行单元测试的时间。
**2-** 如何将您的项目与 [fastlane](https://fastlane.tools/) 整合，以节省发送试飞和发布的时间。
**3-** 如何通过 [Slack](https://slack.com/) 运行你的 faslane 命令行如(testFlight_production，testFlight_Stage 或 release)。

让我们从第一部分开始。

# 1-在 Github 上创建一个新的资源库。

![](img/f8abdb50890f52bb2dfaa914ccc62ab7.png)

# 2-在 Xcode 上创建一个新项目。

让我们在 Xcode 上创建一个新项目，并编写一些单元测试，然后将代码推送到 Github 上，这样我们以后就可以在 Bitrise 上使用这些单元测试了。

![](img/1384d354c15fa0e3923e324ce9b8ed0b.png)

# 3-在 Github 上推送你的代码。

![](img/475ba717d152656a699a696e1bc6f5ec.png)

# 4-在 Bitrise 上创建一个新的应用程序。

首先，你必须有一个 Bitrise 帐户，创建一个很容易，一旦你创建了一个，你将选择添加你的第一个应用程序。

![](img/3fc6d14c5f04598b6827261757610e24.png)

## **下面是添加你的第一个应用的步骤:**

**1-** 选择你的账号和 app 的隐私，我们会为教程选择公共隐私。

![](img/2a46c7a8bc73c34ce789576c8c558087.png)

**2-** 将我们在 Github 上创建的项目连接到 Bitrise。

![](img/3bb2c2a9fcaf8c52169783363437d1cf.png)

**3-** 选择你项目的资源库，设置分支，所以这里的分支就是**主**。

![](img/2d85309a15b11ec322c7e911ee904347.png)

验证你的存储库，但在此之前，确保你的 Xcode 项目有[共享方案](https://devcenter.bitrise.io/troubleshooting/frequent-ios-issues)。

![](img/0ee9c8343676ff64b068b227585281ea.png)

**5-** 设置 Webhook，让 Bitrise 在每次你把代码压入你的库的时候自动开始构建。

![](img/356a600f0b2d7ff78aea80578bec7fe4.png)

祝贺👏，您已经创建了您的第一个应用程序。

**6-** 在你的应用上创建你的新工作流，比如说**运行单元测试**

![](img/0886dfe766da266e959545666fa6d9b5.png)

**7-** 选择 Triggers 选项卡并添加一个触发器，这样您就可以在每次在 Github 上推送新的提交时自动运行这个工作流。

![](img/d46ad290fecd418040d29e8b01682207.png)

当您向您的存储库提交一个新的提交时，您将有 **Run_Unit_Tests** 工作流像这样自动触发。

![](img/73475ddec4054275ad919fadcd93ecb8.png)

**9-** 恭喜恭喜🎉，您第一次提交了您的单元测试。

![](img/83142ce5fbf043afa6bb97ebfb44fd67.png)

[Github 项目](https://github.com/deda9/Bitrise-Example)

你可以在[第二部分](https://medium.com/@deda9/how-to-integrate-bitrise-with-github-and-fastlane-via-slack-part-2-ios-456ac73d0b83)中了解更多，这更有趣😄