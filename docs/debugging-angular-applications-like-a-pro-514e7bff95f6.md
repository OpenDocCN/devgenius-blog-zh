# 像专家一样调试角度应用程序

> 原文：<https://blog.devgenius.io/debugging-angular-applications-like-a-pro-514e7bff95f6?source=collection_archive---------4----------------------->

![](img/c3002a7a1e73042d0cd2ec077e60cba1.png)

**通过** [**努贝尔森·费尔南德斯**](https://unsplash.com/@nubelsondev) 调试来自 Unsplash 的图像

我们时不时会遇到 angular 组件的奇怪问题，调试的一种方法当然是在 DevTools 或 IDE 中使用断点。但是它们可以帮助你检查一段被调用的代码，比如方法等等。

但是，假设我想查看一个属性的当前值，并重新呈现一个使用 OnPush 策略的 Angular 组件，以查看调用 applyChanges 是否会在 ui 上呈现实际值。是的，您可以在代码中使用控制台日志，并尝试将 applyChanges 放入代码中，然后查看更新。但是，如果我们可以从浏览器本身做这些事情，难道不会花费更少的时间吗？

举个例子，你在 ngFor 中渲染一个组件，所以同一个组件有多个实例。例如:-

因此，呈现了 10 个 employee 组件，您希望只看到 id 为 emp-4 的组件的 employee 属性值。从元素中找到组件实例，然后在浏览器控制台中查看属性值，而不是一次又一次地更改代码，并通过控制台日志进行调试，特别是当您处理许多集成在一起的组件时，这样不是更容易吗？

让我们看看如何用 Angular 实现这一点。

从 Angular 9 开始，Angular 公开了一个名为 **ng** 的名称空间，其中有一些方法可以帮助我们轻松调试，而不会因为有控制台日志而有太多麻烦和代码更改。让我们看看官方文档是怎么说的。

> 在全局命名空间中公开一组函数，这些函数对于调试应用程序的当前状态非常有用。当您从`@angular/core`导入并在开发模式下运行您的应用程序时，这些函数通过全局`ng`“namespace”变量自动公开。当应用程序在生产模式下运行时，这些函数不会公开。

名称空间包含以下函数:-

1.  **ng.getComponent**

检索与给定 DOM 元素关联的组件实例。例如，在我们的例子中，当我们想要 id 为 emp-4 的元素的组件实例并读取它的 employee 属性时。我们可以在浏览器控制台中编写以下内容。

我们也可以从浏览器控制台本身调用该组件中的任何方法。

**2。ng.applyChanges**

将组件标记为检查(如果是 OnPush 组件),并对该组件所属的应用程序同步执行更改检测。因此，假设我们想在我们刚刚通过 getComponent 获得的 emp4Comp 上调用 applyChanges，我们可以像下面这样做。

**3。ng.getOwningComponent**

检索其视图包含 DOM 元素的组件实例。因此，在我们的例子中，如果我在 emp4Comp 上调用它，它会给我 AppComponent 的实例，如果我在 employee 组件内的 div 上调用它，它会给我 employee component 的实例。可以这样用。

**4。ng.getContext**

如果在嵌入式视图中(如 ***ngIf** 或 ***ngFor** )，检索元素所属的嵌入式视图的上下文。否则检索其视图拥有该元素的组件的实例(在这种情况下，结果与调用 **getOwningComponent** 相同)。

**5。ng.getDirectives**

检索与给定 DOM 元素关联的指令实例。不包括组件实例。

**6。ng.getHostElement**

检索组件或指令实例的宿主元素。host 元素是与指令的选择器匹配的 DOM 元素。

**7。ng.getInjector**

检索与元素、组件或指令实例相关联的**注入器**。可以像下面这样使用。

**8。ng.getListeners**

检索与 DOM 元素关联的事件侦听器列表。该列表包括主机侦听器，但不包括在角度上下文之外定义的事件侦听器(例如，通过 addEventListener)。

**9。ng.getRootComponents**

检索与 DOM 元素、指令或组件实例关联的所有根组件。根组件是那些由 Angular 引导的组件。

> 这些是 Angular 公开的 API，对于开发模式下的调试非常有用。如果使用得当，我们可以从浏览器控制台调试几乎所有东西，访问我们的 angular 应用程序的不同部分，并使用它们的方法和属性。

祝你即将到来的错误和容易调试好运。