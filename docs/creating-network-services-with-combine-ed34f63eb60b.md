# 使用 Combine 创建网络服务

> 原文：<https://blog.devgenius.io/creating-network-services-with-combine-ed34f63eb60b?source=collection_archive---------5----------------------->

![](img/4fe167b6fd860ccdb4cb97961342802c.png)

照片由[法比奥·布拉克特](https://unsplash.com/@bracht?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

[**组合框架**](https://developer.apple.com/documentation/combine) 可以用来在一个 iOS 应用中构建健壮的、可重用的网络服务。在这篇文章中，我将演示如何用**结合**和[创建一个网络服务。](https://github.com/Alamofire/Alamofire)

首先让我们将 **Alamofire** 添加到 **Podfile** 中。

现在让我们检查一下核心服务结构。

**请求**方法将符合**可编码** ( **可编码**或**可解码**协议)的类型 **T** 和符合**错误**协议的类型 **E** 定义为类型参数。它返回一个[**Future**](https://developer.apple.com/documentation/combine/future)**，它将类型 **T** 作为**输出**，类型 **E** 作为**失败。****

**下面是**示例服务**结构。**

****ExampleService** 定义了一个 **getData** 方法，该方法将**参数**字典作为其参数，并返回一个 **Future < EditionResponse，Error >** 。它调用 **CoreService.request** 传递 **HTTPMethod.get** ，这使得该请求成为 **HTTP GET** 请求。**

# **使用**

**下面是**示例**结构。**

****getData** 方法取参数字典传递给 **EditionService.getData** 。 **subscribe** 方法被指定使用全局队列，该队列使用后台线程。 **receive** 方法被指定使用使用主线程的主队列。**接收器**被调用，以观察从**发布器**接收的值。**接收完成**观察完成时执行关闭。在这里检查完成是错误还是成功。**接收值**当从**发布者**处接收到一个值时，闭包执行。最后**存储**方法来保存由**接收器**方法返回的 **AnyCancellable** 。**

****getData: Example()的示例调用。getData(["format" : "audio "，" type" : "versebyverse"])****

**如果你喜欢这个帖子，请给一些掌声。感谢您的阅读。**