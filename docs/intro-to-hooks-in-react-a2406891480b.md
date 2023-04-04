# React 中的挂钩简介

> 原文：<https://blog.devgenius.io/intro-to-hooks-in-react-a2406891480b?source=collection_archive---------34----------------------->

我们如何在 React 中应用`useState()`和`useEffect()`？

![](img/eb3f977d19328e8e08ed9436cef4560a.png)

鱼钩

要理解 hooks，您应该对以下内容有一个很好的理解:

*   反应 16.8+
*   状态和道具
*   生命周期方法
*   功能组件

# 什么是钩子？

钩子是 16.8 版本中为 React 添加的 JavaScript 函数。它们允许开发人员“挂钩”react 特性并操纵状态，而无需编写类。它们的目的不是从 React 中删除类组件，而是节省在类、函数、高阶组件和渲染道具之间切换的时间。当您第一次开始练习钩子时，最好先用不复杂的组件来尝试它们，然后再编写更复杂的组件，看看它们的全部潜力。它们也可以**仅**用于功能组件。

# useState()挂钩

要声明 state，您必须设置两个变量，在本例中我们将使用`count`和`setCount`，并将其设置为等于`useState()`函数。传递给`useState()`函数的参数将是我们的初始值。我们必须设置两个变量，因为`useState()`返回一对值。让我们把它编码出来！

```
import React, {useState} from 'react';function Counter() {//Here we declared 'count' and 'setCount'
//count is set to equal 0, and setCount is our function that we will use to update state later const [count, setCount] = useState(0);}
```

我们之所以把`count`和`setCount`放在一个数组中，是因为如前所述，`useState()`返回一对值。这被称为**数组析构。**引擎盖下基本就是这个样子。

```
let counterVariable = useState(0);
let counter = counterVariable[0];
let setCounter = counterVariable[1]; 
```

回到我们的函数，如果我们想要读取我们的当前状态，我们可以像这样将`count`变量添加到我们的返回中。

```
import React, {useState} from 'react';function Counter() {//Here we declared 'count' and 'setCount' to equal 0
 const [count, setCount] = useState(0); return (
  <div>
   <h1>You clicked me {count} times!</h1>
  </div>
 );
}
```

接下来，让我们添加一个按钮和一些功能，使我们的计数器增加每次点击按钮。为此，我们将调用我们声明的另一个变量`setCount`，它是 function，类似于类组件中的`this.setState()`。让我们看看那是什么样子。

```
import React, {useState} from 'react';function Counter() { const [count, setCount] = useState(0);return (
  <div>
   <h1>You clicked me {count} times!</h1> <button onClick={() -> setCount(count + 1)}
    Click me!!
   </button>
  </div>
 );
}
```

这就是我们的计数器功能！如果一开始这一切都没有意义，那也没关系。这肯定是需要习惯的东西，并且再次从简单的功能开始，然后在此基础上进行构建。只是需要大量的练习！

# useEffect()挂钩

`useEffect()`钩子允许你在应用程序中执行副作用。该功能类似于`componentDidMount`、`componentDidUpdate`和`componentWillUnmount`。你可以把`useEffect()`想象成这三种功能的组合！整洁对吗？

在类组件中，你通常需要在这两个函数中有重复代码的`componentDidMount`和`componentDidUpdate`。相反，使用钩子，你只需调用`useEffect()`并传入你想要挂载的东西，react 就会自动知道何时更新它，何时卸载它。让我们用之前的例子来说明如何利用这一点。

```
import React, {useState, useEffect} from 'react';function Counter() { const [count, setCount] = useState(0); useEffect(() => {
  document.title = `You clicked me ${count} times!`;
})return (
  <div>
   <h1>You clicked me {count} times!</h1>
    <button onClick={() -> setCount(count + 1)}
    Click me!!
    </button>
  </div>
 );
}
```

我们传入`useEffect()`的函数就是效果。在我们的函数中，我们以文档的标题为目标。当 react 渲染这个组件时，它会记住我们在`useEffect()`中使用的效果，并在 DOM 更新后运行*。同样值得注意的是，传递给`useEffect()`的函数在每次渲染时都会有所不同，因为它总是拥有`count`的当前更新值。每次我们的应用程序重新渲染时，我们实际上是用预定的不同效果替换了之前的效果。*

## 使用 useEffect()清理

假设您正在使用 API 构建一个社交媒体应用程序，并且想要添加一个“添加朋友”功能。当使用类时，你必须使用`componentDidMount()`和`componentWillUnmount()`来改变状态，当一个用户成为另一个用户的“朋友”或“解除朋友”时。有了钩子，`useEffect()`让同时处理这两种功能变得更加容易。

```
import React, { useState, useEffect } from 'react';function Add(props) { const [isFriended, setIsFriended] = useState(null) const handleFriendChange = (status) {
  setIsFriended(status.isFriended);
} useEffect(() => {
  SocialMediaAPI.addFriend(props.friend.id, handleFriendChange) //This function will trigger when the component unmounts
  return () => {
   SocialMediaAPI.removeFriend(props.friend.id, handleFriendChange);
  }
 })
}
```

简单来说就是`useState()`和`useEffect()`！如果你想深入了解，了解其他类型的钩子，或者想学习如何创建自己的钩子，请查看下面的文档。黑客快乐！

[](https://reactjs.org/docs/hooks-intro.html) [## 介绍钩子-反应

### 钩子是 React 16.8 中的新增功能。它们允许您使用状态和其他 React 特性，而无需编写类。这个…

reactjs.org](https://reactjs.org/docs/hooks-intro.html) [](https://reactjs.org/docs/hooks-reference.html) [## 钩子 API 参考-反应

### 钩子是 React 16.8 中的新增功能。它们允许您使用状态和其他 React 特性，而无需编写类。这个…

reactjs.org](https://reactjs.org/docs/hooks-reference.html)