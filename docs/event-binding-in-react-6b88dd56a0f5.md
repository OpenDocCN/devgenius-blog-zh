# React 中的事件绑定

> 原文：<https://blog.devgenius.io/event-binding-in-react-6b88dd56a0f5?source=collection_archive---------9----------------------->

![](img/4c35b75881b8fde05f1993799f8bee3c.png)

React 中的事件绑定可能很棘手。这不是 React 的错，要怪就怪 Javascript。在我的[上一篇](https://medium.com/@shannonmakenna/lexical-scope-and-arrow-functions-in-javascript-4d67dafbbf59)文章中，我简要解释了 Javascript 中的*这个*关键字是什么意思，以及如何确保你引用的是你想要引用的对象。在本文中，我将专门讨论 React 中绑定事件的不同方式，以及每种方式的优缺点。

这里，我的 App.js 有一个状态计数变量，它的值 I 将通过使用 increaseCount 函数增加，该函数将作为 button 元素的属性传递。

```
import React, { Component } from "react";class App extends Component { constructor(props) { super(props); **this.state = {** **count: 0** **}** }
increaseCount() { this.setState(prevState => ({ count: prevState.count + 1 })); }render() { return ( <div className="App">
       ***<button onClick = {this.increaseCount}> 
         Count: {this.state.count}</button>***
     </div>
) export default App;
```

当你像上面那样调整这段代码时，你会得到一个类型错误“无法读取未定义的‘setState’的属性”。这是因为*这个*指的是窗口对象而不是类本身。因此，让我们通过绑定事件处理程序来解决这个问题。

## 在渲染函数中绑定

一种选择是在 render 方法中绑定事件处理程序。

```
*render() {
 return(
  <button onClick =****{this.increaseCount.bind(this)}****>
     Count: {this.state.count}
  </button>
 )
}*
```

虽然此选项有效，但它会导致性能问题，因为每次状态更新都会导致应用程序组件重新呈现，并在每次呈现时生成一个全新的事件处理程序。对于一个事件处理程序来说，这可能不是问题，但是如果您有多个事件处理程序，您的性能将会降低。

## 在 Render 方法中使用箭头函数

您可以使用箭头函数来调用 render 方法中的函数。请注意，我们正在**调用**函数，因此语法如下( *this.increaseCount())*

```
*render() {
 return(
  <button onClick = {() => this.increaseCount()}> 
    Count:{this.state.count}</button>
}*
```

这种方法的缺点类似于第一种方法，即在每次重新渲染时创建一个新的事件处理程序。

## 构造函数中的绑定

常见且稳定的方法是在构造函数中绑定事件处理程序

```
constructor(props) { super(props); this.state = { count: 0
  }**;**
 ***this.increaseCount = this.increaseCount.bind(this);*** }
```

这种方法的好处是调用 render 方法不会为 onClick 生成新的处理程序，因此按钮元素不会被重新呈现。

## 用箭头函数绑定

箭头函数是作为 ES7 类属性引入的，它们提供了绑定事件处理程序的最简单方式。

```
increaseCount = () => { this.setState(prevState => ({ count: prevState.count + 1 }));
};*render() {
 return(
  <button onClick = {this.increaseCount}> 
    Count:{this.state.count}</button>
}*
```

然后，该方法被分配给 render 方法中的 button onClick 属性。一旦组件被初始化，this.increaseCount 将永远不会改变，按钮也不会被呈现。

**结论**

要在 React 中绑定事件处理程序，**最简单、最有效的方法**是绑定箭头函数。因为这是一个新特性，所以在构造函数方法中使用绑定的代码并不少见。这是一个同样有效的选择。

感谢阅读。希望这篇文章有所帮助。关于绑定事件处理程序的更详细的解释，我推荐阅读[这篇](https://www.freecodecamp.org/news/the-best-way-to-bind-event-handlers-in-react-282db2cf1530/) FreeCodeCamp 文章。