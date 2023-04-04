# 以相反的方式快速扩展

> 原文：<https://blog.devgenius.io/swift-extensions-in-opposite-way-3038a3a38e52?source=collection_archive---------30----------------------->

![](img/fd5eaa961244e62c9fe0464a2ba12e05.png)

查尔斯·德鲁维奥在 [Unsplash](/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

想象一下，你正在创建一个应用程序，它应该跟踪用户指标的分析事件。为此，您选择了一个事件跟踪系统(如 Firebase)。

`AnalyticsProtocol`已声明，可以在`UIViewController`上用默认实现实现。这将把`AnalyticsProtocol`的功能添加到所有的`UIViewController`类型及其子类中。

让所有的视图控制器都符合这个协议可能不是最好的主意。如果你用这个扩展交付一个框架，实现这个框架的开发者将自动得到这个扩展，*不管他们喜不喜欢*。这不是最佳实践，因为如果框架中的扩展与应用程序中的现有扩展同名，它们可能会发生冲突。

避免这种情况的方法是翻转扩展:不是用一个协议扩展`UIViewController`，而是扩展一个被约束到`UIViewController`的协议。

```
protocol AnalyticsProtocol {
    func track(event: String, params: [String: Any])
} // bad approach 
extension UIViewController: AnalyticsProtocol {
    func track(event: String, params: [String: Any]) { 
        // implementation
    }
} // better approach 
extension AnalyticsProtocol where Self: UIViewController {
    func track(event: String, params: [String: Any]) { 
        // implementation
    }
}
```

默认情况下，这样可以防止所有视图控制器都遵循某个协议。当提供一个框架时，这是一个至关重要的技术，因为扩展没有命名空间。

```
// using methods of AnalyticsProtocol
extension ViewControllerA: UIViewController, AnalyticsProtocol {
    ...
}// not using methods of AnalyticsProtocol
extension ViewControllerB: UIViewController {
    ...
}
```