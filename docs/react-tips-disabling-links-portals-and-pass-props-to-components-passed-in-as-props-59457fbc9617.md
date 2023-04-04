# React 提示—禁用链接、门户和作为道具传入的组件的传递道具

> 原文：<https://blog.devgenius.io/react-tips-disabling-links-portals-and-pass-props-to-components-passed-in-as-props-59457fbc9617?source=collection_archive---------4----------------------->

![](img/69705c3aa3a1186baac8458ef25a6836.png)

凯蒂·伯诺斯基在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 如何禁用激活的<link>

我们可以通过在 CSS 中设置`pointer-events`属性来禁用链接。

因为我们可以用 CSS 类将一个`className`属性传递给`Link`，我们可以写:

```
class Foo extends React.Component {
  render() {
    return (
      <Link to='/bar' className='disabled-link'>click me</Link>
    );
  }
}
```

我们将`disabled-link`类名应用于链接。

然后，我们可以添加以下 CSS 来禁用链接:

```
.disabled-link {
  pointer-events: none;
}
```

# 如何使用 ReactDOM.createPortal()

我们可以使用`ReactDOM.createPortal()`在 DOM 树中通常位置之外的元素中呈现组件，

例如，给定以下 HTML:

```
<html>
  <body>
    <div id="root"></div>
    <div id="portal"></div>
  </body>
</html>
```

我们可以写:

```
const mainContainer = document.getElementById('root');
const portalContainer = document.getElementById('portal');class Foo extends React.Component {
  render() {
     return (
       <h1>I am rendered through a Portal.</h1>
     );
  }
}class App extends React.Component {
  render() {
    return (
      <div>
         <h1>Hello World</h1>
         {ReactDOM.createPortal(<Foo />, portalContainer)}
     </div>
    );
  }
}ReactDOM.render(<App/>, mainContainer);
```

在`App`中，我们调用了`ReactDOM.createPortal(<Foo />, portal`来渲染`portalContainer`中的`Foo`组件，也就是 HTML 中 ID 为`portal`的 div。

JSX 代码的其余部分在它们通常的地方被渲染。

这让我们可以用任何我们喜欢的方式来渲染我们的组件。

# React 中嵌套组件的事件冒泡

React 在捕获和冒泡阶段都支持合成事件。

因此，我们的单击事件会传播到父元素，除非我们显式地阻止它们这样做。

所以如果我们有:

```
<root>
  <div onClick={this.handleAllClickEvents}>
    <foo>
      <bar>
        <target />
      </bar>
    </foo>
  </div>
</root>
```

我们可以用`handleAllClickEvents`方法监听来自子组件的点击事件。

在里面，我们可以写:

```
handleAllClickEvents(event) {
  const target = event.relatedTarget;
  const targetId = target.id;
  switch(targetId) {
    case 'foo':
      //...
    case 'bar':
      //...
  }
}
```

我们用`event.relatedTarget`属性获得点击目标。

然后我们用`id`属性得到点击目标的 ID。

然后我们就可以随心所欲地检查身份证了。

# 在 React 类组件中设置初始状态的不同方法

有多种方法可以设置类中组件的初始状态。

一种方式是标准的，另一种不是。

标准方法是:

```
class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      name: 'james'
    }
  } render() {
    return  <div>{this.state.name}</div>
  }
}
```

我们用`this.state`赋值在`constructor`中设置它。

非标准的方式是:

```
class App extends Component {
  state = {
    name: 'John'
  } render() {
    return  <div>{this.state.name}</div>
  }
}
```

这不是标准的 JavaScript 语法。

然而，我们可以将它与 TypeScript 或 Babel 插件一起使用，将其转换回标准 JavaScript。

这也设置了`this.state`实例变量。

# 将道具传递给作为道具传递的组件

如果我们想将道具传递给一个已经作为道具传入的组件，那么我们需要用`React.cloneElement`方法进行克隆。

例如，我们可以写:

```
const GrandChild = props => <div>{props.count}</div>;class Child extends React.Component{
  constructor(){
    super();
    this.state = { count: 1 };
    this.updateCount= this.updateCount.bind(this);
  }

  updateCount(){
    this.setState(prevState => ({ count: prevState.count + 1 }))
  }

  render(){
    const Comp = this.props.comp;
    return (
      <div>
        {React.cloneElement(Comp, { count: this.state.count })}
        <button onClick={this.updateCount}>+</button>
      </div>
    );
  }
}class Parent extends React.Component{
  render() {
    return(
      <Child comp={<GrandChild />} />
    )
  }
}
```

我们有显示`count`道具的`GrandChild`组件。

然后我们有了`Child`道具让我们更新`count`状态。

我们将它传递给用`React.cloneElement`从`comp` prop 传入的组件。

我们需要克隆它，这样我们就可以返回一个可写的组件版本。

第二个参数有我们要分别传入的属性名和值。

![](img/8174b37b202d3ff408cd7ec95bec490c.png)

[玛格丽塔·科斯里奥](https://unsplash.com/@margaritakosior?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

我们可以用`cloneElement`方法将道具传递给作为道具传递的组件。

有了门户，我们可以在任何我们喜欢的地方呈现我们的组件。

我们可以用 CSS 禁用 React 路由器链接。