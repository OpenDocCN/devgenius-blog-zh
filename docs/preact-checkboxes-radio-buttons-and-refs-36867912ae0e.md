# 预动作—复选框、单选按钮和引用

> 原文：<https://blog.devgenius.io/preact-checkboxes-radio-buttons-and-refs-36867912ae0e?source=collection_archive---------4----------------------->

![](img/535dfabe111181d271b94e1ed1a3090f.png)

照片由[纳迪尔·沙胡尔](https://unsplash.com/@naadirshah?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Preact 是一个前端 web 框架，类似于 react。

它比 React 小，也不复杂。

在本文中，我们将了解如何开始使用 Preact 进行前端开发。

# 复选框和单选按钮

我们可以添加复选框或单选按钮，并获取它们的选定值。

例如，我们可以写:

```
import { Component, render } from "preact";export default class App extends Component {
  toggle = (e) => {
    let checked = !this.state.checked;
    this.setState({ checked });
  }; render(_, { checked }) {
    return (
      <label>
        <input type="checkbox" checked={checked} onClick={this.toggle} />
      </label>
    );
  }
}
if (typeof window !== "undefined") {
  render(<App />, document.getElementById("root"));
}
```

我们创建了`checked`状态。

然后我们将它传递给`checked` prop。

将`onClick`道具设置为`toggle`方法。

我们调用`setState`来设置`checked`状态。

我们可以通过编写以下内容来添加单选按钮:

```
import { Component, render } from "preact";export default class App extends Component {
  constructor() {
    super();
    this.state = {
      fruit: "apple"
    };
  }onChangeValue = (event) => {
    this.setState({ fruit: event.target.value });
  };render(_, { fruit }) {
    return (
      <div onChange={this.onChangeValue}>
        <input
          checked={fruit === "apple"}
          type="radio"
          value="apple"
          name="fruit"
        />{" "}
        apple
        <input
          checked={fruit === "orange"}
          type="radio"
          value="orange"
          name="fruit"
        />{" "}
        orange
        <input
          checked={fruit === "grape"}
          type="radio"
          value="grape"
          name="fruit"
        />{" "}
        grape
      </div>
    );
  }
}
if (typeof window !== "undefined") {
  render(<App />, document.getElementById("root"));
}
```

我们添加单选按钮。

然后我们通过`onChange`回调来观察单选按钮选择的变化。

在`onChangeValue`方法中，我们调用`setState`来设置`fruit`状态。

在`input`中，我们设置`checked`属性来设置单选按钮的选中状态。

我们从`render`方法中获取`fruit`状态，并对照它进行检查以设置检查状态。

# `createRef`

我们调用`createRef`函数来返回一个带有`current`属性的普通对象。

每当调用`render`方法时，Preact 都会将 DOM 节点或组件分配给`current`。

例如，我们可以写:

```
import { Component, createRef, render } from "preact";export default class App extends Component {
  ref = createRef(); componentDidMount() {
    console.log(this.ref.current);
  } render() {
    return <div ref={this.ref}>foo</div>;
  }
}
if (typeof window !== "undefined") {
  render(<App />, document.getElementById("root"));
}
```

将我们的`ref`分配给 div。

所以当我们记录`this.ref.current`时，我们会看到 div 被记录。

# 回调参考

我们还可以通过传入一个回调函数来获取对元素的引用。

例如，我们可以写:

```
import { Component, render } from "preact";export default class App extends Component {
  ref = null;
  setRef = (dom) => (this.ref = dom); componentDidMount() {
    console.log(this.ref);
  } render() {
    return <div ref={this.setRef}>foo</div>;
  }
}
if (typeof window !== "undefined") {
  render(<App />, document.getElementById("root"));
}
```

我们有带`dom`参数的`setRef`函数，它有我们将这个函数作为`ref`的值传递给的 DOM 节点。

因为我们将它作为 div 中的`ref`属性的值传入，所以 div 就是`dom`的值。

我们将它分配给`this.ref`，以便我们可以访问它。

在`componentDidMount`钩子中，我们记录了`this.ref`的值，我们看到它的值是 div。

# 结论

我们可以使用 Preact 轻松添加复选框和单选按钮。

我们可以用 refs 得到 DOM 元素。