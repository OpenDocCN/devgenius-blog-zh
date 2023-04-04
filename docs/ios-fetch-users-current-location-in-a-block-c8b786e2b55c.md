# iOS —获取用户在块中的当前位置

> 原文：<https://blog.devgenius.io/ios-fetch-users-current-location-in-a-block-c8b786e2b55c?source=collection_archive---------17----------------------->

![](img/55bc97af76b60e2241f8a158a77fea38.png)

照片由[亨利·佩克斯](https://unsplash.com/@hjkp?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在 iOS 应用程序中，我们经常需要获取用户的当前位置。

在某些情况下，我们陷入了必须检索用户的位置才能继续前进的境地。不幸的是，您将最终编写 CLLocationManager 委托和回调。

在本文中，我们将看到如何构建一个类来获取用户的当前位置，并以块的形式返回。

首先创建一个单例类。

在这个 singleton 类中，我们为 completionHandler 创建了一个变量，它将返回用户的当前位置。

接下来创建一个函数来配置位置管理器:

该功能将启动位置管理器。

接下来实现位置管理器委托以在回调中返回位置。

要获取用户的当前位置，请按如下方式调用: