# Swagger API iOS 助手

> 原文：<https://blog.devgenius.io/swagger-api-ios-helper-b09da2c83c25?source=collection_archive---------1----------------------->

![](img/8e83fb2a686961948ca822c1f057339e.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Przemyslaw Marczynski](https://unsplash.com/@pemmax?utm_source=medium&utm_medium=referral) 拍摄的照片

Swagger 是一个很棒的工具，可以帮助开发人员在使用 web 服务时节省时间。API 规范对开发者和 QA 团队都有很大的帮助。

但是还有更多。客户端开发人员可以很容易地跳过开发过程的常规部分，不实现客户端-服务器通信，而是使用 swagger-codegen。

它适用于许多平台，但就我个人而言，我主要是一名 iOS 开发人员，我将分享我在日常工作中使用的一个小技巧。

假设我们有一个 messenger 应用程序，我们需要实现一个带有对话列表的屏幕。我将跳过向毒蛇模式致敬的部分，直接进入我们同意需要某种聊天服务的地方。

假设我们定义了一些协议

现在，我们想实现远程聊天提供商。它应该调用 API 并将 API 响应模型映射到应用层实体中。

您可能已经注意到了一个名为 SwaggerApiResponseHandler 的类。我想和你们分享的是一个简单的类，带有帮助方法。这将为您节省一些时间来避免代码重复。(尽管包含 DispatchGroup 内容的部分也必须重新编写，以便更好地实现。但到目前为止还不错。)

我们在这里所做的只是以我们认为最适合自己的方式展开 swagger api 响应。Swagger 生成的 API 返回一个可选成功响应对象和可选错误的元组。每次处理起来都不是那么方便，所以我想出了上面的处理程序。

您可以坚持闭包风格或者更方便(imho)的方式，即异步方法看起来像同步方法。

所以要使用它，你需要这样做:

这种方法的缺点是您需要自己处理调度队列。但这也是一个优势。您将能够链接方法并避免闭包地狱。

感谢您的宝贵时间！