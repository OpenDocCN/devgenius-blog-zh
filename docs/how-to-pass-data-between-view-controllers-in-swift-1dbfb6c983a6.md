# 如何在 swift 中的视图控制器之间传递数据

> 原文：<https://blog.devgenius.io/how-to-pass-data-between-view-controllers-in-swift-1dbfb6c983a6?source=collection_archive---------40----------------------->

![](img/edb0181a6b88c98be75c39c56e8590ae.png)

麦克斯韦·尼尔森在 T2 的照片

# 背景

一个月前，我在开发一个 iOS 应用程序，它使用 Firebase 中的实时数据库来更有效地存储、访问和利用用户输入的数据。

当存储和检索用户数据时，我需要创建对数据库的引用。

为了成功地创建引用，我需要知道 Firebase 数据库中正确的子名称。不过孩子的名字并不固定，所以我不能硬编码。

我需要将孩子的名字赋给一个变量，并将该变量传递给我的数据库引用。

这就是问题发生的地方…

# 问题是

对于我的项目，我只能从我的 MainViewController 访问子名称，并且只能在我的 SecondaryViewController 中创建我的数据库引用。因此，我需要将名称从 MainViewController 传递到 SecondaryViewController。

我研究了几种不同的方法，最终决定使用 segue 在视图控制器之间传递子名称。

我是这样做的…

# 解决方案

首先，在我的 MainViewController 中，我创建了一个变量来访问和存储孩子的名字。

```
let name: String = data.name
```

其次，在我的 SecondaryViewController 中，我为子名称创建了一个变量。这是从 MainViewController 传递到 SecondaryViewController 时存储名称的位置。

```
var name: String = “ “
```

第三，在我的 MainViewController 中，我定义了 prepare(for:sender:)函数，该函数将在视图控制器之间切换之前被调用。

```
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {}
```

第四，在 MainViewController 的 prepare(for:sender:)函数中，我使用了 if 语句和可选的强制转换来确定 segue 目的地是否是 SecondaryViewController。我还定义了一个名为 vc 的新变量，让我可以访问 SecondaryViewController 中的 name 变量。

```
if let vc = segue.destination as? SecondaryViewController {}
```

第五，在 MainViewController 的 if 语句中，我访问了 SecondaryViewController 中的 name 变量，并将其设置为与 MainViewController 中的 name 变量相等。

```
vc.name = name
```

仅此而已！我发现这是一个非常简单有效的方法。下面是完整的代码:

主视图控制器:

```
let name: String = data.nameoverride func prepare(for segue: UIStoryboardSegue, sender: Any?){if let vc = segue.destination as? SecondaryViewController{vc.name = name}}
```

SecondaryViewController:

```
var name: String = “ “
```

# 最后

下面是我发现的另一个有助于学习在视图控制器之间传递数据的资源:[https://learn appmaking . com/pass-data-between-view-controllers-swift-how-to/](https://learnappmaking.com/pass-data-between-view-controllers-swift-how-to/)

segue 方法对我很有效，但是可能有更好的方法。留下你将如何解决这个问题的评论，让我知道你是否有更简单的方法！