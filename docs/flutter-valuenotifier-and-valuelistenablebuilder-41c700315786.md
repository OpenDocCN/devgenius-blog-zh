# Flutter 中的 ValueNotifier 和 ValueListenableBuilder

> 原文：<https://blog.devgenius.io/flutter-valuenotifier-and-valuelistenablebuilder-41c700315786?source=collection_archive---------0----------------------->

## 今天我们要 ***使用 ValueNotifier 在不使用 setState 的情况下重建一个 widget。***

> 颤振数值通知器完全指南

![](img/f8a03bdc3d0ce1a4c1e7496332b73dba.png)![](img/861a61d642882fff2c4cd26b46525f1b.png)

Flutter 是 2020 年最受欢迎的移动应用开发跨平台框架之一。Flutter 是 Google 的 UI 工具包，用于从单一代码库为移动、web 和桌面构建漂亮的本地编译应用程序。在本帖中，我们将深入探讨 ValueNotifier 及其相应的主题。

首先，为了理解 ValueNotifier 的需求，我们需要理解 Flutter 的底层架构。我们需要了解调用时小部件是如何重建的。

> 颤振中的 ValueNotifier 简介

ValueNotifier 是一种特殊类型的类，它扩展了一个`**ChangeNotifier**`。ValueNotifier 是 Flutter 本身固有的，它提供单个元素的反应性。Flutter 默认也有一个小部件，用它我们可以监听名为`ValueListenableBuilder`的 ValueNotifier。也可以使用`AnimationBuilder`收听，因为它是可收听的。

> *把 ValueNotifier 想象成保存一个值的数据流。我们为它提供一个值，每个侦听器都会收到一个值更改的通知。*

我们可以创建任何类型的 ValueNotifier，例如 int、bool、list 或任何自定义数据类型。您可以像这样创建 ValueNotifier 对象:

```
ValueNotifier<int> counter = ValueNotifier<int>(0);
```

我们可以像这样更新该值:

```
counter.value = counter.value++;
//OR
counter.value++;
```

另外，我们可以这样听`ValueNotifier`:

```
counter.addListener((){
   print(counter.value); 
});
```

如果我们想在一个小部件中监听值，我们使用 ValueListenableBuilder 小部件，该小部件开箱即用，负责在视图之外删除订阅。因此，当使用 ValueListenableBuilder 时，我们不需要手动添加和删除侦听器，它会自动为我们完成这项工作。

# 移除值通知颤振中的监听器

如果我们手动监听一个 ValueNotifier，我们不`dispose`它，因为它在应用程序的某个地方是需要的。我们可以使用`removeListener` 函数，在当前页面上不使用时，从 ValueNotifier 中手动删除一个侦听器。

```
ValueNotifier<int> valueNotifier = ValueNotifier(0);void removeDemo() {
  valueNotifier.removeListener(doTaskWhenNotified);
}void addDemo(){
  valueNotifier.addListener(doTaskWhenNotified);
}void doTaskWhenNotified() {
  print(valueNotifier.value);
}
```

# 在 Flutter 中处理 ValueNotifier

当不再使用时，最好使用`dispose`值通知程序，否则可能会导致内存泄漏。通知程序上的`dispose`方法将释放任何订阅的侦听器。

```
@override
 void dispose() {
    counter.dispose();
    super.dispose();
 }
```

# 什么是 ValueListenableBuilder？

Flutter 中有许多类型的构建器，如 StreamBuilder、AnimatedBuilder、FutureBuilder 等。他们的名字暗示了他们消费的物品类型。ValueListenableBuilder 使用 ValueNotifier 对象。每次我们收到值更新时，都会执行 builder 方法。当我们路由到另一个页面时，ValueListenableBuilder 会自动在内部删除侦听器。

```
const ValueListenableBuilder({
    @required this.valueListenable,
    @required this.builder,
    this.child,
  })
```

这是 ValueListenableBuilder 的构造函数。这里，valueListenable 是要监听的 ValueNotifier。构建器函数接收 3 个参数`(BuildContext context, dynamic value, Widget child)`。`value`是从提供的`valueNotifier`接收的数据。可以使用子参数。如果子进程构建起来很昂贵，并且不依赖于通知程序的值，我们就用它来进行优化。

# 在颤振中使用值通知器的计数器应用程序

使用 ValueNotifer 和 ValueListenableBuilder 的计数器 app 看起来是这样的。注意没有使用`setState`，我们只重建了文本部分以防值发生变化。

```
import 'package:flutter/material.dart';

void main() {
  runApp(const App());
}

class App extends StatelessWidget {
  const App({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      home: HomePage(),
    );
  }
}

class HomePage extends StatefulWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  State<HomePage> createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  final _counterNotifier = ValueNotifier<int>(0);

  @override
  Widget build(BuildContext context) {
    print('HOMEPAGE BUILT');
    return Scaffold(
      appBar: AppBar(title: const Text('Counter App')),
      body: Center(
        child: ValueListenableBuilder(
          valueListenable: _counterNotifier,
          builder: (context, value, _) {
            return Text('Count: $value');
          },
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          _counterNotifier.value++;
        },
        child: const Icon(Icons.add),
      ),
    );
  }

  @override
  void dispose() {
    _counterNotifier.dispose();
    super.dispose();
  }
}
```