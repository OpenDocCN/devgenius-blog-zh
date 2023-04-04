# 下一次前端面试必须知道的问题——第三部分

> 原文：<https://blog.devgenius.io/must-known-react-questions-for-your-next-frontend-interview-part-3-70818c6481d?source=collection_archive---------17----------------------->

![](img/ede4cb65f9bae2758075b825322c2add.png)

这篇文章是上一篇文章的延续，在上一篇文章中，我为你的下一次前端面试列出了一些重要的问题和答案。

1.  **使用带 props 实参的超级构造函数的目的是什么？**

在调用`super()`方法之前，子类构造函数不能使用`this`引用。这同样适用于 ES6 子类。将 props 参数传递给`super()`调用的主要原因是为了访问子构造函数中的`this.props`。

传球道具:

```
class MyComponent extends React.Component {
  constructor(props) {
    super(props)

    console.log(this.props) // prints { name: 'John', age: 42 }
  }
}
```

不传递道具:

```
class MyComponent extends React.Component {
  constructor(props) {
    super()

    console.log(this.props) // prints undefined

    // but props parameter is still available
    console.log(props) // prints { name: 'John', age: 42 }
  }

  render() {
    // no difference outside constructor
    console.log(this.props) // prints { name: 'John', age: 42 }
  }
}
```

上面的代码片段揭示了`this.props`只是在构造函数中有所不同。在构造函数之外也是如此。

**2。如何用动态键名设置状态？**

如果你使用 ES6 或 Babel transpiler 来转换你的 JSX 代码，那么你可以用*计算属性名*来完成。

```
handleInputChange(event) {
  this.setState({ [event.target.id]: event.target.value })
}
```

**3。为什么 React 使用** `**className**` **胜过** `**class**` **属性？**

`class`是 JavaScript 中的关键字，JSX 是 JavaScript 的扩展。这就是 React 使用`className`而不是`class`的主要原因。传递一个字符串作为`className`道具。

```
render() {
  return <span className={'menu navigation-menu'}>{'Menu'}</span>
}
```

**4。什么是碎片？**

这是 React 中的一种常见模式，用于一个组件返回多个元素。 *Fragments* 让你不用给 DOM 添加额外的节点就能分组一个孩子列表。

```
render() {
  return (
    <React.Fragment>
      <ChildA />
      <ChildB />
      <ChildC />
    </React.Fragment>
  )
}
```

还有一个更短的语法，但是它在许多工具中不被支持:

```
render() {
  return (
    <>
      <ChildA />
      <ChildB />
      <ChildC />
    </>
  )
}
```

**5。为什么片段比容器 div 好？**

以下是原因列表

*   通过不创建额外的 DOM 节点，片段速度更快，使用的内存更少。这只对非常大和深的树有真正的好处。
*   有些 CSS 机制像 *Flexbox* 和 *CSS Grid* 有特殊的父子关系，在中间添加 div 很难保持想要的布局。
*   DOM 检查器不那么杂乱。

**6。什么是无状态组件？**

如果一个组件的行为独立于它的状态，那么它可能是一个无状态的组件。您可以使用函数或类来创建无状态组件。但是除非你需要在组件中使用生命周期挂钩，否则你应该选择功能组件。如果你决定在这里使用函数组件，会有很多好处；它们易于编写、理解和测试，速度稍快，并且您可以完全避免使用`this`关键字。

**7。什么是有状态组件？**

如果组件的行为依赖于组件的*状态*，那么它可以被称为有状态组件。这些*有状态组件*总是*类组件*，并且有一个在`constructor`中被初始化的状态。

```
class App extends Component {
  constructor(props) {
    super(props)
    this.state = { count: 0 }
  }

  render() {
    // ...
  }
}
```

React 16.8 更新:

钩子让你不用写类就可以使用状态和其他 React 特性。

*等效功能部件*

```
 import React, {useState} from 'react';

 const App = (props) => {
   const [count, setCount] = useState(0);

   return (
     // JSX
   )
 }
```

8。如何在 React 中对道具应用验证？

当应用程序在*开发模式*下运行时，React 将自动检查我们在组件上设置的所有属性，以确保它们具有*正确的类型*。如果类型不正确，React 将在控制台中生成警告消息。由于性能影响，它在*生产模式*中被禁用。强制道具用`isRequired`定义。

一组预定义道具类型:

1.  `PropTypes.number`
2.  `PropTypes.string`
3.  `PropTypes.array`
4.  `PropTypes.object`
5.  `PropTypes.func`
6.  `PropTypes.node`
7.  `PropTypes.element`
8.  `PropTypes.bool`
9.  `PropTypes.symbol`
10.  `PropTypes.any`

我们可以将`User`组件的`propTypes`定义如下:

```
import React from 'react'
import PropTypes from 'prop-types'

class User extends React.Component {
  static propTypes = {
    name: PropTypes.string.isRequired,
    age: PropTypes.number.isRequired
  }

  render() {
    return (
      <>
        <h1>{`Welcome, ${this.props.name}`}</h1>
        <h2>{`Age, ${this.props.age}`}</h2>
      </>
    )
  }
}
```

注意:在 React v15.5 *中，属性类型*从`React.PropTypes`移到了`prop-types`库中。

*等效功能部件*

```
import React from 'react'
import PropTypes from 'prop-types'

function User({name, age}) {
  return (
    <>
      <h1>{`Welcome, ${name}`}</h1>
      <h2>{`Age, ${age}`}</h2>
    </>
  )
}

User.propTypes = {
    name: PropTypes.string.isRequired,
    age: PropTypes.number.isRequired
  }
```

**9。React 的局限性是什么？**

React 也有一些限制，

*   React 只是一个视图库，不是一个完整的框架。
*   对于刚接触 web 开发的初学者来说，有一个学习曲线。
*   将 React 集成到传统的 MVC 框架中需要一些额外的配置。
*   代码复杂性随着内联模板和 JSX 而增加。
*   过多的小组件导致过度工程化或样板化。

10。事件的反应如何不同？

在 React 元素中处理事件有一些语法差异:

*   React 事件处理程序使用 camelCase 命名，而不是小写。
*   使用 JSX，你传递一个函数作为事件处理器，而不是一个字符串。

在这里，我列出了另外 10 个 ReactJs 问题，这是这个系列的最后一部分。

*   给它鼓掌
*   关注我 [Mohit garg](https://medium.com/u/8a9ce53808fd?source=post_page-----70818c6481d--------------------------------) ，获取基于 JavaScript、ReactJs、Web 开发、Web3 等其他文章的通知。