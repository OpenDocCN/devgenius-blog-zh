# 在 Flutter 中改变状态栏和导航栏颜色的不同方法(iOS 和 Android)

> 原文：<https://blog.devgenius.io/different-ways-to-change-the-status-bar-and-navigation-bar-color-ios-and-android-in-flutter-a786e098f573?source=collection_archive---------0----------------------->

![](img/1117bb8fcada59f11c3fbc8b73862984.png)

Flutter 自发布以来一直很受欢迎，许多开发人员和客户都很喜欢 flutter 的工作方式。iOS 和 android 的一个代码库，热重启功能使 flutter 易于开发，像本机应用程序一样的性能是 flutters 的几个功能。

Flutter 受欢迎的另一点是启动容易。完全没有以前的移动开发知识，具有基本 OOP(面向对象编程)概念的新人可以在 flutter 中开始开发，并使他们梦想的应用程序成真。

但是由于 flutter 的冗长，对于初学者来说，很少有什么事情是压倒性的。所以今天我们来看看在 flutter 社区中一个简单却被问得很多的问题，即如何在 flutter 中改变状态栏和导航栏的颜色。

![](img/0319f48e16d9e8e9deecbd3b6f5adb0a.png)

导航栏

![](img/8ba48eb56001c3e4dbe7eeadcbf3bc2f.png)

状态栏

## 1.使用 app bar(iOS 和 Android)

这里，在亮度参数中，Brightness.light 使状态栏图标变暗，而 Brightness.dart 使状态栏图标变白。

## 2.使用 AnnotatedRegion 小部件(iOS 和 Android)

当你没有使用 AppBar 的时候，你可以使用 AnnotatedRegion 来改变状态栏的颜色，但是如果你有 AppBar，这个方法就不起作用了(iOS 和 Android 都是)

## 3.使用软件包( [flutter_statusbarcolor](https://pub.dev/packages/flutter_statusbarcolor#flutter_statusbarcolor)

另一种改变颜色的方法，如果你不介意使用软件包， [flutter_statusbarcolor](https://pub.dev/packages/flutter_statusbarcolor#flutter_statusbarcolor) 是一个很好的方法。

只需将这个包添加到您的 pubspec.yaml 文件中，导入它并在您想要的任何地方使用它。

## 4.使用系统浏览器

用这种方法，你可以改变导航栏颜色，状态栏颜色，状态栏亮度，状态栏图标亮度，任何更多的自定义选项。

## 推荐的解决方案:

我个人使用数字 4，因为它很容易实现，但你可以尝试其他方法，找到最适合你的选择。