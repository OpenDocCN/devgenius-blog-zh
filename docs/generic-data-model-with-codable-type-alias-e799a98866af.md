# 具有可编码类型别名的通用数据模型

> 原文：<https://blog.devgenius.io/generic-data-model-with-codable-type-alias-e799a98866af?source=collection_archive---------6----------------------->

![](img/8058317891aa5b22d5e6ccc3b421a802.png)

乔尔·菲利普在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

[可编码](https://developer.apple.com/documentation/swift/codable)是[可编码](https://developer.apple.com/documentation/swift/encodable)和[可解码](https://developer.apple.com/documentation/swift/decodable)协议的类型别名。它可以用来序列化和反序列化远程 web 服务数据或任何数据。让我们看一些代码。

先举个简单的例子。下面是**基准**结构。

让我们看看**数据**的编码和解码示例。

让我们看一个真实世界的例子。使用泛型，我们可以创建一个代表根 Json 响应的 **BaseResponse** 。

这个 **BaseResponse** 类具有根 Json 对象响应表示。泛型类型参数 **T** 必须符合**可编码** ( **可编码**或**可解码**协议)。

# 使用

下面是一个 **ExampleResponse** 类。

**示例响应**子类**基本响应**传递**数据**作为通用参数。

这样我们可以将任何符合**可编码**或**可解码**协议的类型 **T** 泛型参数传递给 **BaseResponse** 。这使得代码可重用且更易于管理。

感谢您的阅读。如果你喜欢这个帖子，请给一些掌声。