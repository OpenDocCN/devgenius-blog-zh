# 了解 Android 应用沙盒。

> 原文：<https://blog.devgenius.io/understanding-android-application-sandbox-a2195c7ddac1?source=collection_archive---------5----------------------->

![](img/d4d2f4abcc7f6866afc7ba8f1deaf87f.png)

照片由[弗兰克](https://unsplash.com/@franckinjapan?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

> 应用程序沙箱是一种安全机制，通过这种机制，单个 android 应用程序在自己的“空间”中运行，如果没有适当的权限，就不能与其他已安装的应用程序或 Android 操作系统进行交互。

现在，让我们稍微详细地了解一下。

# 应用程序沙箱的需求

比如说，你已经在你的设备上安装了 Chrome 和 Firefox。如果没有应用程序沙盒，Chrome 将可以访问设备上的所有文件，它可以修改或删除 Firefox 的一些重要文件，使其崩溃/故障。你会发现它是 Firefox 中的一个 bug/问题，然后卸载它，继续使用 Chrome。
**Chrome 争霸还在继续..😃玩笑归玩笑，应用程序沙箱防止应用程序对其他应用程序或操作系统本身做任何恶意的事情。**

# 背景

由于 Android 构建在 [*Linux 内核*](https://www.redhat.com/en/topics/linux/what-is-the-linux-kernel) 之上，因此它使用了 Linux 内核的以下安全特性，即:

*   **过程隔离**
*   **基于用户的权限模型**
*   **进程间通信(IPC)**

应用程序沙箱是这些功能的组合，用于限制应用程序对其他应用程序和操作系统资源的访问。

> Android 为每个应用程序分配一个唯一的 id，并在单独的进程中运行。就 Linux 而言，每个运行的应用程序都是唯一的用户，可以访问磁盘上的个人空间。没有必要的许可，它不能访问另一个用户(应用程序)或与之交互。
> 这可以控制每个应用程序的访问，防止任何恶意应用程序与其他应用程序或操作系统进行交互。

由于这种应用程序沙箱是在内核级完成的，因此可以确保每个应用程序都在一个隔离的环境(进程)中运行，而不管任何与开发相关的因素，如 SDK、API 或编程语言。

# 结论

这是一个小的，简单的，简明的 Android 应用沙箱解释，如果你想阅读更多的术语解释(详情😏)
去 [*这里*](https://source.android.com/security/app-sandbox) 看安卓官方文档。

如果这对你有用，请鼓掌。

干杯！！！