# 探索苹果 iOS 14 新的 Swift 日志 API

> 原文：<https://blog.devgenius.io/explore-apples-new-swift-logging-api-for-ios-14-7542d045bb6?source=collection_archive---------5----------------------->

## 了解如何使用 WWDC 2020 大会上推出的新 API 在您的 iOS 应用程序中记录事件和错误

在 WWDC 2020 上，苹果推出了 iOS 14 的新 Swift 日志 API。这篇文章总结了如何利用这个新的 API 来调试 iOS 应用程序中的行为。

![](img/3c4dd022d4880ba6f6bbc7320fb7a504.png)

我在听，告诉我更多关于这个新日志系统的事！

# 基础

当谈到在 iOS 应用程序中记录消息时，首先想到的 API 可能是`print`或`NSLog`。或者也许你正在使用第三方库，如`CocoaLumberjack`、`Willow`或`SwiftyBeaver`。

然而，在 iOS 10 中，苹果推出了一个新的 iOS 标准，用于使用`OSLog`记录信息，提供了一种高效的记录信息的方式。

在今年的 WWDC 2020 上，这个 API 变得——你可以说——更加迅捷。它提供了不同的日志级别、类别、持久性、格式化、高性能和隐私特性。值得去看看。

让我们看一个例子。

```
import os let logger = Logger(subsystem: "com.tanaschita.funapp", category: "funnyanimals") 
let videoName = "The turtle" logger.log("Starting video with title \(videoName, privacy: .public)")
```

`subsystem`参数通常是应用程序的包标识符。`category`参数可用于构建应用程序不同部分的日志。您可以记录符合`CustomStringConvertable`的任何类型。

运行应用程序时，日志输出会出现在 Xcode 的控制台中。连接设备后，您也可以使用 Mac 上“实用工具”文件夹中的 Console.app。

# 日志记录级别

API 提供了五个日志级别:

*   `Debug`
*   `Info`
*   `Notice`(默认)
*   `Error`
*   `Fault`

`Logger`实例为这些级别提供了合适的方法，如`debug`、`info`等。例如，要记录应用程序中的 API 错误，您可以执行以下操作:

```
logger.error("Missing field \(field) in response from \(endpoint).")
```

# 坚持

日志被存档，以便以后从设备中检索。若要浏览设备上的持续信息，请将设备连接到 Mac，并打开 Mac 上“实用工具”文件夹中预装的 Console.app。

执行后只能检索持久化的日志。日志消息是否持久取决于日志级别。`Notice`、`Error`和`Fault`消息持续存在。

# 表演

根据苹果公司的说法，日志系统非常具有性能，所以你可以在你的应用程序中广泛使用它，而不会降低它的速度。与`print`不同，日志消息没有完全转换成字符串表示。相反，编译器和日志库一起工作来产生日志消息的优化表示。

# 隐私

默认情况下，日志中的非数字值被配置为私有，因此它们在消息中是隐藏的。这可以确保在发货后，日志不会显示任何个人信息。所以在使用时:

```
logger.log("Logging in user with email \(emailAddress)")
```

上述语句将产生以下日志消息:

```
Logging in user with email <private>
```

如果您记录的信息不应被隐藏，您必须将其显式标记为公开:

```
logger.log("Opening funny video \(videoId, privacy: .public)")
```

# 结论

在将 API 与提到的第三方库进行比较时，我个人缺少的一点是从`Logger`实例中检索所有消息的能力，例如能够直接在应用程序中显示日志。

尽管如此，新的 API 是一个有前途的、强大的日志解决方案。

*最初发表于*[T5【https://tanaschita.com】](https://tanaschita.com/posts/20200705-explore-apples-recommended-swift-logging-api-wwdc-2020/)*。*