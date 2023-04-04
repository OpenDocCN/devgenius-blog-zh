# Getx 状态管理有什么新东西带来的波澜

> 原文：<https://blog.devgenius.io/whats-the-new-thing-bring-flutter-getx-state-management-1f8d2c28f3f4?source=collection_archive---------6----------------------->

## #这个图书馆其实就是 boom！

![](img/d762de57f9a60e318ea6a2d8837e0d9a.png)

法希姆·蒙塔希尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

今天我将讨论一个最流行的框架库 Getx，它是一个强大的状态管理框架。Getx 专注于更好的性能，最大限度地减少开发时间，并且易于使用。如果你和其他库比较，那么 Getx 是最好的帮助库，一旦你开始使用这个库，你就会被它迷住。

你会因为它的惊艳表现而爱上它。下面我会给你看一些例子。之后你会决定为什么 Getx 是最好的颤振库。

# Getx

他们声称 Getx 有三个基本原则生产力、性能和组织。

**现在我们来看看它是如何工作的。**

# **安装**

```
dependencies:
  get: ^4.3.8
```

只需添加一行你的包的 pubspec.yaml 和 pub get 即可。

现在来看代码。

```
**import** 'package:get/get.dart';
```

把这个输入到你的 dart 代码里就行了。

# GetMaterialApp

```
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return GetMaterialApp(); // instead of MaterialApp
  }
}
```

# GetBuilder

如果你了解供应商或消费者，那就差不多了。GetBuilder 的工作原理是初始化一个控制器，然后你可以在任何地方找到这个控制器，并实际操作。

```
**class** **Controller** **extends** **GetxController** {
  **var** count = 0;
  **void** increment() {
    count++;
    update();
  }
}
```

GetBuilder 将小部件包装到与方法的交互中，这就是为什么我们可以调用函数。当我们更新控制器中的方法时，小部件可以监听这些变化。第一次需要在 GetBuilder 中初始化控制器时，另一个 GetBuider 会自动共享第一个状态。看起来很有条理。这就是 GetBuilder 的工作方式。

# GetController

这使得我们可以避免 StatefulWidget，你只需要创建一个类，通过 GetxController 和扩展它，在这里保存所有的变量和方法。我们可以在这里保留所有的逻辑，这就是我们可以从视图中访问的原因。并使任何变量都可以观察到。obs”。

```
class Controller extends GetxController{
  var count = 0.obs;
  increment() => count++;
}
```

# 反应式状态管理器

```
**var** name = 'Jonatas Borges';
```

假设你有一个名字变量，你想每次都改变它。

```
**var** name = 'Jonatas Borges'.obs;
```

对于可观测的，只需要加上”。obs”到这一行的末尾。

```
Obx(() => Text("${controller.name}"));
```

当您想要显示值或任何更新时，您将需要这一行。那绝对好用。

# #路线管理

Getx 让路线管理变得尽可能简单。让我们来看看。

```
// Within the `FirstRoute` widget
onPressed: () {
  Navigator.push(
    context,
    MaterialPageRoute(builder: (context) => SecondRoute()),
  );
}
```

在我们用这个导航到新屏幕之前。

```
Get.to(NextScreen());
```

但现在你只需要使用这个，它是如此简单，你可以看到。要返回到上一个屏幕，只需如下所示。

```
Get.back();
```

# #依赖性管理

如果你在你的应用程序中使用依赖管理器，那么你就不需要重复它。您可以非常简单地使用它的依赖注入。

```
Controller controller = Get.put(Controller());
```

如果我们使用这个控制器，那么这个控制器将可用于这个类的所有路由子。你需要写这一行代码，它允许你检索相同的类，而不需要重写它。

Getx 带来了更多新的东西，我将在以后讨论。

# 最后的话

他们声称 GetX 对 flutter 来说是最强大的，实际上也是。它减少了你的开发时间，如果你是一个初学者，那么不需要担心，只要按照文档。我在这里得到了一个[](https://pub.dev/packages/get)**到 GetX 库的链接，所以去看看吧。**

**感谢您的阅读。**

**参赛:https://pub.dev/packages/get**