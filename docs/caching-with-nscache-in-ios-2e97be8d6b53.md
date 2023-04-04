# 在 iOS 中使用 NSCache 进行缓存

> 原文：<https://blog.devgenius.io/caching-with-nscache-in-ios-2e97be8d6b53?source=collection_archive---------1----------------------->

![](img/478312259c5ad8f82e22bca60eef6a40.png)

图书馆里的书架( [Flickr](https://www.flickr.com/photos/opengridscheduler/22468805072/in/photolist-AeuuWU-9L4DC-akFR3u-akFR6d-9tba2y-9t8aqZ-GvPgzi-2XUSwK-n3VAH-8ds4hQ-LRd69z-54AriK-8ew2F6-6goydB-6cFVL4-9uPVG8-rX7yXD-82iqbY-4h2pzN-PDrUTj-82ipB5-hW9GkX-ci8s7s-5Emj44-923VJQ-sTdb6-HRXqoR-4ZUyno-dvUBA7-bVpm9T-aVn2eF-7X3HF7-6tQvry-afnzip-afqnMW-dvNU6a-72DYa6-anxHwx-29gdmz9-zeT59-n3S1U-eem4U-iEvVHU-LYVZXx-C2vb6p-ghnoDc-9Dp34Y-5Y64MV-kb9aZB-25Gs167)

与 Swift 标准库(如`Dictionary`)中的集合相比，`NSCache`更适合缓存对象，因为当系统内存不足时，NSCache 会自动驱逐对象，这反过来使我们的应用程序本身能够在内存中保留更长时间。

NSCache

**ns cache 的局限性**

*   这是一个客观的 C 类
*   它可以存储类实例
*   键应该基于 NSObject(例如 NSString vs String，NSNumber vs Int)
*   缓存值没有现成的到期日

*在围绕 NSCache 编写了一个简单的代理类之后，您可以在 Swift 中最大限度地利用它。*

*通用自定义缓存*

*想法是能够使用任何引用或值类型作为符合 [Hashable](https://developer.apple.com/documentation/swift/hashable) 协议的键。*

*让我们从一个内部 CacheEntry 类开始，其中一个`Value`与一个可选的 expiryDate 属性驻留在一起。*

*页（page 的缩写）如果不想将 CacheEntry 定义为 CustomCache 的内部类，也可以在 CustomCache 上定义一个扩展来访问泛型值。*

*值容器类*

*接下来，我们将需要一个自定义的 NSObject 子类来托管我们的 hashable 键，以便能够将其用于 NSCache。如果您想只使用 String 作为缓存的键，可以跳过这一部分，在 insert 函数中将 String 转换为 NSString。*

*保存可散列密钥的 KeyWrapper。*

*我们的自定义类可以保存一个私有的 NSCache 对象用于内部访问。*

*我们可以定义所需的插入、删除和获取值函数。*

***用途***

***订阅***

*我们可以定义一个下标来更容易地访问缓存的值。*

*你可以从下面的[链接](https://gist.github.com/IronLeash/7d4a5583a85d60cb36d2e35e53427c42)中获得完整的类。*

***结论***

*使用 NSCache 而不是简单的字典带来的好处主要在内存管理部分。一旦你有了一个通用的自定义缓存类，你就可以将它用于任何本地缓存目的。*