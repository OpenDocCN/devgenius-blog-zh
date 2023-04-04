# React-Redux &语义 UI React 的威力

> 原文：<https://blog.devgenius.io/react-redux-the-power-of-semantic-ui-react-d1bb5ef354c4?source=collection_archive---------2----------------------->

![](img/53490c3864e631749e71d9584eeb871d.png)

米利安·耶西耶在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# Redux 是什么？

简单明了地说，Redux 是 Javascript 应用程序的可预测状态容器，从普通 Javascript 一直到 React Javascript 这样的框架。

作为开发人员，Redux 对我们尤其有益，因为它解决了处理属性或道具的复杂性问题。随着我们的应用程序变得越来越大，我们的状态和道具在不同的组件之间变得更加分散。让你的组件变成一堆乱七八糟的道具和状态，这会模糊我们对组件如何处理和共享数据的看法。

Redux 对此提供了一个非常独特和令人敬畏的解决方案，而不仅仅是简单地将我们的状态移动/存储一个层次，Redux 鼓励将所有数据存储在一个与组件分离的 JS 对象中。Redux state 独立于组件树，允许我们作为开发人员通过简单地连接组件来获取和使用我们想要的任何组件的数据。

# 从 API 获取 Redux 中的状态

> [要改变状态，您需要分派一个动作。动作是一个普通的 JavaScript 对象(注意我们没有引入任何魔法？)描述了所发生的事情。](https://redux.js.org/introduction/core-concepts)

在我的 React 项目中，我使用调度操作从我的 API 端点获取数据。然后返回一个承诺，我接受这个承诺，它就变成 JSONified 了。然后，JSON 响应被分派给“FETCH_CARDS”类型键，它将该值等同于有效载荷。

Redux 的阻力是一个称为减速器的功能。Reducers 允许我们将状态和动作联系在一起。Reducers 接受一个状态、一个动作和一个新状态，以返回到应用程序。

现在，因为我们只能向 React 应用程序发送一个减速器，所以我们必须使用组合减速器功能，该功能允许我们组合我们需要的任意多个减速器，但只能向我们的应用程序发送一个干净的减速器。如下所示:

# 语义 UI 反应

首先，我想说明我是多么喜欢在 React 应用程序中使用语义 UI。它提供了一种快速简单的方法来设计我的整个应用程序。我不能低估在应用程序中设置和运行语义 UI 的速度。如果你从未听说过语义 UI，语义 UI React 是语义 UI[的官方 React 集成，你可以点击链接查看更多的文档。](https://semantic-ui.com/)

到目前为止，我最喜欢在 React 中使用语义 UI 的地方是语法。它和 JSX 是如此的相似，以至于在使用它的时候，它感觉就像是我手的自然延伸。

 [## 表单-语义 UI 反应

### 简介入门写作速记道具主题布局示例原型 v2 迁移指南

react.semantic-ui.com](https://react.semantic-ui.com/collections/form/#types-form) 

如果你不相信我，你可以点击链接，自己去看一下。真的很棒。它与 React 的虚拟 DOM 配合得非常好，因为它是 JQuery 免费的。

我肯定计划在我将创建的所有未来项目中继续使用 Redux 和语义 UI。所以，说了这么多，编码快乐！希望这篇文章能在某种程度上帮助你。