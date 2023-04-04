# React 挂钩简介— useReducer

> 原文：<https://blog.devgenius.io/introduction-to-react-hooks-usereducer-7b87a6cb4289?source=collection_archive---------10----------------------->

## 使用 React Hooks 构建 web 应用程序的快速指南。

![](img/1bf691bcf7c761c9400704046d1adc3e.png)

费伦茨·阿尔马西在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

React 钩子从 React 16.8 开始可用，从版本 0.59 开始支持 React Native。在我之前的文章中讨论了关于 React 的**基本钩子**。如果你是新手，点击 [**这里的**](https://medium.com/dev-genius/introduction-to-react-hooks-e49738432f54) 来阅读。在本文中，我将讨论 React 上的附加挂钩中的 *useReducer* 。

React 中有两种类型的内置挂钩，即基本挂钩和附加挂钩。

**基本钩子**
*useState*
*use effect*
*use context*
**附加钩子**
*useReducer*
*use callback
useMemo
useRef
useImperativeHandle
useLayoutEffect
useDebugValue*

**什么是 useReducer？**

> useState 的替代方案。接受(state，action) => newState 类型的 reducer，并返回当前状态和分派方法。[来源](https://reactjs.org/docs/hooks-reference.html#usereducer)

*useReducer* 是 React 中内置的钩子，用于 React 中的状态管理。 *useReducer* 是*使用状态*的替代。它与减速器功能有关。如果您对 Redux 状态管理有清晰的认识，那么您已经对它的工作原理有了足够的了解。点击 [**此处**](https://medium.com/dev-genius/getting-started-with-react-native-redux-479fb6e37be4) 了解 Redux。

为什么我们使用 useReducer 而不是 useState？

*使用状态*，当状态值是原语时，状态值只在单个组件内部使用或者逻辑简单。

使用 useReducer ，当我们有复杂的状态时，比如涉及到对象或数组，下一个状态依赖于前一个状态，组件间的数据共享和更新状态更加复杂。useReducer 针对触发深度更新的组件进行了性能优化，因为您可以向下传递分派，而不是回调。

现在，我将实现一个简单的计数器 web 应用程序，并讨论 *useReducer 的特性。*

**第一步。定义初始状态和减速器功能。**

reducer 函数接受两个参数，即当前状态和动作，然后返回一个值，即新状态。一个*动作*参数 *i* 给出了执行减速器功能的指令。这里，*增量*、*减量*和*复位*是对减速器功能的给定指令(*动作*)。

```
const initialState = {
 value: 0
}const reducer = (state, action) => {
 switch(action.type){
  case ‘INCREMENT’ :
   return {value: state.value + 1 };
  case ‘DECREMENT’ :
   return {value: state.value — 1 };
  case ‘RESET’ : 
   return initialState;
  default:
   return state; 
 }
}
```

**第二步。useReducer 挂钩**

首先，我们需要将 *useReducer* 从 React 本地状态导入到函数组件中。

```
**import** React, { ***useReducer***} from ‘react’;
```

*状态*、*初始化器参数*和*初始化器*是 useReducer 的输入参数。我们可以通过两种不同的方式初始化 *useReducer* 状态。

**2.1 指定初始状态**

第一种方法是，将初始状态作为第二个参数传递。这是最简单的方法。这里 *useReducer* 接受两个参数，即*减速器*的功能和初始状态。

```
**const** [count, dispatch] = ***useReducer***(reducer, initialState);
```

*count.value* 打印该值，并用动作的分派类型进行更新。完整的代码，

图 1 (App.js)

**2.2 惰性初始化**

> React 不使用 Redux 推广的 state = initialState 参数约定。[来源](https://reactjs.org/docs/hooks-reference.html#usereducer)

另一种方法是，懒洋洋地创建初始状态。在这里，传递一个 *init* 函数作为第三个参数。用这种方法我们可以还原外面的初始状态。 *useReducer* 接受初始渲染时调用的初始动作的可选第三个参数。

在下面的代码中，我将初始值 0 传递给计数器函数作为 props since App 函数组件。

图 2 (App.js)

**结论**
在本文中，我们讨论了关于 *useReducer* 钩子、 *useState* 和 *useReducer* 的区别，并使用 React 钩子实现了简单的计数器 web 应用。我想这篇文章对那些进入 React Hooks 的人会有所帮助。

**参考文献**

[](https://reactjs.org/docs/hooks-reference.html#usereducer) [## 钩子 API 参考-反应

### 钩子是 React 16.8 中的新增功能。它们允许您使用状态和其他 React 特性，而无需编写类。这个…

reactjs.org](https://reactjs.org/docs/hooks-reference.html#usereducer)