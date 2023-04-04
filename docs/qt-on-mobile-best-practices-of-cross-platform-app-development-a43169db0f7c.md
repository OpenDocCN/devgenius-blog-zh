# 移动平台上的 Qt:跨平台应用开发的最佳实践

> 原文：<https://blog.devgenius.io/qt-on-mobile-best-practices-of-cross-platform-app-development-a43169db0f7c?source=collection_archive---------5----------------------->

是什么让一个 app 有市场？一款应用要想成功，需要满足很多要求。在本文中，您可以学习在移动设备上使用 Qt 开发成功应用程序的最佳实践。

*注:本文是我们的首席执行官&联合创始人 Chris 在* [*Qt 虚拟技术大会 20*](https://blog.felgo.com/events/meet-felgo-at-qt-virtual-tech-con-2020) *上发表的会议摘要。在他的演讲中，他分享了他对移动优化应用的最佳实践以及如何克服 iOS 和 Android 设备上的移动优化挑战的深刻见解。点击下面的视频可以观看完整的会议:*

# 从为什么开始:你的应用程序的目的

作为移动开发的第一个最佳实践——这在一般情况下适用，不仅仅适用于 Qt——请问自己这个简单的问题:**你为什么实际上想要构建一个移动应用程序？这个“为什么”的问题非常重要，因为许多应用程序只显示简单的信息，但用户不想下载应用程序只是为了获得一些在网站上也可以获得的信息。**

用户真的想从移动应用中获得真正的价值。你怎么能这样做？您可以通过使用手机的独特功能来提供价值。例如，你可以通过简化应用程序来做到这一点。您可以利用以下任何[独特功能](https://felgo.com/doc/)为您的应用增值:

*   手势
*   触摸控制
*   位置传感器和模块
*   推送通知
*   传感器
*   照相机
*   相机顶部的增强现实

获得正确的价值后，你应该致力于用出色的用户界面(UI)和用户体验(UX)给用户留下深刻印象，从而真正有机会在所有其他移动应用中脱颖而出。

目前，Google Play 中有超过 300 万个移动应用，iOS 应用商店中也有大约相同的数量。所以要想鹤立鸡群，目标应该是真正把东西做得更好。

# 伟大的 UX 在手机上到底意味着什么？

核心部分是提供原生用户体验。原生意味着它符合 iOS 或 Android 等底层平台的设计准则。

![](img/0f73429e76975d85fca9a6344df6d061.png)

举几个例子，先从[导航](https://felgo.com/doc/apps-howto-use-app-navigation/)说起。iOS 和 Android 上的导航差别很大。

iOS 用户习惯于使用向后滑动手势在不同页面之间导航。这很有意义，因为在 Android 上，你可以按底部的后退按钮，但在 iOS 上，没有内置的按钮可以返回。

如果你在页面顶部只有后退按钮，这意味着用户将不得不返回到手机的顶部。随着手机变得越来越大，这可能有点棘手。这也意味着用户需要用双手来控制应用程序。这就是为什么在 iOS 上使用后扫导航真的是最佳实践的原因。否则，用户可能会觉得很奇怪，这可能会导致用户放弃应用程序。

另一方面，在 Android 上，你有一个后退按钮导航和一个“物理”后退按钮。此外，与 iOS 不同，你通常还有一个导航抽屉。在苹果智能手机上，大多数时候标签导航栏都在底部。

![](img/28c56bd61ed1497c169049005b284d55.png)

除了导航之外，其他差异可能很细微，但是如果将两个平台之间的所有差异加起来，就会产生巨大的差异，例如:

*   滚动:由于实际滚动的速度、速率和加速度，滚动列表的外观和感觉是不同的
*   滚动条的外观
*   小跟班
*   字体类型和大小
*   对话框:像本地和共享对话框
*   日期和时间选择器

下面是 Android 和 iOS 上的原生对话框的快速演示:

![](img/822b0c295ea0e6a6c659bd3fb08ae5f9.png)

[Qt Quick Controls 2](https://felgo.com/doc/qt/qtquickcontrols2-index/) 给你极大的灵活性来改进单个控件的设计。然而，让所有这些原生细节都适用于 Android 和 iOS 将是一项艰巨的任务。但如果没有它，你的应用程序就不会像一个原生的移动应用程序。

这也是近十年前 [Felgo SDK](https://felgo.com/) 诞生的原因之一。Felgo 用 200 多个额外的 API 扩展了 Qt，使移动开发变得更加简单。最初，主要的焦点是在移动设备上，但是在过去的几年里，在移动设备的 UX 方面，最佳实践也扩展到了其他平台。尤其是在嵌入式领域，类似智能手机的 UX 最近正在成为最佳实践。

此外，如果你看看桌面应用程序，你会发现移动世界对用户体验的影响越来越大。如果和十年前相比，很多事情都朝着积极的方向改变了。

关于您可以使用的 API，有一个积极的趋势，因为您不局限于移动平台。现在，您也可以在嵌入式和桌面设备上使用它。

除了 SDK，Felgo 还提供专业服务，帮助团队开发适用于移动、嵌入式和 web 等所有平台的应用。Felgo 还在现场和远程举办培训，这在当今更为普遍。

# Felgo APIs 有哪些？

200+[Felgo API](https://felgo.com/doc/apps-api/)是 fel go 在 Qt 之上添加的，这意味着除了下表中用橙色标记的 API 之外，您还可以使用全系列的 QML API。正如你所看到的，不仅 UI 和控件有不同的功能和特性，而且它们还围绕着逻辑、原生功能等等。哪些 API 和组件为您和您的项目提供了最大的价值取决于用例。

以下是移动平台最常见的 API 和组件的快速浏览:

![](img/b498459a26a70fb4ad6ffce8918249a2.png)

# 适应性布局/响应性布局

先说[自适应布局](https://felgo.com/doc/apps-supporting-multiple-screens-and-screen-densities/)或者也叫响应式布局。这意味着你的应用适应/响应屏幕大小，以利用所有可用空间。例如，在平板电脑上，你可以显示与智能手机不同的用户界面，因为有更多的可用空间。仅仅扩大规模并不是一个好主意。

Felgo 组件会自动进行调整。当屏幕变大时，它们会自动改变，例如并排显示附加内容。真正的好处是，您可以以此为基础，根据自己的喜好进一步定制，而无需从一开始就考虑这一切。

![](img/82dfae63dfe4f671da824e3738a3e34c.png)

出于模拟目的，您还可以在桌面上更改目标平台和屏幕大小。

# 切口/显示切口支持

另一个经常被忽视，但却是 UX 的一个关键部分的话题，是对[凹槽或显示切口](https://blog.felgo.com/cross-platform-app-development/notch-developer-guide-ios-android)的支持。凹口/显示器切口是屏幕的一部分被框架部分覆盖的顶部和底部区域。由于越来越多的移动设备采用这种设计，这变得越来越重要。

你必须避免在这个区域显示任何关键内容，因为在纵向和横向模式下，这些顶部和底部区域可能会重叠。

![](img/e0c4f639cd032007137ed47e37a57b0c.png)

使用 Felgo SDK，您可以在 iOS 和 Android 上获得完全支持这种设计的组件。您可以完全控制安全区域，以及您选择是否在那里渲染。

![](img/28a2e42a0c95bc77906906d9585b9871.png)

# 第三方集成

对于许多功能和服务，集成第三方服务是有意义的，甚至是必需的，例如广告、分析等。

[Felgo 插件](https://felgo.com/plugins)提供现成的第三方服务集成，例如:

下面是一个将远程推送通知插件与 OneSignal 集成的示例:

![](img/c13c8ea02016fc68ecad0def211c6e53.png)

第三方服务的一个常见问题是确保它们与最新的 Qt、iOS、Android 和 Android SDK & NDK 版本协同工作。这可能会非常耗时。Felgo 会为您处理这些，并负责维护和更新插件。

如果您需要帮助来集成一个现有插件，或者将另一个第三方服务与 Qt 或 Felgo 集成，您可以联系 Felgo 专业服务团队。

# 增强现实和机器学习

![](img/d46463ee2b7ff5455e19048b5525ab1d.png)

一个更高级但越来越重要的功能是增强现实。有了 Felgo，你可以通过 AR 插件使用这个特性。

您可以在文档中找到 AR、ML 和 3D [等主题的示例和集成指南。](https://felgo.com/doc/apps-howto-3d/)

# 显色法

创建优秀应用的另一个关键步骤是经常测试，尤其是在你的移动设备上。Qt 开发人员通常倾向于在桌面上进行开发和测试，而只是偶尔部署到移动设备上。

## 通常的开发过程在移动设备上是什么样的？

![](img/40b6bb373284652ff78931df84efb6b7.png)

首先，你写你的代码，点击保存，编译，部署，然后在你的手机上测试。关键的区别在于，它在移动设备上比在桌面上花费更长的时间。在桌面上，根据您的项目，可能需要几秒到几分钟的时间。

在移动设备上，根据您的项目，这可能需要几分钟的时间。如果您考虑到您可能会在多种设备上进行测试，如智能手机和平板电脑或 Android 和 iOS，那么这种总结会非常快。

你很可能一天要测试你的代码几十次或者几百次。很多时候，当你在等待你的应用程序被部署时，你会失去对编码本身的关注。

# QML 实况转播

这就是为什么 Felgo 创造了一个解决这个问题的方案，叫做 [QML 生活 Roload](https://blog.felgo.com/updates/release-2-14-0-live-code-reloading-for-desktop-ios-android) 。Felgo 的解决方案将整个部署和测试过程缩短到一秒钟。

![](img/0f3b57c0bb09c11917654bb7664c7e2c.png)

这是如何工作的？QML Live Reload 是一个独立的应用程序，添加到您的 Qt Creator 环境之外，如下所示。它与 Qt Creator 直接集成。当检测到更改时，所有连接的客户端会同时更新。代码被传输到这些设备并立即重新加载。这不仅发生在桌面应用上，也发生在连接的 iOS 和 Android 设备上。这是一个巨大的时间节省，因为你只需要等待一秒钟，而不是几分钟。

# 热代码重新加载

普通部署和 QML 实时重载都有一个共同的问题:你总是从应用程序的根目录开始，并且必须导航到你当前想要测试的页面。

Felgo 通过名为 [QML 热重装](https://felgo.com/qml-hot-reload-for-qt-with-felgo-live)的下一代代码重装解决了这个问题。

QML 热重装与 Felgo Live 允许您更改您的 QML 和 JavaScript 源代码，并查看实时结果。它在保存后立即在每台连接的设备上应用 QML、JavaScript 和资产更改。

QML 热重装应用源代码中的更改，而不会丢失应用程序的状态。如果你现在正在一个子页面上工作，你不需要在每次改变后都导航回它，你将停留在你的应用程序中的正确位置。

![](img/75fb264133077982b51ed5fa0348a0c5.png)

# QML 热重装的好处

这改变了你用 Qt 编码的方式。使用 QML 热重装的一些主要优势是，您可以:

*   经常保存你的代码
*   做更小的整合
*   更加渐进地构建用户界面
*   不仅测试简单的东西和改变属性
*   改变整个范围:添加新的 QML 文件，并修改多个变化一次

总之，QML 热重装是一个独立的工具，可以在 Qt Creator，Qt Design Studio，Qt 3D Studio，Visual Studio，其他文本编辑器 ide 上工作，可以完美地与任何 Qt 和 Qml 项目一起工作。您不需要使用任何 Felgo APIs，因为它可以与任何现成的股票 Qt 应用程序一起工作。QML 热重装工程的移动桌面，嵌入式和网络应用。

由于只有 QML 和 JS 代码可以在没有编译步骤的情况下热重载，所以建议在应用程序中尽可能使用 QML。将 QML 和 JS 不仅用于 UI，还用于对时间要求不是很严格的应用程序逻辑，这是非常好的。

当然，这并不意味着你需要放弃 C++，你仍然可以使用它与 QML 和使用 QML 热重装的 QML 部分。

# 空中更新(OTA)

酷的是，你可以使用这项技术进行 OTA 更新，这样你就可以发布新的移动、桌面和嵌入式应用程序更新。只要有互联网连接，你就可以用同样的技术无线更新这些实时应用。OTA 也在[嵌入式](https://felgo.com/embedded)上工作。

# 移动应用发布流程

移动应用的最后一个实践是向应用商店发布的实际过程。通常的流程是团队中的一名成员，通常是开发人员或构建工程师，负责设置构建 PC 并负责实际的构建过程。那么 app store 发布就是一个手动的过程。

但是，项目还在继续，问题还会出现。当 Qt、Android、iOS 或 Android NDK 版本改变时会发生什么？当这些版本不能一起工作时，你会怎么做？当 Android 需求或 SDK 改变时会发生什么？有很多情况可能对产品不利。

![](img/27133efaea697bc6d910759e97ef5696.png)

# Felgo 云构建

作为这个问题的解决方案， [Felgo Cloud Builds](https://felgo.com/cloud-builds) 应运而生。基本上，您将 Git 存储库与 Felgo Cloud Builds 服务器连接起来。它们都可以在网上或你自己的服务器上运行。

然后，您可以直接在云中为您的目标平台构建应用程序。

云构建允许您自动构建实际的应用程序。然后，您可以分发这些二进制文件。也可以直接将二进制文件自动上传到 iOS 应用商店和谷歌 Play 商店。下一步，你可以将它与 Apple Test Flight 连接，以将你的测试版分发给 Apple Test 用户，或者分发给谷歌 Play 商店进行测试。如果你愿意，也可以让它瞬间活起来。

云构建的一个巨大优势是，一旦建立起来，一切都是自动化的，并且易于使用。

云构建还允许您集成定制的构建、测试或部署步骤。

# Webassembly 的 Felgo

你也可以通过 [Felgo for Webassembly](https://blog.felgo.com/updates/release-3-5-0-run-qt-apps-in-browser-webassembly-wasm) 从相同的源代码发布你的应用程序。

Felgo for WebAssembly 帮助您简化开发流程，并为所有部署目标(包括 web)维护一个通用的代码库。

使用 Felgo for WebAssembly，您可以将生产就绪的 Felgo 和 Qt 应用程序放到 web 上！

您也可以直接从 Qt Creator 使用 QML 热重装 WebAssembly:

![](img/8b46fbb55021c6e9471b683498444947.png)

# 结论

为了总结最重要的最佳实践，这里有一个快速概述:

*   为你的用户提供真正的价值。确保你得到正确的价值主张。
*   用尊重平台标准的优秀用户界面和 UX 给你的用户留下深刻印象。
*   在开发过程中经常测试你的应用程序，以获得正确的用户界面和 UX。
*   经常发布更新。不幸的是，经常更新的重要性经常被忽视。自动化的 CI/CD 可以帮助您缩短发布周期。
*   分析用户对你的产品做了什么，这样你就可以根据反馈改进你的应用。
*   优化你的应用商店入口。

[Felgo SDK](https://felgo.com/download) 和专业 [Felgo 开发服务和培训](http://felgo.com/qt-training-consulting-services)可以帮助你在使用 Qt 和/或 Felgo 开发应用的过程中迈出任何一步。

Qt 上最重要的 Felgo SDK 新增功能有:

*   Qt 上的 200 多个 Felgo 组件
*   更好的用户界面/UX
*   更好的 Qt 应用
*   更多功能
*   较少代码
*   更快的开发时间
*   QML 热点和现场重新加载
*   Felgo 云构建 Qt CI/CD
*   用于第三方服务集成的 Felgo 插件

# 相关文章:

[如何向您的应用添加推送通知](https://blog.felgo.com/ios-development/local-push-notifications-for-your-apps-and-games)

[如何给你的应用添加增强现实](https://blog.felgo.com/cross-platform-development/qt-ar-why-and-how-to-add-augmented-reality-to-your-mobile-app)

[Qt 初学者教程](https://blog.felgo.com/qt/qt-tutorials-resources-beginners)

[持续整合并交付 Qt & Felgo](https://blog.felgo.com/blog/continuous-integration-and-delivery-for-qt-and-felgo)

[如何将机器学习添加到你的 App 中](https://blog.felgo.com/cross-platform-development/machine-learning-add-image-classification-for-ios-and-android-with-qt-and-tensorflow)

# 阅读更多信息:

[多屏幕尺寸响应式设计的终极指南](https://blog.felgo.com/cross-platform-app-development/the-ultimate-guide-to-responsive-design-for-multiple-screen-sizes)

![](img/b0329f413de77cfa582b9c0016322505.png)

[iPhone X 支持和运行时屏幕方向变化](https://blog.felgo.com/updates/release-2-16-0-iphone-x-support-and-runtime-screen-orientation-changes)

![](img/832bf18e44db7c44eff23bac66c1a949.png)

[Firebase QML 教程:如何创建 Android 和 iOS 应用](https://blog.felgo.com/cross-platform-app-development/firebase-qml-tutorial-android-ios-create-an-app-with-realtime-database-and-login)

![](img/2e8b7bce960ef3734c15526391f4d06f.png)

*原载于*[](https://blog.felgo.com/cross-platform-app-development/best-practices-of-qt-on-mobile)**。**