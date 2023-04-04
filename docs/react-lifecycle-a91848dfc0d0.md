# 反应生命周期

> 原文：<https://blog.devgenius.io/react-lifecycle-a91848dfc0d0?source=collection_archive---------6----------------------->

# 概观

在 react 中，组件有三个阶段:[挂载](https://reactjs.org/docs/react-component.html#mounting)、[更新](https://reactjs.org/docs/react-component.html#updating)和[卸载](https://reactjs.org/docs/react-component.html#unmounting)。[反应生命周期图](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)

# 增加

挂载阶段是创建组件并将其插入 DOM 的阶段。此阶段依次调用的方法有:

## [构造函数()](https://reactjs.org/docs/react-component.html#constructor)

constructor()方法在挂载之前被调用。没有必要定义构造函数方法。通常，构造函数方法用于初始化状态和/或绑定方法。如果组件接受 props，那么在定义方法时它应该是一个参数。道具不应该复制成状态。此外，构造函数不应调用 setState()。

```
constructor(props) {
   super(props); // Necessary to access props in constructor
  this.state = { counter: 0 };
  this.handleClick = this.handleClick.bind(this);
}
```

## [render()](https://reactjs.org/docs/react-component.html#render)

render()方法是类组件唯一需要的方法。渲染方法查看组件状态和属性，并返回以下内容之一:

*   React 元素——典型的 html 标签，如或 react 组件
*   数组和[片段](https://reactjs.org/docs/fragments.html) —呈现多个元素
*   [门户](https://reactjs.org/docs/portals.html) —在不同的 DOM 子树中呈现孩子
*   字符串和数字—典型文本
*   布尔值或空值—不呈现任何内容。例如:返回布尔&&

## [componentidmount()](https://reactjs.org/docs/react-component.html#componentdidmount)

组件挂载后会立即调用 componentDidMount()方法。需要 DOM 节点或 API 调用的初始化应该用这个方法。您可以在此设置订阅，但请确保在 componentWillUnmount()中取消订阅。虽然您可以在此方法中调用 setState()，但它会触发额外的 render()方法调用。

# 更新

更新阶段是道具或状态改变的时候。此阶段依次调用的方法有:

## [渲染()](https://reactjs.org/docs/react-component.html#render)

与挂载阶段的 render()方法相同。

## [componentDidUpdate()](https://reactjs.org/docs/react-component.html#componentdidupdate)

如果组件状态或属性有任何变化，componentDidMount()将被立即调用。初始渲染时不会调用它。如果你使用这个方法，确保你有一个条件语句，否则它将陷入无限循环。

```
componentDidUpdate(prevProps) {
    // Usually compare props with previous props
    if (this.props.id !== prevProps.id) {
        this.getUser(this.props.userID);
    }
}
```

# 卸载

卸载阶段是从 DOM 中删除组件的时候。此阶段调用的唯一方法是:

## [componentWillUnmount()](https://reactjs.org/docs/react-component.html#componentwillunmount)

在卸载组件之前，立即调用 componentWillUnmount()方法。这是进行任何必要清理的地方，如使计时器无效、取消订阅和取消网络请求。您不应该在此方法中调用 setState()，因为它将不会在到达此点后呈现。

# 结论

在 React 的旧版本中，每个阶段调用更多的生命周期和方法。上面列出的方法是最常用的方法。任何没有列出的方法都被认为是遗留的或者不是通用的生命周期。