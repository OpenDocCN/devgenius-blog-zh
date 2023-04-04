# swift——你可能知道但忘记的 6 件事

> 原文：<https://blog.devgenius.io/swift-6-things-you-might-know-but-you-forgot-45ea63c99ae1?source=collection_archive---------3----------------------->

![](img/ee3a67a440bb0b26c94b6beaca49939b.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=4162499)

你好，在这篇文章中，我想谈谈我在开发 Swift 应用程序时遇到的让我说“Evrica”的事情。这些小知识可能你已经知道了，但是很有可能你忘记了，因为其中一个不太常见。开始编码吧。

# 1.继承 Objc 中的 Swift 类

简单的回答是，**不可能**，在 Objc 中不允许继承 Swift 类。

如果你发现自己处于这种情况，你必须解决:

*   也要重写 Swift 的 Objc 类，推荐的方法；
*   如果 first solutions 不是一个选项，您可以提取需要被覆盖到协议中的方法，比如 floor:

# 2.JSON 序列化

目前在 Swift 中，我们有两个解析 json 的工具，`JSONDecoder/Encoder`和`JSONSerialization`。如果出现问题，第一个是抛出一个错误，这是非常方便和容易调试的。在`JSONSerialization`的情况下，事情在解析过程中没有那么清楚，如果有什么地方出错，很可能会导致难以调试的崩溃。幸运的是，api 为我们提供了一种在执行任何序列化之前验证数据的方法，方法是调用`isValidJSONObject`:

在上面的例子中，如果我们不首先验证地图，应用程序将崩溃，因为一个键不是`Hashable.`

# 3.获取文档目录路径

获取文档目录路径是一件非常简单的事情，但是由于某种原因，我发现我自己几乎每次想获取这个路径时都要进行谷歌搜索。问题是我知道获取这个路径的方法，包含一些关于“搜索路径”的内容，但是在这种情况下，自动完成对我没有帮助，所以我使用谷歌搜索来获取这个:

即使上面的解决方案行之有效，对我来说也很难记住。我可以告诉你，如果你想从`FileManage`本身访问这个路径，有一种更容易记住的不同方法，通过调用:

# 4.无限滚动视图

你可能会在你的 iOS 开发生活中遇到需要无限滚动视图的功能。“万维网”对这种无限滚动有很多解决方案，从非常复杂的到难以理解的。我发现的一个非常简单的解决方案是使用集合视图，并为`numberOfItemsInSection`返回一个大的数字，以给出无限的效果。

如果您想在一个方向上无限滚动视图，这种方法很有效。如果您想在两个方向上滚动，您必须做的唯一调整是在中间滚动集合视图。这里需要注意的是，如果您试图在中间移动，那么`Int.max`会崩溃，因此您可以定义一个最大的细胞数，例如十亿。

# 5.本地化资产图像

我见过很多本地化应用程序图标的方法，通过在图片名称前加前缀或后缀地区代码来硬编码图片名称。幸运的是，Xcode 将我们带到了这里，并提供了对图像本地化的全面支持，您只需要检查图像的所有语言，然后提供相应的资产，为了更好地理解，请参见下面的截图:

![](img/3542c9bf8a65390b19f0831f5182fe41.png)

# 6.客观标识符

如果你想打印一些对象的内存地址，一个非常简单的方法就是使用`ObjectIdentifier`，注意，顾名思义它只对对象有效，对 struct 或 primitive 数据类型无效。

事实上，`===`操作符的实际 swift 实现依赖于`ObjectIdentifier`，你可以在这里查看[:](https://github.com/apple/swift/blob/43ddb88f8aad419da6f4cba21d1f623895f50961/stdlib/public/core/Equatable.swift#L249-L259)

希望这篇文章对你有用，内容丰富，如果你有任何想法要补充，我很乐意。