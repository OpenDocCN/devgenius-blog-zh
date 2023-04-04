# 如何减轻在 Swift 中编写单元测试的痛苦

> 原文：<https://blog.devgenius.io/how-to-ease-your-pain-writing-unit-tests-in-swift-fcf84bab765b?source=collection_archive---------11----------------------->

![](img/9b00f9401b3dfbc26210bd0e897278e0.png)

图片由[freeimages.com](https://www.freeimages.com/photo/busy-businessman-2-1237011)提供

我知道开发人员的一个假设，他们意识到编写单元测试对于中大型项目是至关重要的，尽管他们认为这是一个非常烦人和耗时的过程。在这里，我想谈谈一些技巧/工具，它们不仅能帮助你减少编写测试的时间，还能提高可读性和可伸缩性。

# 自动生成模拟

当您开始编写测试但不知道自动模拟生成时，最烦人的部分是手动编写它们。在最坏的情况下，您将陷入创建无限数量的类/方法/变量并按顺序支持它们的困境。对于我们的幸福，有一些美好的工具——资源。

互联网上有很多不同的 sourcery 模板实现。你可以从 Sourcery Github 本身的自述中提到的[这个](https://github.com/krzysztofzablocki/Sourcery#quick-mocking-intro--getting-started-video)开始。主要思想是，您只需标记需要的协议和您想要模拟的类，每次触发应用程序构建或 bash 命令时，模拟都会自动创建和更新。

```
//sourcery: AutoMockable
protocol MyServiceProtocol { 
    func fetchItems() 
}
```

# 使用样本函数

另一个问题是模仿一些数据，尤其是你的类型的实例。对于像 dto 这样的类，你可以创建 JSONs 并使用 ***JSONDecoder*** 解析它们:

然而，你可能不希望为你的所有类型做可解码的一致性。在这种情况下，有一些很好的方法——采样器。这个想法非常简单:您为想要创建的每个字段创建一些带有默认参数的静态函数。如果您只需要一个不同的字段值，您只需传递这个字段:

这里有一个库，允许你自动创建这个采样器！

# 快速/灵活的用法

在 swift 中有两个帮助你编写单元测试的顶级框架— [Quick](https://github.com/Quick/Quick) 和 [Nimble](https://github.com/Quick/Nimble) 与默认的 XCTest 框架相比，它们有很多好处。我认为最重要的是:

*   声明性测试的结构—快速
*   清晰可读的断言函数——敏捷

我会给你一些用 XCTest 和 Quick/Nimble 编写的简短例子，这样你就可以自己比较了。

# 结构示例

快速/敏捷:

XCTest:

# 写入异步断言

快速/敏捷:

XCTest:

# 快照测试

在 iOS 项目中，我们通常根本不测试视图层，否则它看起来会很奇怪。最合理的情况是测试正确显示所有内容的视图。最好的方法是编写快照测试。它基本上是拍摄你的视图，并将其与你提供的完美参考的每个像素进行比较。有很多框架可以帮助你编写快照测试，我将只列出其中最流行的:

*   GitHub-point freeco/swift-snapshot-testing:📸令人愉快的快速快照测试。
*   [GitHub-Uber/iOS-Snapshot-test-case:iOS 的 Snapshot view 单元测试](https://github.com/uber/ios-snapshot-test-case)

# 摘要

尽管安装和学习上面提到的工具和方法肯定会花一些时间，但您最终会习惯它，编写更好的测试代码，并可能编写自己的助手来使这个过程变得更容易和有趣😎