# 哪里让你失望了

> 原文：<https://blog.devgenius.io/where-ngondestroy-fails-you-54a8c2eca0e0?source=collection_archive---------1----------------------->

![](img/13c19d3a01a89f3517b76f92b25548ee.png)

亚历山大·安德鲁斯在 [Unsplash](/s/photos/navigation?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

[ngOnDestroy](https://angular.io/api/core/OnDestroy) 是 Angular 迄今为止最重要的生命周期挂钩之一。它用于取消订阅事件，并在销毁组件/服务之前处理任何拆卸事件。尽管我们对 NGO destroy 完成这些事情充满信心，但有时候 NGO destroy 实际上会让你失望。

对于 4 个导航事件，将*而不是*调用 ngOnDestroy:

*   页面刷新
*   关闭选项卡/浏览器
*   导航到不同的页面

在这些情况下，如果用户执行这些导航事件中的任何一个，运行某些需要保存的指令(比如临时会话状态)将不会被保存。

例如，让我们假设我们有数据，我们临时存储了 [sessionStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage) 对象，我们希望在组件被销毁时保存这些数据。

但是当我们按 F5 重新加载页面时会发生什么呢？数据不持久！ngOnDestroy 不会被调用，因为当导航到不同的路径时，一些组件不会被破坏。

## 拯救路由器事件！

我相信你一定知道 Angular 内部的[路由器](https://angular.io/api/router/Router#events)服务。您可以使用这项服务来订阅任何与角度路线和导航相关的内容。

使用这个类，我们可以订阅 [events](https://angular.io/api/router/Router#events) 变量，这是一个订阅路由器事件的可观察变量。

订阅可观察事件时，有几种不同的导航周期可供您订阅。其中包括:

*   **导航开始**:导航开始时触发。
*   **GuardCheckStart** :检查组件的第一个防护(如果有)时触发。
*   **ActivationStart** :路由的[解析](https://angular.io/api/router/Resolve)阶段触发

**注意:**这些周期中的每一个也有它们的结束版本

回到上面的例子，我们可以做两件事:将代码放在事件的订阅者内部，或者在订阅者内部调用 ngOnDestroy。

以下是所做的更改:

1.  将路由器服务注入到类中
2.  订阅 ngOnInit 内部可观察到的事件
3.  在那里调用代码

## 外卖食品

只有在刷新应用程序或离开应用程序时必须保存数据时，才应使用此方法。在大多数情况下，ngOnDestroy 将完成您需要的一切，尤其是在取消订阅事件时。这些代码可以帮助您处理在执行浏览器导航活动时需要调用代码的任何必要情况。

希望这有所帮助！