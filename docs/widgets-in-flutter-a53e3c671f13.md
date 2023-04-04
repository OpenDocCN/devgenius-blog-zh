# 飘动中的部件

> 原文：<https://blog.devgenius.io/widgets-in-flutter-a53e3c671f13?source=collection_archive---------5----------------------->

## 第 2 部分:什么是小部件，它们的用途是什么？

![](img/7749724c6c9560a1be49f0508be9cd0d.png)

瑞安·昆塔尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

> PS:这是我给初学者的扑系列的第二部分。如果你想看看剩下的部分，链接在文章的末尾。

如果你正在学习 Flutter，你可能遇到的第一件事就是小部件。它们是什么？

在 Flutter 中，一切都是“小部件”。受 [React](https://reactjs.org/) 的启发，用最简单的术语来说，小部件就像是应用程序的构建模块。应用程序中的每个组件都是一个微件。小部件描述了在给定当前配置和状态的情况下，它们的视图是什么样子。无论你看的是文本、按钮、表格视图还是回收器视图(取决于你是来自 iOS 还是 Android)、导航条，一切都是小部件。所有这些小部件组合在一起构建您的应用程序。

说到小部件，主要有两种类型的小部件:

1.  有状态小部件
2.  无状态小部件

让我们来看看他们每个人是做什么的。

## 有状态小部件

有状态的小部件可以根据某个值或触发的事件(如按钮点击、http 响应等)来改变它的外观。这些事件可以由用户操作触发，或者在满足条件的情况下以编程方式触发。有状态窗口小部件的例子有 TextField、Checkbox、Slider 等。

有状态小部件可以通过调用 *setState* 方法来重建。

这是有状态小部件的基本语法:

```
*class* Home *extends* StatefulWidget {
  @override
  _HomeState createState() => _HomeState();
}

*class* _HomeState *extends* State<Home> {
  @override
  Widget build(BuildContext context) {
    *return* Container();
  }
}
```

## 无状态小部件

简单地说，无状态小部件永远不会改变。文本、图标以及更大程度上的静态视图都是无状态小部件的例子。这些小部件只构建一次，之后就不再更改。更改无状态小部件中的任何组件都需要重新构建整个小部件。由于无状态小部件无法重建，因此这些小部件中定义的任何变量本质上都是不可变的。

无状态小部件的基本语法如下:

```
*class* Home *extends* StatelessWidget {
  @override
  Widget build(BuildContext context) {
    *return* Container();
  }
}
```

## 包裹

这篇文章让你对 Flutter 中的小部件有一个基本的了解。随着我们继续深入，我们将更详细地了解这些，并广泛地使用它们。和往常一样，请让我知道你想让我谈什么话题，我会尽我所能解释清楚。在那之前，再见！

PS。本文是系列文章的一部分。点击这里查看其他文章:

1.  [适合初学者的颤振](https://medium.com/dev-genius/flutter-for-beginners-c2fae3d4d540)
2.  [微件在颤动](https://medium.com/dev-genius/widgets-in-flutter-a53e3c671f13)
3.  [导航&颤振中的路由](https://medium.com/dev-genius/navigation-routing-in-flutter-655f08183084)
4.  [颤振布局](https://medium.com/dev-genius/layouts-in-flutter-c65b0dc6b356)
5.  [Listviews in Flutter](https://medium.com/dev-genius/listviews-in-flutter-12bab3dccadc)