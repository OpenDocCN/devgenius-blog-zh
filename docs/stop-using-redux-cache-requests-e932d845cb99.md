# 停止使用 Redux，缓存请求

> 原文：<https://blog.devgenius.io/stop-using-redux-cache-requests-e932d845cb99?source=collection_archive---------26----------------------->

在本文中，我不想谈论新的状态管理库，也不想比较 Redux 和 Context API。我只想说，在某些情况下，你不需要 Redux，你可以通过其他解决方案来处理它。

![](img/8be741c2c7bc6810a742d4ffc8cdaf39.png)

约翰·马特丘克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

想象一下，我们有一个 PWA，它是一个在线商店。通常，我们在产品之间导航并检查它们。当你喜欢一个产品时，你可能会检查它很多次，如果每次都要等待从服务器获取产品数据，这将是一个糟糕的体验。我们有两个解决方案来获得更好的体验。

**一个。使用 Redux(不推荐)**

我们可以使用 Redux，有一个状态，在这个状态下推送产品数据。但是当你开发了一个大规模的应用程序，这不是一个好的解决方案。

**两条。缓存请求(推荐)**

缓存请求。在可以缓存请求之前，为什么要使用 Redux？

我们可以使用缓存存储来存储请求数据，当一个 API 请求被调用时，我们首先在缓存存储中寻找响应，如果我们没有响应，现在我们需要调用 API 请求。

我们有许多方法来实现这个特性，我将向您展示其中的一些。

*   您可以按如下方式实现它:

或者如果你使用的是 [axios](https://www.npmjs.com/package/axios) ，你也可以使用 [axios-extensions](https://www.npmjs.com/package/axios-extensions) 或者 [axios-cache-adaptor](https://www.npmjs.com/package/axios-cache-adapter) 包。