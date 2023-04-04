# 在 React 中使用钩子

> 原文：<https://blog.devgenius.io/working-with-hooks-in-react-c1974693241c?source=collection_archive---------16----------------------->

# 介绍

钩子是 React 中的一个新特性，它让你不用写类就可以使用状态和其他 React 特性。挂钩是让您从功能组件“挂钩”React 状态和生命周期特性的功能。

在 React 发布钩子之前，您只能使用类组件中的生命周期方法来操作或更新`state`,但是使用钩子允许您在不编写类的情况下利用 React 的强大功能。钩子提供了一种在组件之间重用功能的强大而富有表现力的新方法。

本文将涵盖从基础到高级的钩子概念，在本文结束时，您将理解何时以及如何在您的应用程序中使用不同类型的 React 钩子。

# 钩子的规则

有指导钩子使用的规则，为了避免你的代码抛出错误，这些规则必须被遵守。

*   钩子必须在函数或组件体内部被调用。
*   不要有条件地、在循环内部或嵌套函数中调用钩子。
*   命名钩子时，应该以前缀“use”开头。这适用于自定义挂钩和 React 提供的挂钩。
*   当使用钩子时，组件必须是大写的。

# 使用挂钩

React 提供了在应用程序中使用钩子的不同方式。这意味着我们可以在应用程序中使用不同的钩子，这取决于我们想要达到的目标。React 也给了我们创建定制钩子的能力，这允许我们重用有状态逻辑。是不是很牛逼！！！

让我们从看一看 React 中的不同挂钩开始

*   使用状态挂钩

useState 挂钩用于管理和更新 React 应用程序中的`state`。它给了我们一个数组，这个数组由两个值组成:T2 和 T3，这个 setter 函数是我们用来更新状态的。useState 钩子也接受默认状态的值。

示例:

```
import React, { useState } from "react";

const App = () => {

  const [text, setText] = useState(false);

  return (
    <div>
      <h2>{text === true ? "Sign In" : "Sign Up"}</h2>
      <button onClick={()=>setText(!text)}>Change Text.</button>
    </div>
  );
};
export default App;
```

在上面的例子中，每当我们点击`h2` 元素时，`button` 就会更新它的内容，因为有了 useState 钩子，这才有可能。

首先，我们初始化一个名为`text`的新状态变量和一个名为`setText`的 setter 函数，然后将这两个变量赋给 useState。

然后我们返回两个元素，`h2` 元素保存状态本身的值，我们声明一个条件语句来检查状态`text`是否等于 true。如果是，我们希望显示`Sign In`或`Sign Up`

button 元素接受一个 onClick 属性，该属性的值是我们用来更新状态的 setter 函数。每次我们点击这个按钮，React 都会重新渲染组件，从而将正确的值传递给`h2`元素。

*   使用效果挂钩

useEffect 挂钩允许您在函数中执行副作用。副作用包括:从 API 获取数据、改变 DOM 元素、将内容推入数组等。

useEffect 执行 componentDidMount、componentDidUpdate 和 componentWillUnmount 组合的功能。

默认情况下，useEffect 在组件中每次重新渲染后运行，大多数时候我们只希望 useEffect 在挂载时运行。为了解决这个问题，我们必须在 useEffect 中包含一个依赖数组，这在我们想要从 API 获取数据的情况下非常方便。我们不希望每次组件重新渲染时都调用 API，我们只想在 useEffect 安装后立即调用它。

```
useEffect ( () => {
  //do something here
}, [])
```

让我们通过一个例子来看看 useEffect 是如何工作的:

```
import React, { useState, useEffect } from "react";

const App = () => {
  const [products, setProducts] = useState({});

  const fetchData = async () => {
    const response = await fetch("https://fakestoreapi.com/products?limit=5");
    const data = await response.json();
    setProducts(data)
  };

  useEffect(() => {
    fetchData();
  }, []);
  return (
    <div className="App">
      {products.map(({image, title, price,id})=>(
        <div key={id} className="container">
        <img src={image} alt="store" />
        <p>{title}</p>
        <p>{price}</p>
        </div>
      ))}

    </div>
  );
};

export default App;
```

在上面的例子中，我们从一个假的 API 获取数据，映射从 API 得到的结果，最后返回结果。

*   useReducer 挂钩

useReducer 挂钩是一个用于状态管理的挂钩。您可能想知道为什么 useReducer 钩子执行与 useState 钩子相同的功能。

useReducer 挂钩和 useState 挂钩都用于状态管理，但它们的使用取决于应用程序的大小。当您有涉及多个子值的复杂状态逻辑时，useReducer 通常比 useState 更可取。它还允许您优化触发深度更新的组件的性能，因为您可以向下传递分派而不是回调。

useReducer 钩子语法与 useState 非常相似，只是它接受两个参数，即`reducer function`和`initial state`。初始状态的值可以是任何值，这取决于我们试图初始化什么。它可以是数字、函数、数组、对象等。

让我们来看看如何通过构建一个简单的计数器在我们的代码中实现 useReducer 挂钩的演示。

```
import React, { useReducer } from "react";

function reducer(state, action) {
  switch (action.type) {
    case "increment":
      return state + 1;
    case "decrement":
      return state - 1;
    default:
      return state;
  }
}

const App = () => {
  const [count, dispatch] = useReducer(reducer, 0);
  return (
    <div className="App">
      <h1>Count: {count} </h1>
      <button onClick={() => dispatch({type:"decrement"})}>-</button>
      <button onClick={() => dispatch({type:"increment"})}>+</button>
    </div>
  );
};
export default App;
```

首先，我们声明一个保存当前状态和`dispatch`函数的数组，然后将它分配给 useReducer 钩子。useReducer 钩子接受两个值，它们是 Reducer 函数和初始状态。在这种情况下，初始状态为 0。

接下来，我们返回从函数返回值的 JSX 元素。`h1`元素保存了`count`的值，以及增加和减少`count`值的两个按钮。每个按钮都有一个`onClick`属性，我们将传递`dispatch`函数给它。 `The dispatch`函数还包含一个参数，表示我们想要执行的动作类型，在本例中，我们想要递增和递减。

最后，我们在 main 函数之外声明我们的 reducer 函数。该减速器函数接受两个参数`state`和`action`。现在我们想使用一个 switch 语句来检查已经在`dispatch`函数中声明的动作类型。因此，如果动作的类型是递增，我们希望状态增加 1，如果是递减，我们希望状态减少 1，否则什么都不会发生。

*   使用上下文挂钩

useContext 挂钩允许我们使用 React 的上下文 API，它本身是一种机制，允许我们在它的组件树内共享数据，而无需通过 props。它基本上消除了道具钻井！

useContext 挂钩接受 Context 对象，该对象是从 React.createContext 返回的值，并返回当前上下文值，该值由给定上下文的最近上下文提供程序提供。

让我们通过构建一个随机密码生成器来看看它是如何工作的

```
import React,{useContext, createContext, useState} from "react";

const RandomPasswordContext = createContext();
const SetRandomPasswordContext = createContext();

function RandomPassword() {
  const randomPassword = useContext(RandomPasswordContext);
  return <div>Password: {randomPassword}</div>;
}

function PasswordGenerator() {
  let text =""
  let characters = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";
  const setRandomNumber = useContext(SetRandomPasswordContext);
  const generateRandomNumber = () => {
      for (let i = 1; i < 7; i++){
    setRandomNumber (text+=characters.charAt(Math.floor(Math.random() * characters.length)));
  }
};
  return <button onClick={generateRandomNumber}>Generate Random Password</button>;
}

const App = () => {
  const [randomNumber, setRandomNumber] = useState(0);

  return (
    <div>
      <RandomPasswordContext.Provider value={randomNumber}>
        <SetRandomPasswordContext.Provider value={setRandomNumber}>
          <RandomPassword />
          <PasswordGenerator />
        </SetRandomPasswordContext.Provider>
      </RandomPasswordContext.Provider>
    </div>
  );
}

export default App
```

*   useRef 挂钩

useRef 钩子返回一个可变的 Ref 对象，其中`.current`属性被初始化为传递的参数(初始值)。这个钩子使得访问功能组件中的 DOM 节点成为可能。我们可以如下使用它:

```
import React, {useRef} from "react";

const refContainer = useRef(initialValue)
```

这个钩子用于处理 react 中对元素和组件的引用。我们可以通过向元素传递 ref 属性来设置引用。

让我们看看如何使用 useRef 钩子的最常见的例子

```
import React, { useEffect, useRef } from "react"

const App = () => {
  const inputEl = useRef(null);
  useEffect(() => {
    inputEl.current.focus();
  }, []);
  return <input ref={inputEl} type="text" />;
}

export default App;
```

在上面的例子中，我们正在访问 input 元素，我们希望在组件安装后立即关注 input 元素。

*   使用回调挂钩

这个钩子允许我们传递一个内联回调函数和一组依赖项，并将返回回调函数的记忆版本。当我们想防止在每次渲染时都创建一个函数时，这是很有用的。

```
import React, {useState} from "react";

const Counter = ({increment}) => {
  return <button onClick={increment}>increase</button>;
};
const App = () => {
  const [count, setCount] = useState(0);
  return (
    <div>
      <p>Count : {count}</p>
      <Counter increment={()=>setCount(count + 1)} />
    </div>
  );
};

export default App;
```

在上面的例子中，每次 App 组件刷新时，都会呈现`setCount`函数。我们可以使用`useCallback`挂钩来防止这种情况发生。

```
import React, { useState, useCallback } from "react";

const Counter = ({increment}) => {
  return <button onClick={increment}>increase</button>;
};
const App = () => {
  const [count, setCount] = useState(0);
  const increment = useCallback(()=>{
    setCount(c => c + 1)
  },[setCount])
  return (
    <div>
      <p>Count : {count}</p>
      <Counter increment={increment} />
    </div>
  );
};

export default App;
```

当向优化的子组件传递回调时，useCallback 钩子很有用。它的工作方式类似于 useMemo 钩子，但是用于回调函数。

*   使用备忘录挂钩

记忆化是一种优化技术，其中函数调用的结果被缓存，然后在相同的输入再次出现时被返回。useMemo 钩子允许我们计算一个值并记忆它。我们可以如下使用它:

```
import { useMemo } from 'react'

  const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b])
```

当我们想要避免重复执行昂贵的操作时，useMemo 钩子对于优化是有用的。

*   useLayoutEffect

这个钩子与 useEffect 相同，但是它只在所有文档对象模型(DOM)突变后触发。我们可以如下使用它:

```
import { useLayoutEffect } from 'react'

  useLayoutEffect(didUpdate)
```

useLayoutEffect 钩子可以用来从 DOM 中读取信息。

尽可能使用 useEffect 挂钩，因为 useLayoutEffect 会阻止可视化更新并降低应用程序的速度。

所以这些都在“在 React 中使用钩子”中，以后我会有更多这样的文章。

在那之前，请继续关注我！