# 编写用于在 Swift 中设置自动布局约束的扩展

> 原文：<https://blog.devgenius.io/writing-an-extension-for-setting-up-autolayout-constraints-in-swift-b6b5d56aa798?source=collection_archive---------7----------------------->

![](img/d2f34616d0446e15ba85ac4eedc5e385.png)

哈基姆·拉赫曼在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

我们都知道，对于任何 iOS 应用程序，我们都需要使用 Autolayout 来使我们的应用程序在每台设备上看起来一致。

故事板适用于 UI 上的 UI 控件很少的应用程序，我们可以很容易地从故事板设置自动布局。然而，如果我们以编程方式创建 UI，我们需要以编程方式添加约束。

如果数量较少，在每个视图控制器中设置自动布局很容易。然而，如果应用程序有超过 10-15 个 ViewController，这是大多数中型或大型应用程序的情况，这不是一个好的做法，因为您需要在大多数 view controller 中编写类似的代码。

使用 Swift，您可以通过编写 UIView 的扩展来轻松解决这个问题。下面是代码。

```
**extension** UIView {**func** **addConstarint**(top: NSLayoutYAxisAnchor?, left: NSLayoutXAxisAnchor?, bottom: NSLayoutYAxisAnchor?, right: NSLayoutXAxisAnchor?, paddingTop: CGFloat, paddingLeft: CGFloat, paddingBottom: CGFloat, paddingRight: CGFloat, width: CGFloat, height: CGFloat) {translatesAutoresizingMaskIntoConstraints = **false
// Use the top parameter to set the top constarint****if** **let** top = top {
**self**.topAnchor.constraint(equalTo: top, constant: paddingTop).isActive = **true** }
**// Use the left parameter to set the left constarint****if** **let** left = left {
**self**.leftAnchor.constraint(equalTo: left, constant: paddingLeft).isActive = **true** }
**// Use the bottom parameter to set the bottom constarint****if** **let** bottom = bottom {
**self**.bottomAnchor.constraint(equalTo: bottom, constant: -paddingBottom).isActive = **true** }
**// Use the right parameter to set the right constarint****if** **let** right = right {
**self**.rightAnchor.constraint(equalTo: right, constant: -paddingRight).isActive = **true** }**// Use the width parameter to set the top constarint
if** width != 0 {
widthAnchor.constraint(equalToConstant: width).isActive = **true** }
**// Use the height parameter to set the top constarint****if** height != 0 {
heightAnchor.constraint(equalToConstant: height).isActive = **true**}
}
}
```

在上面的 **addConstarint** 方法中，我们使用了许多参数，比如顶部、底部、左侧、右侧、高度和宽度，因此您可以传递您想要设置的任何约束值。

您可以从任何一个 ViewController 中调用上面的 **addConstarint** 方法，一切就绪。

为了更好的编码实践，将上面的代码放在一个单独的文件中，将其命名为 **AppExtension.swift** ，或者您想要的任何名称。

# 在 ViewController 中使用 UIView 扩展

让我们在任何 viewController 中使用我们的扩展，看看神奇之处。假设您有一个名为 signInButton 的按钮，您想在其中添加这个扩展来设置您的约束。

登录按钮。 **addConstarint** (上: **PYV** ，左: **PYV** ，下: **PYV** ，右: **PYV** ，paddingTopTop: **PYV** ，paddingLeft: **PYV** ，paddingBottom: **PYV** ，paddingRight: **PYV** ，padding

***PSV:传递你的值**

只需一行代码，您就可以根据需要传递适当的值，将自动布局设置为登录按钮。

我希望这将有助于您通过创建 UIView 的扩展并从多视图控制器调用它来在应用程序中设置自动布局。

如果你认为我需要编辑一些东西或者有什么建议，请尽管提出来。永远欢迎。

编写可读且不重复的代码是提高编码技能的最佳方式。