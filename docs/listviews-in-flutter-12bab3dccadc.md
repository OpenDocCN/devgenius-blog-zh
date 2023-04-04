# 颤振中的列表视图

> 原文：<https://blog.devgenius.io/listviews-in-flutter-12bab3dccadc?source=collection_archive---------2----------------------->

## 第 5 部分:在 Flutter 中使用列表

![](img/d7b5ec8b839ee7aab2c5b0b7f40bddff.png)

奥米德·卡什马里在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

> PS:这是我的颤振基础系列的最后一章。如果你想看看剩下的部分，链接在文章的末尾。

在这一章中，我们将看看如何在 Flutter 中实现 listviews。

当我们必须显示一个项目列表时，ListView 是最常用的小部件。想想联系应用，Twitter/FB/Instagram 上的帖子。它们都以这样或那样的方式实现了列表视图。在 Android 中，我们将使用回收器视图来实现这一点。在 iOS 中，我们将使用 UITableView 来实现这一点。Flutter 团队给了我们 ListViews 来实现同样的功能。让我们看看 ListViews 是如何工作的。

首先，ListView 是 CustomScrollView 的简化版本，只包含一个 SliverList。不同之处在于，Flutter 团队已经完成了大量繁重的工作，我们拥有的是一种更简单的方式来显示项目列表。在本文中，我们将不涉及列表。如果你想知道更多关于 SliverLists 的信息，请在评论中告诉我。

如果你看下面的例子，我们有一个 ListView 的基本实现。ListView 有一个 children 参数，它接受一个 Widget 类型的列表。如果您还记得的话，我们在上一篇文章中已经在行和列小部件中看到了类似的东西。不同之处在于，这款手机可以容纳比屏幕上显示的更多的内容，并且具有滚动功能。

默认情况下，ListView 会自动填充可滚动的极值。这样做是为了避免端部的部分阻塞。您可以通过将填充设置为零来覆盖它。

在 Flutter 中实现列表视图的例子

现在我们已经了解了 ListView 是如何工作的，让我们来看一些稍微有趣的东西。在上面的例子中，我们显示了列表中定义的项目。但是现在让我们假设我们想要基于索引或来自服务器的一些数据按需构建小部件。ListView.builder()来了。ListView.builder()小部件的一个重要优势是内容是以惰性方式构建的，也就是说，子小部件在滚动到视图端口之前不会呈现。这在显示大量数据时非常有用。

在下面的示例中，您可以看到 ListView.builder()有一个 itemBuilder 参数，它实现了一个 builder 函数。您可以使用项目索引来显示列表中的内容。

> **重要提示:**很多在线教程都说，如果你要实现无限滚动，可以忽略 itemCount 参数。这是绝对不正确的！

itemCount 帮助 ListView 估计最大滚动范围。只需从下面的示例中删除 itemCount 并运行它，就可以看到不提及 itemCount 的效果。颤动将抛出一个索引错误。

如果希望实现无限滚动，更好的方法是实现一个提供者，并在从服务器异步获取数据时不断更新列表。

使用 builder 方法的 ListView 示例

# 包裹

伙计们，这就把我们带到了这篇文章的结尾。我们已经了解了 ListViews 的基本实现，并理解了 ListViews 是如何工作的。

这就把我们带到了这个系列的结尾。我希望你喜欢读这篇文章，就像我喜欢写它一样。我很快会带着一个新的系列回来，涵盖了 Flutter 中不同的主题。像往常一样，让我知道你想让我谈论什么话题，不要忘记通过鼓掌和与你的朋友分享来表达你的支持。在那之前，编码快乐！

PS。本文是系列文章的一部分。点击这里查看其他文章:

1.  [适合初学者的颤振](https://medium.com/dev-genius/flutter-for-beginners-c2fae3d4d540)
2.  [微件在飘动](https://medium.com/dev-genius/widgets-in-flutter-a53e3c671f13)
3.  [导航&路由在扑](https://medium.com/dev-genius/navigation-routing-in-flutter-655f08183084)
4.  [颤振布局](https://medium.com/dev-genius/layouts-in-flutter-c65b0dc6b356)
5.  [Flutter 中的列表视图](https://medium.com/dev-genius/listviews-in-flutter-12bab3dccadc)