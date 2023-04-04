# React 挂钩简介—基本挂钩

> 原文：<https://blog.devgenius.io/introduction-to-react-hooks-e49738432f54?source=collection_archive---------11----------------------->

## 用 React 钩子构建 web 应用程序的快速指南。

![](img/b4f75fa2598d2b71018b43c262d6681e.png)

马文·迈耶在 [Unsplash](https://unsplash.com/s/photos/web-deveopment?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

React 钩子从 React 16.8 开始可用，从版本 0.59 开始支持 React Native。通常，我们知道 React 类组件是有状态的，而函数组件是无状态的。当有状态组件可以保持状态并处理数据变化时，无状态组件在功能上既没有状态也没有逻辑，而是通过 props render React 元素获取数据。

所以，一般反应应用程序，父类组件保持状态并根据逻辑变化。然后，子功能组件通过 props 传递数据，并在不做任何更改的情况下呈现它们。

但是不再需要使用 React 类组件。使用带有 React 挂钩的功能组件…

**什么是反应钩子**

> 钩子是让你从功能组件中“钩入”反应**状态**和**生命周期特性**的功能。钩子在类内部不起作用——它们让你在没有类的情况下使用 React。[来源](https://reactjs.org/docs/hooks-state.html)

根据上述语句 React Hooks 启用功能组件中的**状态**和**生命周期特性**。通过使用 React 钩子，你可以使用所有的特性而不需要一个类。你也不需要修改任何已经写好的代码来移动钩子。您可以在组件之间重用有状态逻辑，而无需改变它们的组件层次结构。也不需要使用生命周期方法，如*componentdimount()*， *componentDidUpdate()* 来执行副作用，使代码重用和代码组织更加困难。

这里我将讨论基本**基本挂钩**和**定制挂钩**的比较类的组件。

基本钩子:
使用状态
使用效果
使用上下文

**使用状态**

我们可以使用类和函数来编写 React 组件。首先，我编写 App 类组件。这里，显示的名称根据输入文本的改变而改变。这很简单，做起来…

图 1

现在，试着把上面的任务写成一个函数组件。我想添加“名称”状态变量的当前值，另一个是设置名称状态变量的函数“setName”。但是我很纠结(图 2 中的第 5 行和第 6 行),因为函数组件是无状态的？？

图 2

用钩子解决这个问题。*使用状态*从反应本地状态到功能组件解决问题。传递“helloWorld”来初始化状态名。这里，*使用状态*是一个钩子，它是 React 中的一个函数。*使用状态*可以在一个函数中多次使用

```
 import React, {*useState*} from ‘react’
 const [name, setName]= *useState*(‘helloWorld’); 
```

图 3

**使用效果**

类有生命周期方法，从 React 组件的诞生到死亡，一系列事件执行这些方法。任何组件都会经历安装、更新和卸载。我们使用一些循环方法如*componentidmount()*，*componentiddupdate()*， *componentWillUnmount()* 等在类组件中执行副作用。像数据获取、订阅或从 React 组件手动更改 DOM 这样的操作被称为副作用。在下面的代码中添加了*componentdimount()*， *componentDidUpdate()* 来随着输入名称的改变而改变文档标题。

图 4

render()方法是纯的，没有副作用，因为它们会影响其他组件。那么如何在函数组件中实现呢？*使用效果*挂钩习惯了。在初始渲染和每次更新后都运行。 *useEffect* 也是不止一次使用。

```
useEffect(()=>{
    document.title = name
});
```

图 5

**使用上下文**

Context API 提供了一种简单明了的方法来在组件之间共享状态，而不需要 React 本身传递属性。在这里浏览我以前的文档，学习更多关于上下文 API 的知识。 *useContext* hook 接受 Context 对象，其值从 *React.createContext()返回。*

在示例*中，ColorContext* 的默认值为 light。

```
const colors = {
 light: ‘white’,
 dark: ‘black’,
 danger: ‘red’
}const ColorContext = React.createContext(colors.light);
```

*Home* 函数 *useContext* 接受 *ColorContext。*

```
function Home(){
 const color = useContext(ColorContext);
 return(
  <text style={{color: color.danger}} >Hello world</text>
 )
}
```

然后是 *< ColorContext。提供者>* 提供了访问上下文的能力，上下文是从它包装的(这里是**的家)**。它提供了向下传递给所有组件的数据和函数。

```
export default function App() {return (
 <ColorContext.Provider value={colors}>
  <Home/>
 </ColorContext.Provider>
 );
}
```

完整的代码，

**定制挂钩**

> 自定义钩子是一个 JavaScript 函数，它的名字以“use”开头，可以调用其他钩子。[来源](https://reactjs.org/docs/hooks-custom.html)

定制钩子的目标是防止复制逻辑。自定义挂钩允许在组件之间重用有状态逻辑，而无需添加更多组件。这段代码中的逻辑是独立于 *useNameInput()* 和 *useDocumentTitle()* 函数的。现在它们是钩子。现在自定义钩子完全独立于状态本身。所以现在你可以在一个组件中多次使用定制钩子。

图 6

**挂钩规则**

有两个规则来挂钩另外使用的 JavaScript 函数规则。

> 只调用顶层的钩子**。不要在循环、条件或嵌套函数中调用钩子。**
> 
> 仅从 React 函数组件调用挂钩**。不要从常规的 JavaScript 函数中调用钩子。[来源](https://reactjs.org/docs/hooks-rules.html)**

**结论**

在本文中，我们讨论了 React 钩子的基本概念以及 React 引入钩子的原因。然后用钩子构建功能组件，并与类组件进行比较。主要讨论*使用状态*、*使用效果*、*使用上下文*和自定义钩子以及钩子的规则。我想这篇文章对那些进入 React Hooks 的人会有所帮助。接下来，我们将讨论:

[](https://medium.com/dev-genius/introduction-to-react-hooks-usereducer-7b87a6cb4289) [## React 挂钩简介— useReducer

### 使用 React Hooks 构建 web 应用程序的快速指南。

medium.com](https://medium.com/dev-genius/introduction-to-react-hooks-usereducer-7b87a6cb4289) [](https://medium.com/dev-genius/performance-optimization-with-react-hooks-usecallback-usememo-f2e527651b79) [## React 挂钩的性能优化—使用回调和使用备忘录

### 使用 useCallback & useMemo 钩子优化 web 应用程序性能的介绍。

medium.com](https://medium.com/dev-genius/performance-optimization-with-react-hooks-usecallback-usememo-f2e527651b79) 

**参考文献**

[](https://reactjs.org/docs/hooks-intro.html) [## 介绍钩子-反应

### 钩子是 React 16.8 中的新增功能。它们允许您使用状态和其他 React 特性，而无需编写类。这个…

reactjs.org](https://reactjs.org/docs/hooks-intro.html)