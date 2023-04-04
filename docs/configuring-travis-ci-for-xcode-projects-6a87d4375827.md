# 为 Xcode 项目配置 Travis CI

> 原文：<https://blog.devgenius.io/configuring-travis-ci-for-xcode-projects-6a87d4375827?source=collection_archive---------2----------------------->

使用 CocoaPods 运行多个脚本并为项目配置

![](img/df3250bddc34b8e5fdfd438e0f944565.png)

Travis CI 徽标取自[文档](https://docs.travis-ci.com/user/for-beginners/)

嗨，

在本文中，我将分享如何配置？yml 文件，使其自动运行应用程序的测试套件，运行多个脚本，并构建和测试使用 CocoaPods 的项目。

> 注意:本文假设您已经在存储库中设置了 travis，并且拥有. travis.yml 文件

# 1.出发点

最初，您的. travis.yml 文件可能看起来像这样—

这个 Travis 配置足以构建您的 Xcode 项目并运行测试。然而，我们想考虑和解决一些额外的使用案例，例如—

1.  让 Travis 运行一些其他的脚本(例如 Swiftlint)
2.  让 Travis 工作，以防项目使用依赖管理器(例如 CocoaPods)

在接下来的章节中，我们将逐一解决这两种常见的用例—

# 2.在 Travis 中运行多个脚本

要运行多个脚本，例如 Swiftlint + Build Xcode 和 Test Suite，我们可以像这样配置. travis.yml 文件—

在上面的 Travis 配置中，我们添加了运行多个脚本的能力，例如:运行 Swiftlint 脚本和默认的 xcodebuild 测试脚本。

需要注意的是，虽然之前 xcodebuild 脚本是由 Travis 默认运行的，但现在我们需要在显式声明“script”后自己指定它(连同 Swiftlint，第 9-10 行)。

为了能够使用 Swiftlint，我们首先安装它。这可以在“install”声明中完成(第 1 行)。

# 3.使用 CocoaPods 为项目配置 Travis

到目前为止，我们告诉 Travis 构建 xcode_project(上面代码中的第 5 行)。然而，如果我们在项目中使用像 CocoaPods 这样的依赖管理器，它通常会创建一个新的 xcworkspace 文件，然后用于开发，而不是 xcodeproj 文件。

在这种情况下，我们还需要告诉 Travis 使用工作区文件。这可以这样做—

在上面的代码中，我们做了一些修改，以便能够使用 CocoaPods 和 Travis 构建我们的项目。

1.  我们将项目更改为工作区(第 8，13 行)。
2.  为项目安装 CocoaPods 和 pod(第 1-5 行)。

# 感谢您的阅读！

*请随时与我联系并联系，查看我们的项目，或者加入我们的开源社区:* [*LinkedIn*](https://www.linkedin.com/in/yugantar-jain-1a7820158/)*，*[*GitHub*](http://github.com/yugantarjain)*，* [*导师 iOS*](https://github.com/anitab-org/mentorship-ios) *，*[*AnitaB.org 社区*](https://anitab-org.zulipchat.com)