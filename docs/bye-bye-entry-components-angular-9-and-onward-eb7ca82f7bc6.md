# 再见入口组件(角度 9°及以上)

> 原文：<https://blog.devgenius.io/bye-bye-entry-components-angular-9-and-onward-eb7ca82f7bc6?source=collection_archive---------3----------------------->

![](img/767e59730d0873001fd5e6da05c6ea4e.png)

最近我在看关于 Angular 9 的官方发布信息，发现 Angular 团队已经决定放弃 entryComponents array，这是 NgModule 创建动态组件(如模型、广告横幅等)的关键部分。如果你想了解这个反对意见，你可以点击这里查看。

在 Angular 9 之前的版本中，如果我们试图动态创建任何不属于 entryComponents 的组件，我们会得到以下错误。

> 找不到 ModalComponent 的组件工厂。添加到@NgModule.entryComponents 了吗？

要理解这一点，我们需要理解为什么 Angular 在 Angular 9 之前需要 entryComponents 单独数组来动态创建这些组件。原因如下。

1.  如果我们仔细阅读错误，它是抱怨一个失踪的工厂。模板中有选择器的组件，Angular 用于创建 NgFactories。
2.  因为动态组件是用 ComponentFactoryResolver 创建的，模板中没有任何选择器，所以 Angular 没有为它们创建工厂。
3.  entryComponents 是一个组件数组，用于通知 Angular 关于这些组件的信息，以便它可以在运行时提供它们的 NgFactory。

现在我们明白了，为什么 Angular 需要 entryComponents 数组直到版本 8。但是从版本 9 开始发生了什么使得没有必要使用 entryComponents 数组呢？

答案是常春藤渲染器。有了 Ivy 渲染器，Angular 不再使用 NgFactory。相反，它将呈现组件所需的所有信息存储在静态属性 cmp 和 fac 中。所以，基本上组件的引用就足够了，而不是依赖于模板中的选择器。

所以现在你可以在声明数组中包含所有的组件，并且可以在以后通过选择器选择使用哪个组件以及动态创建哪个组件。

但是等等，是不是说 Angular 9 不需要 entryComponents。事实上，这完全取决于你是否使用常春藤渲染器。如果你最近更新了你的应用程序到 Angular 9，但是仍然有一些东西与 Ivy 不兼容，并且你仍然在没有 Ivy 的情况下运行你的项目。您仍然需要使用 entryComponents 数组，直到您决定为您的项目打开 Ivy。

但是如果你已经升级到 Angular 9，Ivy 是一个关键的特性，它会通过减少构建规模来给你一个主要的性能提升。所以最好的方法是将它升级到 Ivy 并去掉 entryComponents，因为它将在未来的版本中被删除。