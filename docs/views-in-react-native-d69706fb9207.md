# React Native 中的视图

> 原文：<https://blog.devgenius.io/views-in-react-native-d69706fb9207?source=collection_archive---------13----------------------->

![](img/094a2150d555e67f540b5a3eeb738d3d.png)

由[保罗·斯科鲁普斯卡斯](https://unsplash.com/@pawelskor?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/view?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

如果你有构建动态 web 应用程序 ui 的经验，你可能熟悉[前端 javascript 框架 React](https://reactjs.org/) 。React 非常强大且易于使用，它使创建现代 web 应用程序成为一个真正优雅的过程。

在习惯了 React 中的编码之后，您可能会发现自己希望能够使用 React 开发各种应用程序，而不仅仅是前端 web 应用程序。幸运的是， [React Native](https://reactnative.dev/) 的创建是为了让您可以构建熟悉的 React JS 应用程序，在 iOS 和 Android 等移动平台上作为原生应用程序运行。

看看网上的几个 React 原生教程，你会发现它看起来有多熟悉。如果你是一个经验丰富的 React 开发人员，学习曲线是相当低的，因为你仍然是在开发一个基于组件的 javascript 应用程序。

React native 和您可能已经习惯的 ReactDOM web 实现之间有一个关键的区别:React Native 的 JSX 不使用普通的 HTML。相反，所有元素都是使用[特定的预构建“核心组件”](https://reactnative.dev/docs/components-and-apis)实现的，React 本地引擎将这些组件直接转换为本地移动操作系统上的 UI 元素。

这些组件有自己的内部逻辑，需要特定的道具和信息才能正常工作。几乎所有用于 UI 设计/布局的 HTML5 元素都有类似的组件，将 reactDOM jsx 文件转换成功能性的原生版本并不困难。原生核心组件及其子组件的特殊性使它成为一个更加固执己见、更少变化的环境，这可能被视为一件好事或坏事，取决于您的偏好和以前的编码经验。

## 视图组件

在 React 本地组件的返回值中，[您将使用的最基本的单元是视图组件](https://reactnative.dev/docs/view)。将`<View>`视为普通 react jsx 中的`<div>`的等价物。

您可能会将整个组件放在一个视图容器中，以及组件特定部分的更多视图容器。视图容器让你定义[样式](https://reactnative.dev/docs/style)以及更多的移动[平台细节，比如触摸处理](https://reactnative.dev/docs/handling-touches)。

React Native 中应用程序的整体样式是由主视图组件处理的[，尽管所有组件都有自己特定的样式细节。样式是在 Javascript 本身中实现的，而不是在单独的 CSS 样式表中实现的。react 原生样式是一个普通的旧 javascript 对象，其键和值与传统的 CSS 属性相同。](https://reactnative.dev/docs/view-style-props)

可以为每个核心组件单独定义一个样式对象，或者在视图上使用更复杂的样式时，将它们一起嵌套在一个更大的样式表对象中，类似于一个完整的 CSS 样式表。React Native 甚至为此提供了一个特殊的样式表内置对象——使用 StyleSheet.create()设置样式，以便在页面的所有组件中引用。样式表对象为您验证所有提供的样式，然后只处理一次，使您的代码比需要解析许多单独的 POJO 样式更加优化和高效。

React Native 是一个迷人的框架，它使得 web 开发人员开发本地应用程序变得非常容易。希望这是对 React 本地风格世界的有益介绍。