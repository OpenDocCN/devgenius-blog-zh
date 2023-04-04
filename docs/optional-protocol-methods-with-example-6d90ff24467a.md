# 可选协议方法及示例

> 原文：<https://blog.devgenius.io/optional-protocol-methods-with-example-6d90ff24467a?source=collection_archive---------24----------------------->

![](img/e71550d5bfabb434176653584a21b224.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Max Duzij](https://unsplash.com/@max_duz?utm_source=medium&utm_medium=referral) 拍照

默认情况下，Swift 协议中列出的所有方法必须以符合的类型实施。然而，根据您的需要，有两种方法可以绕过这个限制
**第一种选择**是使用@objc 属性标记您的协议(这意味着它只能被类采用，这意味着您将单个方法标记为可选的)

```
@objc protocol PaymentActionViews { @objc optional  func hideView(view: UIView)
   @objc optional  func showView(view: UIView)
   func setSelectButton(button: UIButton)}
```

**第二个选项** 编写不做任何事情的可选方法的默认实现或者做默认实现(这是更好的选项)

```
protocol Printable{
    func canPrint() -> Bool
}extension Printable{ func canPrint() -> Bool{
        return true
    }
}
```