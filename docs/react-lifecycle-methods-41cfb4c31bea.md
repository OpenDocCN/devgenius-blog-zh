# 反应生命周期方法

> 原文：<https://blog.devgenius.io/react-lifecycle-methods-41cfb4c31bea?source=collection_archive---------9----------------------->

![](img/d83064935042661cfdb0f3e0fb3eb5fc.png)

如果你正在学习 React，刚刚进入类组件的世界，你一定遇到过“生命周期方法”这个词。本文将探索这些生命周期方法。如果你像我以前一样从事过 android 开发，生命周期方法可能对你来说有些熟悉，类似于 android 中的应用程序生命周期。但是如果你还没有，不要担心，我们试着一起一个一个的探索，爬上眼前的这座小山峰。

# 什么是生命周期方法？

当我们使用类组件时，React 会提供给我们一些方法，这些方法如果在我们的应用程序中定义，会在不同的预定义时间点自动执行。
其中最常见的一种是渲染，你现在一定碰到过这个。我们可以简单地将这些生命周期方法分为三类，安装、更新和卸载。我们将逐一探讨每一个类别。

## *安装*

当组件被挂载时，挂载类别下的方法被调用。这些方法是:
1。构造函数()
2。render()
3。componentDidMount()

根据 React 官方文档，前三种方法是最常用的安装方法。当一个组件被安装时，这些方法按上述顺序被调用。下面是启动基本 React 应用程序时的控制台输出。[本文使用的代码附在最后]

> 构造函数被调用
> 渲染被调用
> 组件被调用挂载

## 构造器

正如预期的构造函数类似于我们在编程语言中已经看到很久的构造函数，通常用于初始化状态或其他在以后需要的变量。我们通常避免在构造函数中进行任何数据密集型调用，这并不是说它有任何影响，只是它符合 React 社区编码标准。如果我们不必初始化 state 或 prop，我们可以选择省略定义构造函数的方法。

```
constructor(props){
   state = {latitude:null, variable2:null);
   this.prop.variable3 = undefined;
}
```

虽然我们将构造函数放在“安装”类别下，但是构造函数实际上是在组件被安装之前被调用的。

## 提供；给予

render()是我们的老朋友，只是简单地用来返回 JSX 内容。这是类组件所需的唯一方法，其余的方法可以根据需要使用。当我们使用“setState”更新状态时，也会调用它。当我们从 ComponentDidMount()函数获得状态更新时，将再次调用 render 函数，这一次,“纬度”变量的更新值将显示在屏幕上。

```
render(){
 return <div> {this.state.latitude} <div>
```

## 组件安装

对于数据/时间密集型调用，我们通常使用 componentDidMount()，假设您希望获得用户的位置，此方法将是进行该调用的好选择。

```
componentDidMount()
{
window.navigator.geolocation.getCurrentPosition(
(position) => {
this.setState({latitude:position.coords.latitude})
},
(err)=> {
this.setState({errorMessage:err.message})
})
}
```

在上面的代码片段中，我们从地理位置 API 获取位置，并将状态变量“latitude”更新为获取的值。在构造函数和呈现方法之后调用此方法。

# 更新

当状态或属性被更新时，一些生命周期方法被调用，这些方法在下面被提及:
1。shouldComponentUpdate()
2。【T2 渲染()】3。componentDidUpdate()

当组件更新被触发时，这些函数按上述顺序被调用。

## shouldComponentUpdate

顾名思义，shouldComponentUpdate 处理组件是否应该更新。默认情况下，它返回值 true，如果声明为 false，将导致 render 和 componentDidUpdate 等后续函数不会被触发。

```
shouldComponentUpdate()
{
 return false;
}
```

如果返回 true，则在 shouldComponentUpdate 之后调用 render。这是我们在安装部分已经讨论过的内容。

## componentDidUpdate

顾名思义，这个函数在组件更新时被调用。例如，如果您更新了一个状态的值，那么这个函数将被调用。componentDidUpdate 通常也适用于需要某种状态更新的数据密集型调用。

```
componentDidUpdate(){
console.log(‘ComponentDidUpdate called’);
}
```

当我们获取纬度并更新它时，我们从上面的函数 componentDidMount 更新状态，一旦我们更新了状态，componentDidUpdate 就会被调用。

来自控制台的输出(在函数 componentDidMount 中添加了控制台日志语句)

> index.js:17 componentDidMount 被调用
> index . js:21 ComponentDidUpdate 被调用

这是我们在使用生命周期方法时遇到的最常见的方法。如果你觉得这有帮助，别忘了鼓掌。

完整的代码片段:【https://github.com/archi14/Lifecycle-methods 