# 钩子中的顺序很重要的原因

> 原文：<https://blog.devgenius.io/the-reason-why-order-in-hooks-matters-2acd702526b3?source=collection_archive---------13----------------------->

![](img/144184eec512046093b5754e7737fce5.png)

由[劳塔罗·安德烈亚尼](https://unsplash.com/@lautaroandreani?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

React 钩子是 React 16.8 中的新特性。它们允许您在不编写类的情况下使用状态和其他 React 特性。它们是编写有状态组件的强大方法，也是编写功能组件的好方法。

然而，所有这些力量都是有代价的。它们有一些你应该遵守的约束，以使它们很好地工作，否则，你最终会有很多 bug。

今天我想谈谈一个具体的规则:

> *不要在循环、条件或嵌套函数中调用钩子。*

所以，简单地说，我们不能这样做:

```
import * as React from "react";

const Iron = ({ isMelted = false }) => {
  if (isMelted) {
    const [temperature, setTemperature] = React.useState(null);
  }

  return <div>{...}</div>;
};
```

或者更糟的是:

```
<button onClick={() => useRequest({ id: 12 })}>
  {n + 1}
</button>
```

有时候，阅读这条规则的人会应用它，而不会问太多为什么和如何应用的问题，如果你也是其中之一，那没关系，遵循文档而不深入并不可耻，但命运希望你在这里就是因为这个原因，所以我问你:你能告诉我为什么它如此重要吗？

在做任何解释之前，我希望你打开你的问题解决工具——大脑，我会给你五分钟时间想出一个解决方案，然后你可以浏览这篇文章来寻求启发！

你解决问题的过程如何？希望你找到了很酷的东西！让我们深入了解一下，实现我们自己的**使用状态**。

首发应用会是这个，你猜怎么着？另一个计数器…但是将自定义解决方案与真实解决方案进行比较会很有用。

```
import ReactDOM from "react-dom";
import { useState } from "react";

// The actual Component
export default function App() {
  const [counter, setCounter] = useState(10);
  const increment = () => setCounter(counter + 1);

  return (
    <div>
      <button onClick={increment}>{counter}</button>
    </div>
  );
}

ReactDOM.render(<App />, document.getElementById("root"));
```

> *我们将使用 React 17，因为 18 版本中的新渲染逻辑不能很好地与“自制”解决方案一起工作，但你知道，这只是一个实验😇*

# 我们的自定义使用状态

我们的目标是调用自定义的 useState，而不是真实的 useState，让我们揭开这个钩子的行为的神秘面纱:

*   可以接受带有状态初始值的参数。
*   它返回一个具有实际值的元组和一个更新该值的函数。
*   一旦状态被更新，它触发组件的重新呈现，保持更新的值。

所以我们要做的第一件事是用一些基本的占位符来声明我们的函数，并注释真正的函数💅

```
// import { useState } from "react";

function useState(initialValue) {
  const setValue = (newValue) => {};
  const tuple = [initialValue, setValue];
  return tuple;
}
```

太好了，现在没有东西崩溃了，但它也不起作用…我们的 setValue 函数什么也不做。我们需要给她实际的功能，但是你可能会注意到这里的一个问题:状态是如何存储在函数中的？

我的意思是，每个人都知道 React 组件只是函数，对吗？React 本身调用这些触发组件渲染的函数，但是对于每一次对`App`组件的新调用，我们都会初始化一个全新的 useState 函数。

```
App(); // A new useState is invoked
App(); // A new useState is invoked
App(); // A new useState is invoked
```

因此，为了解决这个问题，我们需要一个外部变量来存储我们的钩子声明！姑且称之为**状态**。

```
// This variable will be persistent between renders!
let state = [];

function useState(initialValue) {
  const setValue = (newValue) => {};
  const tuple = [initialValue, setValue];
  return tuple;
}
```

现在是时候实现钩子的核心逻辑了，初始版本可能是这样的:

```
let state = null;

function useState(initialValue) {
  if (state && state[0]) {
    return state;
  }

  const setValue = (newValue) => {
    state[0] = newValue;
    customRender(); // Who am I?
  };

  state = [initialValue, setValue];

  return state;
}
```

让我们来分解一下行为:在初始调用时，useState 将检查在 States 数组的特定索引处是否已经有了什么，如果是，它将返回它，否则它用元组填充 state 变量并返回它。

```
// First Render: Initialize with the Tuple
// Second Render: State is not null, so returns it.
// Third Render: State is not null. so returns it.
// Continue Infinitely...
```

> *但是 the 雷纳托，我迫不及待地尝试了这段代码，但什么都没用。你在开玩笑吗？！？*

仔细看前面的代码片段，看到`customRender`函数调用了吗？这是我们在 react 中模拟重新渲染的奇怪技巧。简单地说，我们创建一个包装了`ReactDOM.render()`调用的函数，并在设置新值时调用它。

```
// Wrap the render function into a function.
function customRender() {
  ReactDOM.render(<App />, document.getElementById("root"));
}

// Don't forget to call it immediately, we need our initial render :)
customRender();
```

如果你尝试这段代码，你会发现它实际上和真实的一样，我会把沙箱留在这里。

酷，现在是时候让一切爆炸了！

看看我放在这里的这个新沙箱:

你能发现这个漏洞吗？这并不酷…每个按钮都有相同的状态值🥲，也许是时候更好的实现了！

# 是时候更好地实现了！

第一个明显的问题是，我们的 **state** 变量接受单个值，所以它需要成为一个数组，此外，我们需要一种方法来跟踪我们的`useState`调用的索引，因为，对于每个状态，将有不同的值！

在这里，你可以找到两个不同的按钮，最终享受自己的价值工作版本！

# 我们问题的答案

到目前为止，我们问自己为什么钩子的顺序很重要，我希望现在你自己已经找到了答案。

原因很简单，就是这个变量:

```
const states = []; // I'm a bad Guy 😙
```

尽管这是一个非常幼稚的实现，但 internally react 的工作方式与此类似。每个钩子定义都存储有一个特定的索引，因此 React 依赖它来返回正确的值。

正如我们在第一个例子中看到的，这就是这样做不正确的原因:

```
import * as React from "react";

const Iron = ({ isMelted = false }) => {
  // Sometimes the index can be zero, sometimes not?
  // There is no consistency between renders!
  if (isMelted) {
    const [temperature, setTemperature] = React.useState(null);
  }

  return <div></div>;
};
```

您可能还会发现 React 常见问题解答中的这个答案很有用:

> ***React 如何将钩子调用与组件关联起来？***
> 
> *每个组件都有一个内部“存储单元”列表。它们只是 JavaScript 对象，我们可以在其中放置一些数据。当你调用一个类似 useState()，* ***的钩子时，它读取当前单元格(或者在第一次渲染时初始化它)，然后将指针移动到下一个*** *。这就是多个 useState()调用各自获得独立的本地状态的方式。*

*最初发布于*[*https://renatopozzi . me*](https://renatopozzi.me/articles/the-reason-why-order-in-react-hooks-matters)*。*

[](https://www.linkedin.com/in/itsrennyman/) [## 雷纳托·波齐|职业简介| LinkedIn

### 把善良和知识带给其他人。我是一名软件开发人员，目前在意大利米兰，🇮🇹，我曾经…

www.linkedin.com](https://www.linkedin.com/in/itsrennyman/)