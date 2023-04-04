# iOS 而非通知中心中的多播授权

> 原文：<https://blog.devgenius.io/multicast-delegation-in-ios-instead-of-notification-center-c6433401ab51?source=collection_archive---------9----------------------->

![](img/30cc8232830fc97d8b67973670c522b2.png)

**广播发射塔** [**Flickr**](https://www.flickr.com/photos/benjaminedwards/2541360913/)

NotificationCenter 是[观察者设计模式](https://en.wikipedia.org/wiki/Observer_pattern)的典型例子。这是一个“酷”且简单的机制，可以将一个类的信息广播给所有注册的听众。您可以为通知定义一个关键字，并通过其唯一的名称将观察者添加到通知中。您在 observer 类中实现了一个在接收到通知时触发的处理函数。发送者通过 N [通知中心](https://developer.apple.com/documentation/foundation/notificationcenter)广播其信息，N[通知中心](https://developer.apple.com/documentation/foundation/notificationcenter)负责跟踪各个通知的收听者和观察者。

**理论上的利弊**

*   交互对象(发送者和观察者)之间的松耦合设计
*   它存档了一个 1:M 通信频道
*   无需更改发件人或主题即可添加新的观察者。
*   观察员可以随时添加/删除。

NotificationCenter 工作得很好，但是在干净的代码和可伸缩性方面，随着项目规模的变大，它带来了许多倒退

**我遇到的问题**

1.  任何对象都可以发出通知，并且任何对象都可以将自己注册为任何通知的侦听器。当通知发出时，很难调试或跟踪通信流。当您第一次在新项目中看到通知时。您可能会搜索它的唯一名称来查看所有的观察者/发送者，并试图弄清楚每个发送者是如何推送通知的，以及它在每个侦听器端是如何处理的。
2.  每个通知都可以发送 [userInfo](https://developer.apple.com/documentation/foundation/nsnotification/1409222-userinfo) 字典来传递附加信息给它的监听器。相同的通知可以通过不同的 userInfo 字典发送，我祝你通过字典键获取值一切顺利。当你在团队中与多个 iOS 开发者一起工作时，这甚至会是一个更大的难题。:)
3.  当通知被发布时，通知中心遍历注册的观察者，并在发布通知的线程上调用相应的处理函数。不久，如果你在后台线程上发布通知，观察者处理程序也将在同一后台线程上被调用。这很公平，但是开发人员应该注意在需要更新 UI 的后台线程上调用的处理程序，反之亦然，当在需要进行大量计算的主线程上调用处理程序时。

# 多播委托

委托是一种强大的设计模式，其中一个对象代表另一个对象。在传统模式中，委托对象保留对另一个委托对象的引用，并通过某些协议方法向该对象发送消息。协议定义以结构化的方式描述了委托类和委托类之间的公共语言。这种设计模式在现代 iOS 应用中几乎随处可见。(UITableViewDelegate，UITableViewDatasource，UIPickerViewDelegate)

就通信而言，传统的委托可以应用于任何一对一的通信，因为委托对象持有单个委托引用。

在组播委托方法中，委托对象可以持有多个符合特定协议的委托实例。因为委托类可以一次通知多个代理，所以通信通道不局限于 1:1，而是变成了 1:N，就像 NotificationCenter 中一样。由于通信交换以结构化的方式在定义的协议方法上进行，这种方法消除了上面提到的 NotificationCenter 的前两个缺陷。

委托对象需要存储多个委托实例，并且它们需要作为弱引用来保存，以避免保留循环。在我的第一次多播委托尝试中，我使用以下方法将委托实例存储在弱包装器中，以打破 swift 数组的强引用持有。

[如何在 swift 数组中存储弱引用？](https://www.objc.io/blog/2017/12/28/weak-arrays/)

后来，我发现了一种更好、更干净的方法，Vadim Bulamin 在他关于[多播代理](https://www.vadimbulavin.com/multicast-delegate/)的博文中解释了这种方法，它使用 [NSHashTable](https://developer.apple.com/documentation/foundation/nshashtable) 来存储代理对象。

具有添加、移除和调用方法的通用多播委托类。

AuthenticationManager 类，保存具有 AuthenticationManagerDelegate 类型参数的 multicastDelegate 对象

如第 15 行所示，在注销的情况下将调用所有注册的代理，而在第 21 行中，所有的代理都将为登录事件调用。

要注册和注销 AuthenticationManager 的代理，请执行以下操作:

如果监听器没有实现 AuthenticationManagerDelegate 协议中定义的方法，编译器将会报错。因为 multicastDelegate 通过弱引用访问它的侦听器，所以任何被释放的侦听器都不会被 invoke 方法通知。因此，如果监听器对多播委托对象提供的信息不再感兴趣，我只调用 *remove* 方法。

**结论**

我个人只在我的项目中为像 uiapplicationwilletenterforegroundnotification 或 UIKeyboardDidShowNotification 这样的 iOS 驱动事件使用 NotificationCenter。

如上例所示，由于委托类的单例性质，在应用程序的整个生命周期中，可以将单个代理添加到委托 AuthenticationManager 类中。如果 sender/observable 被释放，我们将丢失所有已注册的委托引用，并且需要在初始化 sender 的新实例时重新注册它们。

需要为发送者创建一个单独的类可能会被视为这种方法的缺陷。我将很快写一篇新帖来解释这种方法的扩展版本，它可以用于任何类型的发送者类。

***P.S:*** 对于针对 13.0 之前 iOS 版本的项目，值得考虑异步事件处理框架[结合苹果 19 年在 WWDC 推出的](http://Customize handling of asynchronous events by combining event-processing operators.)。Combine 有广泛的应用，它将简化开发者处理通知、委托和回调的方式。这将是 iOS 中反应式编程的新标准，在此之前，这种编程是在第三方框架上完成的。