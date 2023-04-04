# 应用组件:Android 应用的入口点

> 原文：<https://blog.devgenius.io/app-components-entry-points-for-android-applications-f3d0b0294af7?source=collection_archive---------5----------------------->

![](img/a821a1d192de587e516103b9428a91d1.png)

斯蒂芬·弗兰克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

这个故事是“Android 开发者综合指南”系列的第一部分。

首先，Android 应用程序主要是用 Kotlin、Java 和 C++编写。Android SDK 将你的代码连同数据和资源文件编译成一个 APK 文件，即和 *Android 包。*这个 APK 文件可以通过多种方式分发，如 Google Playstore、第三方应用市场、文件共享服务等。这一部分介绍了 Android 应用程序的入口点。

# 应用组件

应用组件是系统或用户进入应用的入口点。

有四种不同类型的应用程序组件:

*   活动
*   服务
*   广播接收机
*   内容提供商

让我们来看看它们各自的功能:

## 活动

一个*活动*是与用户交互的入口点。它代表具有用户界面(UI)的单个屏幕。活动为用户提供了与应用程序交互的界面，即查看屏幕上的内容并执行操作。

## 服务

一个*服务*是一个通用的入口点，用于出于各种原因保持应用程序在后台运行。服务在后台执行操作，即后台同步、处理通知、播放音乐。服务没有用户界面。

## 广播接收机

*广播接收器*是一个组件，使系统能够在常规用户流之外将事件传递给应用程序，允许应用程序响应系统范围的广播公告。这方面的一个例子是应用程序显示某些事件的通知，如警报、提醒、应用程序通知等。与服务一样，广播接收器不显示任何用户界面。但是，他们可以创建状态栏通知，作为其他组件的网关。当广播事件发生时，这些通知会提醒用户。

## 内容提供商

一个*内容提供者*根据请求从一个应用程序向其他应用程序提供数据。内容提供商可以以不同的格式存储数据，如文件、SQLite 数据库，甚至可以通过网络获取数据。如果内容提供商允许，外部应用程序可以查询或修改数据。

从上述组件中，可以使用*意图激活活动、服务和广播接收器。*意图是在运行时将单个组件相互绑定的异步消息。

## 我们如何激活它们？

*   可以通过向 *startActivity()* 或*startActivityForResult()*方法传递一个*意图*来启动活动。
*   在 Android 5.0 和更高版本中，您使用 *JobScheduler* 类来调度操作或服务。对于早期的 Android 版本，你可以通过将一个*意图*传递给 *startService()* 来启动服务，或者通过将一个*意图*传递给 *bindService()来绑定服务。*
*   您可以通过将*意图*传递给*send broadcast()*或 *sendStickyBroadcast()等方法来启动广播。*
*   您可以通过在 *ContentResolver()* 上调用 *query()* 来执行对内容提供商的查询。

参考链接:

*   [developer.android.com](https://developer.android.com/guide/components/fundamentals#Components)
*   [tutorialspoint.com](https://www.tutorialspoint.com/android/android_content_providers.htm)