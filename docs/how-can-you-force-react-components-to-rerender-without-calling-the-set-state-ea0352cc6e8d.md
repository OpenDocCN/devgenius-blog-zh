# 如何在不调用 set 状态的情况下强制 react 组件重新呈现？

> 原文：<https://blog.devgenius.io/how-can-you-force-react-components-to-rerender-without-calling-the-set-state-ea0352cc6e8d?source=collection_archive---------13----------------------->

![](img/84243d75ba03fcbaa709527a09860a4c.png)

每当 React 组件的道具或状态发生一些变化时，react 组件会自行重新渲染。简单地从代码中的随机位置更新状态，会导致用户界面(UI)元素自动重新呈现。

在类组件中，您可以选择调用 force update 来强制重新呈现。然而，在函数组件中，没有强制更新的机会，因为没有对等物，但是您可以选择在 useState 钩子的帮助下设计一种强制更新的方法。必须尝试并避免强制更新，因为它偏离了反应思维模式。React 文档列举了一些何时可以使用强制更新的例子。

默认情况下，当组件的状态或属性发生变化时，组件将重新呈现。但是，如果存在隐式更改，如对象内部深层数据的更改，而对象本身也没有更改，或者如果您的渲染方法依赖于另一个数据，您可以选择告诉 React 只需通过调用 force update 来重新运行渲染。

# 强制更新:

然而，有人提出了一个想法，对于深度嵌套的对象，强制更新是必要的。在不可变数据源的帮助下，跟踪变化变得很便宜。变化总是会导致新的对象。因此，我们只需要检查对象引用是否已经改变。您甚至可以使用库不可变 JS 来将不可变数据对象实现到应用程序中。

通常，您必须尽量避免使用强制更新，并且应该只从这里读取。道具还有这个。呈现中的状态。这使得 react 组件变得“纯粹”,应用程序变得更加容易，同时也非常高效。更改要重新呈现的元素键将会起作用。您必须通过 state 在元素上设置 key prop，然后在您想要更新时将状态设置为具有新的键。

通过这样做，发生了一个变化，然后你需要重置这个键。setState ({key: Math。random })；您必须注意，这将帮助您替换正在更改密钥的元素。例如，当上传图像后，您希望重置一个文件输入字段时，它会非常有用。

# 检查您的代码:

此外，您必须注意，如果您正在使用强制更新，您可能想要检查您的代码，并检查是否有任何其他方法来做同样的事情。更改密钥)将完全替换该元素。如果您更新密钥以带来所需的更改，您可能会在代码中的某个地方遇到问题。因此利用数学。随机 n 键可以帮助您在每次渲染时重新创建元素。不建议以这种方式更新键，因为 react 使用键来有效地确定重新渲染事物的最佳可能方式。

React 开发人员努力解决应用程序中不必要的重新渲染组件。当一个组件不断在后台更新其数据，从而导致整体性能发生变化时，我们都经历过这种情况。

# 渲染的快速说明:

React 的 createElement 函数有助于根据给定的元素类型创建和返回新元素。所有的更新都会在需要的时候自动完成。现在让我们看看重新渲染是如何在类以及功能组件中工作的。

下面是一些重新渲染 React 组件的方法。

**状态发生变化时重新渲染组件:**

每当 React 组件状态改变时，React 必须运行 render 方法。

```
import React from 'react'
export default class App extends React.Component {
 componentDidMount() {
   this.setState({});
 }
 render() {
   console.log('render() method')
  return <p>Hi!<p>;
}
}
```

在上面提到的例子中，组件安装时的状态被更新。
你甚至可以选择重新呈现一个事件组件，例如，一个点击事件。

```
import React from "react";

export default class App extends React.Component {
 state = {
   mssg: ""
 };

 handleClick = () => {
   this.setState({ mssg: "Hi there!" });
 };

 render() {
   console.log("render() method");
   return (
     <>
```

{this.state.mssg}

</> ); } }

## 输出:

说点什么

这两个输出看起来有点像这样:

```
render() method 
render() method
```

**道具改变时重新渲染组件**

```
import React from 'react'

class Child extends React.Component {
 render() {
   console.log('Child component: render()');
   return
}
}
```

在上面的例子中，<child>组件没有状态。但是，它有一个自定义属性，即它接受的消息。</child>

当点击按钮时，它将更新<child>组件，渲染生命周期将再次运行。</child>

```
Child component: render() 
Child component: render()
```

**用关键道具重新渲染:**

您可以更改 key prop 的值，这将使 React 卸载并再次重新装载组件，并经历渲染生命周期。

**强制重新渲染:**

非常不鼓励这种方法，也不推荐这种方法。你应该总是使用道具和状态来进行新的渲染。

然而，这是你可以做的。

```
import React from 'react'

export default class App extends React. Component {
 handleClick = () => {
   // force a re-render
   this.forceUpdate();
 };
 render() {
   console.log('App component: render()')
   return (
     <>
     </>
   );
 }
}
```

## 输出:

说点什么

# 结论:

你必须尝试使用上述资源，包括不同的例子和案例，使你的重新渲染计数。如果需要重新渲染 React 组件，应该始终更新组件状态和属性。

尝试避免使用关键道具进行重新渲染，因为这会使渲染变得更加复杂。尽管有一些奇怪的用例需要它。但是，您必须记住，不要使用强制更新来导致重新渲染。

有任何疑问吗？或者需要 ReactJS 的任何服务？从 Bosc Tech 雇佣 react js 开发人员[。今天就从我们的专家那里获得免费咨询！](https://bosctechlabs.com/hire-react-developer/)

**内容来源:**[https://bosctechlabs . com/force-react-components-to-re render-without-calling-set-state/](https://bosctechlabs.com/force-react-components-to-rerender-without-calling-set-state/)