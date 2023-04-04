# 2020 年 iOS 开发快速入门指南

> 原文：<https://blog.devgenius.io/the-quick-guide-to-getting-started-with-ios-development-in-2020-b1aaa97649ae?source=collection_archive---------5----------------------->

![](img/b5213c35629d319b17390532c15e2ace.png)

蓝菊·福托雷菲在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

你有苹果电脑吗？如果是这样，那么你已经拥有了开始为 iPhones 和 iPads，以及任何其他运行 iOS 操作系统的设备编写应用程序所需的几乎所有东西。在本指南中，我将带你了解关于设置开发环境、编写应用程序以及将应用程序分发给用户所需的知识。

iOS 的新应用通常是用 [Swift](https://developer.apple.com/swift/) 编程语言编写的。如果您不熟悉 Swift 语言，有许多资源可以帮助您学习。如果你是编程新手，并且正在寻找一种有趣的方法，那就去看看 Swift Playgrounds 。如果你是一个已经熟悉类似 C 语言的有经验的开发人员，那么你可能更喜欢从类似于[这个](https://github.com/reinder42/SwiftCheatsheet/blob/master/swift-cheatsheet.md)或者[这个](https://koenig-media.raywenderlich.com/uploads/2019/11/RW-Swift-5.1-Cheatsheet-1.0.1.pdf)的备忘单开始。

自 2014 年发布以来，Swift 语言已经发展了很多。Swift 目前的版本是 5.2 版，发布于 2020 年 3 月 24 日。当您寻找 Swift 学习资源时，请注意正在使用的 Swift 版本。Swift 3 发布时发生了一些重大变化，Swift 4 发布时也发生了较小程度的变化。从那时起，源代码兼容性在不同的版本之间得到了维护。因此，涵盖 Swift 4 或更高版本的资源可能仍然相关；那些涵盖早期版本的可能没那么有用。

iOS 的应用程序也可以使用 [Objective-C](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/Introduction/Introduction.html) 编程语言编写。从 2008 年第一个 iOS SDK 发布到 2014 年 Swift 发布期间，Objective-C 是编写 iOS 应用程序唯一支持的语言。您将经常在较旧的 iOS 开发资源中遇到 Objective-C 代码，由较大公司维护的遗留 iOS 应用程序通常是用 Objective-C 编写的。很难找到关于 Objective-C 开发的最新资源，我强烈建议您首先关注 Swift。

[Xcode](https://developer.apple.com/xcode/) 是用于编写、运行、调试、测试和分发 iOS 软件的集成开发环境(IDE)。你可以[在这里](https://apps.apple.com/us/app/xcode/id497799835?mt=12)从 Mac 应用商店免费下载。你需要一个[苹果 ID](https://appleid.apple.com) 来下载和使用 Xcode。如果你还没有苹果 ID，你可以[在这里](https://appleid.apple.com/account#!&page=create)免费创建一个。

要编写 iOS 软件，你需要知道的不仅仅是 Swift 语言；您还需要了解工具和框架。苹果的[*Xcode*](https://developer.apple.com/videos/play/wwdc2019/404/)入门视频很好的介绍了开发环境。

如果你是 iOS 和 Swift 的完全初学者，那么来自[raywenderlich.com](https://www.raywenderlich.com)的免费 [*你的第一个 iOS 和 SwiftUI 应用*](https://www.raywenderlich.com/4919757-your-first-ios-and-swiftui-app) 视频课程将是学习如何编写你的第一个应用的一个很好的选择。

如果你更喜欢你的教程是基于网络的，而不是基于视频的，那么一个好的选择是浏览苹果的 [*学习用 SwiftUI*](https://developer.apple.com/tutorials/swiftui/tutorials) 制作应用系列教程。

这三种资源都使用苹果新的 SwiftUI 框架来构建用户界面。然而，你在网上找到的许多现有的 iOS 内容将基于 UIKit 框架。如果你预计需要使用基于 UIKit 的项目，你可能想从 raywenderlich.com 的 [*你的第一个 iOS 和 UIKit 应用*](https://www.raywenderlich.com/5993-your-first-ios-and-uikit-app) 视频课程开始。

要运行、调试和测试 iOS 软件，您可以使用在 Mac 上运行的 iOS 模拟器，或者连接到 Mac 的 iOS 设备。模拟器和连接的设备通过 Xcode 进行管理。

要分发 iOS 软件，你需要一个苹果开发者计划[会员](https://developer.apple.com/support/compare-memberships/)。费用为 99 美元/年。你可以[在这里](https://developer.apple.com/programs/enroll/)注册，但是也可以等到你写了你想发布的东西再注册。

发布 iOS 软件最常见的方式是通过[应用商店](https://www.apple.com/ios/app-store/)。然而，有很多准则需要遵循，你的应用完全有可能在第一次尝试时就无法通过[应用审查](https://developer.apple.com/app-store/review/)，或者永远无法通过。

幸运的是，其他分发方法是可用的。例如，您可以使用临时分发将您的应用程序分发到多达 100 个特定的 iOS 设备。然而，如果你不拥有你想要分发应用的所有设备，这种方法是乏味的。

另一种方法是 [TestFlight，](https://developer.apple.com/testflight/)这是苹果的一项免费服务，允许你向多达 10，000 名你选择的测试者分发你的应用程序和更新，你可以通过电子邮件地址邀请他们。如果你只是想与家人和朋友分享你的应用，而不想麻烦应用商店的审查，那么测试飞行分发将是最简单的选择。

在发布您的 iOS 应用程序之前，您应该查看 Apple 的 [*准备您的应用程序以供发布*](https://developer.apple.com/documentation/xcode/preparing_your_app_for_distribution) 和 [*发布您的应用程序以供测试和发布*](https://developer.apple.com/documentation/xcode/distributing_your_app_for_beta_testing_and_releases) 指南。

只要在 Xcode 项目中启用了“自动管理签名”设置，您可能就不需要了解签名证书和预置描述文件如何工作的全部细节。即便如此，读一读 [*什么是 app 签约还是无伤大雅的？*](https://help.apple.com/xcode/mac/current/#/dev3a05256b8) 引。

App Store Connect 现在要求应用程序在被接受分发之前，其资产目录中必须有应用程序图标。有关更多信息，请查看《创建资产目录和集 》指南。Nicolas Miari 提供的免费[图标集创建工具](https://apps.apple.com/us/app/icon-set-creator/id939343785?mt=12)对于创建您需要的所有不同尺寸非常有用。

我建议你在尝试通过应用商店发布应用之前，通过 TestFlight 将你的应用发布给 beta 测试者。

要通过 TestFlight 发布您的应用程序，您应该首先使用 Xcode 将您的应用程序版本上传到 [App Store Connect](https://appstoreconnect.apple.com) 。此处描述了[的工作流程。页面上有很多有用的信息，但需要知道的关键是，在您可以从 Xcode 上传应用程序版本之前，您必须在 App Store Connect 中创建一个应用程序记录。](https://help.apple.com/app-store-connect/#/dev300c2c5bf)

在您使用 TestFlight 测试了您的应用程序后，您可以按照 [*通过 App Store 分发应用程序*](https://help.apple.com/xcode/mac/current/#/dev067853c94) 指南将您的应用程序发布到 App Store。

我希望那有帮助。还有很多东西要学，但这应该是你的开始。

当你准备好了解更多信息时，关于特定 iOS 技术的最权威的信息来源几乎总是苹果开发者视频。直接听取构建这些技术的工程师的意见通常是非常宝贵的。