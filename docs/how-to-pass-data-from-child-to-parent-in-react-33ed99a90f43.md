# 如何在 React 中将数据从子节点传递到父节点

> 原文：<https://blog.devgenius.io/how-to-pass-data-from-child-to-parent-in-react-33ed99a90f43?source=collection_archive---------0----------------------->

React 面试中常见的问题。

![](img/20d0695f4cdebbbdbe93b9950914e1b1.png)

费伦茨·阿尔马西在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

React 中一个不太常见的细微差别是它遵循单向数据绑定。换句话说，在 React 的情况下，数据只向一个方向流动，从父组件流向子组件。反之，也就是说，将数据从子节点传递到父节点是不可能的，至少在理论上是不可能的。

数据可以通过 props 从父组件传递到子组件。父组件的任何属性，无论是变量还是函数，都可以作为道具传递给子组件。虽然没有直接的方法将数据从子组件传递到父组件，但是有一些变通方法。最常见的方法是将一个处理函数从父组件传递给子组件，该处理函数接受子组件的数据参数。这可以用一个例子来更好地说明。

```
import React from "react";export default function App() { return ( <div> <Parent /> </div> );}
```

考虑一个带有上面显示的`App`组件的简单应用程序。它在 div 元素中返回一个`Parent`组件。让我们看看`Parent`和`Child`组件。

```
const Parent = () => {
  const [message, setMessage] = React.useState("Hello World"); const chooseMessage = (message) => {
    setMessage(message);
  }; return (
    <div>
      <h1>{message}</h1>
      <Child chooseMessage={chooseMessage} />
    </div>
  );};const Child = ({ chooseMessage }) => {
  let msg = 'Goodbye';
  return (
    <div>
      <button onClick={() => chooseMessage(msg)}>Change    Message</button>
    </div>
  );};
```

`Parent`组件中的`message`变量的初始状态被设置为‘Hello World ’,并显示在`Parent`组件的 h1 标签中，如图所示。我们在`Parent`组件中编写一个`chooseMessage`函数，并将这个函数作为道具传递给`Child`组件。这个函数接受一个参数`message`。但是这个`message`参数的数据是从`Child`组件内部传递的，如图所示。因此，单击更改消息按钮，`msg`=‘Goodbye’将被传递给`Parent`组件中的`chooseMessage`处理函数，并且`Parent`组件中的`message`的新值将为‘Goodbye’。这样，来自子组件的数据(在`Child`组件的变量`msg`中的数据)被传递到父组件。

这是如何将数据从子组件传递到父组件的一个简单且相当基本的例子。了解这一点很方便，因为您可能会遇到具有多个组件级别的 React 项目，其中父组件可能依赖于/期望来自子组件的数据。