# 反应类和功能组件

> 原文：<https://blog.devgenius.io/react-class-and-functional-components-b1a2f4153510?source=collection_archive---------43----------------------->

![](img/39a766fef9d3d4300a0506a7db3a39f0.png)

用户界面由不同的部分组成。一个网页可以有导航栏、标题、关于页面、联系页面等，在 React 中，我们将这些部分描述为**组件**。组件允许我们在 ui 的不同部分重用相同的 DOM 结构。例如，一条 tweet 可以分解为一个输入字段，有一个字符限制，一个 tweet 按钮，以及添加链接、图像、视频和转发的功能。这些部分中的每一个都可以被视为用于创建“分子”的原子，这就是推特的功能。

当使用 React 创建用户界面时，寻找机会将元素分解成可重用的部分。组件允许我们使用数据来构建可重用的 UI。

有两种类型的组件:**功能组件和类组件。**

# **类组件**

Javascript 不是面向对象的语言，因此类不是 Javascript 的固有特性，而是作为 ES6 的关键特性引入的。

创建一个类组件:定义一个扩展**组件**并具有渲染功能的类。render 函数是组件生命周期方法之一，也是类组件中唯一需要的*方法。其他生命周期方法可以在类组件中使用，例如 componentDidMount。*

```
import React, { Component } from "react";class App extends Component{
 render(){
   return(){
     <header> Hi There </header>
  }
}export default Header;
```

这个类组件只是呈现一个 header 元素。通常，类组件是“智能”组件，这意味着它们具有状态和实现逻辑。

```
import React, { Component } from "react";class App extends Component{
constructor(props)
super(props)
this.state = { 
  name = "Olu";
} render(){
   return(){
    <Header name = {this.props.name}/>
  }
}
```

在上面的代码片段中，App 组件将名称存储为 state，并将其作为 prop 传递给代码片段在下面的 Header 组件。

# **功能组件**

功能组件通常被称为“哑的”/无状态组件，因为它们只是接受数据作为道具并返回一个 DOM 元素。如果不需要跟踪组件中的状态，建议使用功能组件而不是类组件。功能组件应该接受 props 并返回一个 DOM 元素而不会引起副作用，使它们具有高度的可测试性。因为它们只是 Javascript 函数而不是类，所以它们的作用域中没有“this”。您也*不能*实现像 render 和 componentDidMount 这样的组件生命周期方法。

要创建功能组件，可以使用 Es6 箭头功能或使用关键字*功能*。

```
import React from 'react';const Header = props =>{
<div>
    <h1> Hello, {props.name} </h1>
</div>
}
export default Header;
```

在这里，Header 组件从 App 组件接收名称作为道具，并将 *Hello，Olu* 呈现到 U.I

React 的一个基本主题是理解类和功能组件。希望这篇文章有帮助！