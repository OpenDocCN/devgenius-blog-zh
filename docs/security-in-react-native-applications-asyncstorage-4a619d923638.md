# React 本机应用程序中的安全性—异步存储

> 原文：<https://blog.devgenius.io/security-in-react-native-applications-asyncstorage-4a619d923638?source=collection_archive---------7----------------------->

![](img/9d8349e0cd4a801492e2886d9d3ed006.png)

来源:https://www.pexels.com/@fotios-photos

大多数程序员发现很明显不要在代码中存储敏感的 API 键或环境变量。从应用捆绑包中获得这样的密钥需要几秒钟。然而，人们往往会忘记，这不是唯一的地方，这应该得到保护。

想象开发一个下载和存储数据的应用程序。它可能是观看过的电影列表或旧的购物清单，但如果应用程序获取并保存医疗数据或信用卡信息会怎样？

# 异步存储的局限性

根据[官方 RN 文档](https://reactnative.dev/docs/security#async-storage)，AsyncStorage 是一个异步且未加密的键值存储。因为它是未加密的，所以在 AsyncStorage 中保存的任何内容都不应该被认为是安全的。

> **Android:** AsyncStorage 使用数据库保存数据(RocksDB 或 SQLite)。
> 
> **iOS:** AsyncStorage 在 UserDefaults 中存储小值，在文件中存储大值。

在这一点上，有些人可能会问—这两个平台上的潜在威胁是什么？

在 Android 上，没有 root 权限是无法读取应用数据的。坏消息是，人们可以很容易地找到大多数可用的设备，然后读取数据。

我们在 iOS 上持久化数据的方式更糟糕。AsyncStorage 不允许指定哪些值属于用户默认值，哪些值属于文件。这并不幸运，因为存储在*文档*或*库/缓存*中的数据是加密的，但用户默认值并未加密，它们很容易被访问。此外，存储在 *Documents* 目录下的任何内容都可能被 iCloud 同步，因此在重新安装应用程序时，我们的用户可能会获得旧数据。

# 有没有办法让异步存储更安全？

Android 平台的一个建议解决方案是数据库中的数据持久性，通过 Keystore 系统进行额外的加密。不幸的是，这需要在本机端添加一些代码，更不幸的是，没有以包的形式存在这样的解决方案。

在某些情况下，[react-native-sensitive-info](https://github.com/mCodex/react-native-sensitive-info)库应该可以完成这个任务。它将数据存储在 SharedPreferences 中。SharedPreferences 的主要目的是存储未嵌套的键值数据，因此请仅使用它来以这种格式持久存储数据。否则，它可能会严重影响应用程序的性能。

另一件可能有助于提高整体安全级别的事情是升级 minSdkVersion。不能将密钥库用于运行版本低于 23 的设备。实际上，这意味着不再支持运行在低于 6.0 版本的 Android 上的设备。

对于 iOS 应用程序来说，事情变得越来越复杂。钥匙串服务应该可以处理少量的数据，而且有各种关于内存不足的错误报告。修补 AsyncStorage 库并设置更高的保护级别，同时添加允许在*库/缓存*目录下自动保存文件的部分，可以在许多用例中有所帮助。

# 结论

AsyncStorage 不是存储敏感数据的最佳位置，在实施任何与 AsyncStorage 相关的任务之前，请仔细考虑潜在的风险。每个解决方案都有额外的成本，因此在开始修补 AsyncStorage 之前，最好先退一步检查一下，在此用例中是否不可避免地要在 AsyncStorage 中保存敏感数据。