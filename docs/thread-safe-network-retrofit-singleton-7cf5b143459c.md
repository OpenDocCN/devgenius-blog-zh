# 线程安全网络(翻新)单例

> 原文：<https://blog.devgenius.io/thread-safe-network-retrofit-singleton-7cf5b143459c?source=collection_archive---------3----------------------->

![](img/b98583eb18eb224b9fb72bc551553974.png)

尼科·纳泽尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

用于与远程服务通信的网络对象应该在应用程序的生命周期中创建一次，然后重用。它应该是线程安全的，以便在没有任何问题(例如，内存干扰、死锁、活锁等)的情况下实现多线程访问。)这可能在多线程环境中出现。

代码中应用了双重检查锁定和延迟初始化。翻新的日志记录被配置为在调试生成期间激活。连接、读取和写入超时是自定义的。

下面给出了 Kotlin 的实现。

尽管看起来没问题，但还是有问题。在 Java 1.4 及更早版本下，易变修饰符[被破坏](https://en.wikipedia.org/wiki/Double-checked_locking#Usage_in_Java)。所以让我们做得更好。

上面的例子使用了 [**按需初始化保持器**](https://en.wikipedia.org/wiki/Initialization-on-demand_holder_idiom) 模式。这对于在所有 Java 版本上使用都是安全的。

让我们为科特林检查更好的版本。

如果你喜欢这个帖子，请给一些掌声。感谢阅读。