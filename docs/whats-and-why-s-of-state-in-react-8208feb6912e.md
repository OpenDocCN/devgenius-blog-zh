# 反应的状态是什么和为什么。

> 原文：<https://blog.devgenius.io/whats-and-why-s-of-state-in-react-8208feb6912e?source=collection_archive---------6----------------------->

![](img/09d59ff6f750afadea3abfb888ee7d7d.png)

马文·迈耶在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

> **状态，在 React 中经常被称为组件的内存，是一个普通的 JavaScript 对象，React 使用它来表示关于组件当前情况的一些信息。换句话说，它描述了组件的当前状态，无论是一些数据还是 UI。**

组件的状态可以随着时间的推移而更新，比如一个模态可以打开或关闭，我们输出的数据可以改变。基于用户交互，组件应该改变屏幕上显示的内容。

## 为什么是状态变量而不是局部变量？

我们不能使用任何局部变量有两个主要原因。

1.  局部变量不会在渲染之间持续(保留数据)。【React 第二次渲染一个组件时，会从头开始渲染。它不考虑局部变量的任何变化。
2.  **局部变量的改变不会触发渲染。React 没有意识到它需要用新数据再次呈现组件。**

## 状态变量如何保存数据？

作为一个组件的内存，状态不像一个常规变量，在你的函数返回后就消失了。这个状态实际上“T10”生活在“T11”之中——就像在一个架子上一样！—在您的功能之外。当 React 调用您的组件时，它会为您提供该特定渲染的状态快照。您的组件返回 UI 的快照，在它的 JSX 中有一组新的道具和事件处理程序，所有这些都是使用该呈现的状态值计算的**!**

为了用新数据更新组件，需要做两件事:

1.  **保留**渲染之间的数据。
2.  **触发**反应用新数据重新渲染。

**useState** 钩子提供了这两件事:

1.  一个**状态变量**来保持渲染之间的数据。
2.  一个**状态设置器函数**更新变量并触发 React 再次呈现组件。

## 使用 useState 钩子来设置和更新状态。

useState 挂钩返回一个数组，该数组包含两项，一个状态变量和一个状态设置函数。当您调用 useState 时，您是在告诉 React 您希望这个组件记住一些东西。

**举例。** 让我们来看一个例子，其中我们有一个状态变量**计数器**，一个状态设置函数 **setCounter** 和几个增加和减少计数器的按钮，每个按钮都与各自的事件处理程序及其功能相关联。

```
import { useState } from "react"
export default function App(){
  const [ counter , setCounter ] = useState(0)
  function handleIncrement(){
    setCounter(counter + 1)
  }
  function handleDecrement(){
    setCounter(counter - 1)
  }
  return(
    <div>
     <button onClick={handleIncrement}>Increment</button>
     <button onClick={handleDecrement}>Decrement</button>
    </div>
  )
}
```

查看这个例子的工作演示。

## 多个状态变量。

为了对关注点进行适当的分离，如果它们的状态是不相关的，那么拥有多个状态变量是一个好主意。

```
const [ index, setIndex ] = useState(0)
const [ showMore, setShowMore ] = useState(false)
```

但是，如果你发现你经常一起改变两个状态变量，把它们合并成一个可能会更好。

```
const [ person, setPerson] = useState({
  firstName:'Gwen',
  lastName:'Stacy'
}) 
```

## 结论

*   当组件需要在渲染之间“记住”一些信息时，使用状态变量。
*   通过调用 **useState** 钩子来声明状态变量。
*   钩子是以**开始使用**的特殊功能。它们让您“挂钩”React 特性，如状态。
*   **useState** 钩子返回一对值:当前状态和更新它的函数。
*   您可以有多个状态变量。

## 资源

react 团队最终推出了最新的 [react 文档](https://beta.reactjs.org/)(测试阶段)，其中包含交互式示例、图表以及 React 入门所需的几乎所有内容。

如果你喜欢这样的文章，可以看看我的 [hashnode](https://hashnode.com/@jayprakash07) 简介和我的[博客网站](https://jaykalia.netlify.app/)。

感谢阅读🙏