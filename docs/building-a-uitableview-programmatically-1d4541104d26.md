# 以编程方式构建 UITableView

> 原文：<https://blog.devgenius.io/building-a-uitableview-programmatically-1d4541104d26?source=collection_archive---------0----------------------->

## 编程 UITableView

## 以编程方式创建表格视图| Swift 5.3 和 Xcode 12

![](img/2f17fa8e66b89aa965d9def0614a16d5.png)

作为用户，我们在自己的设备上启动应用程序，但我们从来不知道应用程序中到底发生了什么。我们只知道这个应用程序“华丽、美丽，甚至壮观”。但是作为一名开发人员，我们不用看代码就知道代码中发生了什么。我们可以看出这是一个 UITableView，甚至可能是一个 UICollectionView。

今天我们只看 UITableView。具体来说，我们将着眼于以编程方式构建一个“UITableView”。让我们来看两种方法来解决这个问题。一种是通过一个函数创建我们的 UITableView，这个函数赋予我们调用它的能力。第二个是在计算属性中创建 UITableView。两种方法都会给你同样的结果。

# 通过计算机属性初始化 ViewController 中的 UITableView。

我们将设置一个带有普通视图控制器的应用程序。如果你不知道如何以编程方式设置你的项目，请看看我过去的文章或者你可以看看我的 youtube 视频。一旦我们的应用程序启动并正确配置，让我们跳到我们的 ViewController 文件。

这里的第一个选项是通过一个计算属性来设置它。

# 通过函数初始化 ViewController 中的 UITableView。

这里的第二个选项是用函数设置 UITableView。

请记住，如果您不引入"***translatesAutoresizingMaskIntoConstraints***"您将不会在屏幕上看到您的 UITableView 填充！这个美妙的长词的定义——来自苹果文档——如下所列。

> 如果该属性的值为`true`，系统将创建一组约束，这些约束将复制视图的自动调整大小掩码所指定的行为。这也允许您使用视图的`[frame](https://developer.apple.com/documentation/uikit/uiview/1622621-frame)`、`[bounds](https://developer.apple.com/documentation/uikit/uiview/1622580-bounds)`或`[center](https://developer.apple.com/documentation/uikit/uiview/1622627-center)`属性修改视图的大小和位置，允许您在自动布局中创建静态的、基于框架的布局。
> 
> 请注意，自动调整大小掩码约束完全指定了视图的大小和位置；因此，如果不引入冲突，就无法添加额外的约束来修改此大小或位置。如果您想使用自动布局来动态计算视图的大小和位置，您必须将该属性设置为`false`，然后为视图提供一组明确、无冲突的约束。
> 
> 默认情况下，对于您以编程方式创建的任何视图，该属性都被设置为`true`。如果在界面构建器中添加视图，系统会自动将该属性设置为`false`。

# 总之！

现在，如果我们要在故事板文件中这样做，您可以像平常一样设置 UITableView。当使用编程代码时，它是关于你如何用动画、风格和用户友好来定制它。希望你们喜欢这篇小短文！