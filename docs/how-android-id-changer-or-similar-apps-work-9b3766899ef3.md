# Android ID changer 或类似的应用程序是如何工作的？

> 原文：<https://blog.devgenius.io/how-android-id-changer-or-similar-apps-work-9b3766899ef3?source=collection_archive---------0----------------------->

![](img/554c3da8e1b03e6a0ca86bd5791ee6fc.png)

米克·豪普特在 [Unsplash](https://unsplash.com/collections/2416700/how-many?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

最近我在探索找出一个唯一的设备标识符来跟踪唯一用户的数量。在我的研究中，我发现其中一个合格的候选人是 Android id，它以前(在 Android Oreo 之前)被称为设备 ID。有趣的是，谷歌不仅将其重命名为 Android_ID，还改变了它的行为。

> 虽然在本文中讨论这种差异没有太大关系，但只是为了让您了解这个主题的本质，当第三方应用程序请求 Android_ID 时，它不再是每个设备唯一的，而是每个应用程序、每个用户唯一的。假设由两个不同的应用程序开发人员 A 和 B 开发的两个应用程序请求它，将返回两个不同的 Android _ IDs。更令人困惑的是，如果任何预安装或系统应用程序或系统组件请求它，它就是唯一的。

好吧…回到主题，在我的探索过程中，我也发现有几十个名为 Android ID changer，Device ID 等的应用程序…嗯…我的好奇心越来越强，我一直在唠叨我自己如何才能在不成为 Android 系统的一部分的情况下修改这些细节。我进一步安装了几个应用程序，并验证了行为，令我惊讶的是，没有一个应用程序工作，我可以看到类似 ***Root 访问权限未被授予的错误消息。这意味着所有这些应用程序只有在用户已经给他们的手机加了根的情况下才能工作。***

我更进一步，看看这是如何工作的，如果手机是根。这一次……砰……ID 变了，应用程序运行得很完美。那么现在这里发生了什么，事情是如何工作的，只是根权限，所有这些应用程序仍然不是系统的一部分。这是我的发现。

在深入研究这些发现之前，先简单了解一下 Android ID 在系统内部是如何存储的。

Android ID 存储在 ***设置提供者*** 包下的 ***安全*** 表中。一个人应该有***WRITE _ SECURE _ SETTINGS***权限来修改这个表，但是任何应用程序都可以从中获取数据。除此之外，正如我前面提到的，每个应用程序在获取 Android_ID 时都会有一个不同的 Android _ ID。为了支持这种行为，设置提供程序还维护了一个不同的 SSAID 表，其中包含应用程序列表及其 Android_ID 详细信息。每当从 playstore 安装或卸载新应用程序时，都会更新 SSAID 表。在内部，所有这些数据都存储在单独的 xml 文件中。

回到我的发现，有两种方法可以改变 ID

> 修改存储在安全表中的 Android_ID。
> 
> 分别为每个应用程序修改存储在 SSAID 表中的 Android_ID。

所以所有这些应用程序做的第一步是使用下面的 adb shell 命令静默获取 WRITE_SECURE_SETTINGS 权限。

```
adb shell pm grant com.somecomp.someapp android.permission.WRITE_SECURE_SETTINGS
```

> 上面的命令不要求用户授予权限，但默默地批准了这个请求，因为这样的命令只能由系统运行。希望你能感觉到这里的危险和 root 访问的影响。

一旦获得该权限，就可以使用下面的代码更新该表

```
Settings.Secure.*putString*(getContentResolver(),Settings.Secure.***ANDROID_ID***,<Any_Value>);
```

另一种方法是直接修改底层 xml 文件。这些 xml 文件存储在***/data/system/users/0/***下，这里相关的文件有***settings _ secure . XML***和***settings _ ssaid . XML***

最后的想法

我可以弄清楚这些东西是如何在引擎盖下工作的，但我还没有弄清楚谷歌在 Android Oreo 上改变 Android_ID 行为的动机。希望有一天我会明白。如果你已经知道这个变化，请在这里留言。