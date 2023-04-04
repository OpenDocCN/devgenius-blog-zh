# 我的父母、OTT 和适配器设计模式

> 原文：<https://blog.devgenius.io/my-parents-ott-and-the-adapter-design-pattern-80d4c0e2c1c9?source=collection_archive---------9----------------------->

## 适配器设计模式接受一个接口，并使它与另一个接口兼容。

比方说，我们已经得到了源代码(一个接口及其子类)。我们的源代码接口向第三方库接口发出请求。这两个接口可以互相交流，并完美地同步工作。

![](img/8df33073234c737570cabcc493d6d48a.png)

兼容接口通过松散耦合进行交流

但是一个新的厂商走进来，写了一个新的第三方库(接口)。新的第三方库接口接受 specificRequest 而不是 generic 请求。

![](img/ec570b08a6ee0e9af6bd04466d2c3d37.png)

不兼容的接口:源代码无法调用 specificRequest

这使得两个接口不兼容。两个接口现在都不能通话。源代码不知道提出具体的请求，新的第三方库不知道如何接受一般的请求。

我们现在陷入了两个接口如何交互，如何实现兼容性的问题。这就是适配器出现的原因。适配器将位于两个接口之间。适配器将接受来自源代码的一般请求，然后将其转换为特定请求。

适配器像翻译器一样提供兼容性

![](img/7f4d003e1970659b70579cf19972d2f5.png)

适配器设计模式 UML

比方说，用户或客户机请求目标接口请求方法。从客户端到目标的此请求方法希望访问 specificRequest。但是目标接口又不知道如何访问 specificRequest。目标接口与适配器接口不兼容。目标接口不知道如何与适配器接口对话。目标接口不知道如何调用 specificRequest。

目标接口无法满足客户端的请求。

这就是适配器进入画面的时间。它将实现目标接口并覆盖请求方法，以便适配器可以访问 specificRequest。适配器将覆盖目标接口的请求来访问 specificRequest。

在适配器的帮助下，Adaptee 接口(及其子类)将适应目标。

![](img/580c92d812aab28d1df5077ded79ad63.png)

父母、OTT 和适配器设计模式

我的父母将充当客户，他们可以随时观看电视频道上的节目。他们可以简单地向目标 TVChannel 提出请求，并观看他们想要的任何节目。我的父母知道如何请求观看电视频道的节目，并在他们想要的任何时候访问它们。

```
**new NationalGeographic().watchShow(); new Discovery().watchShow()**
```

Parents 类可以简单地使 Discovery 和 National Geographic 类的对象调用 watchShow 方法。

但我的父母无法在智能电视上访问 OTT 应用程序。我父母不知道登录 OTT 然后看节目。我的父母不知道向 loginAndWatchShow 方法请求。

还有，TVChannel 和 OTT 的接口互不兼容。TVChannel 不知道怎么和 OTT 对话。TVChannel 不知道怎么把我父母的 watchShow 请求转换成 OTTs 的 loginAndWatchShow。

然后，OTTAdapter 会走进来，实现 TVChannel。OTTAdapter 会覆盖 TVChannel 的 watchShow，调用 OTT 的 loginAndWatchShow。(使两个接口兼容)

客户端和适配者从不交互。类父不与 OTT 接口交互。

```
Definition: The adapter pattern convert the interface(OTT) of a class(AmazonPrime) into another interface(Target TVChannel) clients expect. Adapter lets classes(Discovery and AmazonPrime) work together that couldn’t otherwise because of incompatible interfaces.Application: **Use the Adapter class when you want to use some existing class (AmazonPrime or HotStar), but its interface(OTT) isn’t compatible with the rest of your code(Target TVChannel).**Imagine that you’re creating a stock market monitoring app. The app downloads the stock data from multiple sources in XML format and then displays nice-looking charts and diagrams for the user. At some point, you decide to improve the app by integrating a smart 3rd-party analytics library. But there’s a catch: the analytics library only works with data in JSON format.
```

![](img/1c9bbb2473f1eb37acb79272116486ea.png)

[自由股票](https://unsplash.com/@freestocks?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

你在生活中有什么适配器设计模式的故事吗？请在评论中告诉我。下次见！