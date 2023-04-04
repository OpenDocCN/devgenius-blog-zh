# Preact —创建 Web 组件和服务器端呈现

> 原文：<https://blog.devgenius.io/preact-create-web-component-and-server-side-rendering-21e6c2e4d165?source=collection_archive---------1----------------------->

![](img/c71c2a4e2dbb8c6b351500467e3ded8f.png)

[左 eris kallergis](https://unsplash.com/@lefterisk?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

Preact 是一个前端 web 框架，类似于 react。

它比 React 小，也不复杂。

在本文中，我们将了解如何开始使用 Preact 进行前端开发。

# 创建 Web 组件

我们可以用`preact-custom-element`包将 Preact 组件转换成 web 组件。

例如，我们可以写:

```
import { render } from "preact";
import register from "preact-custom-element";const Greeting = ({ name = "World" }) => <p>Hello, {name}!</p>;register(Greeting, "x-greeting", ["name"]);export default function App() {
  return <x-greeting name="Jane"></x-greeting>;
}if (typeof window !== "undefined") {
  render(<App />, document.getElementById("root"));
}
```

导入`register`函数，将我们的 Preact 组件注册为 web 组件。

我们有一个`Greeting`组件，我们将它注册为`x-greeting`定制元素。

第三个参数具有`x-greeting`所取的属性，即`name`属性。

它将被插入到`Greeting`中的`p`元素中，就像我们使用常规的 Preact 组件一样。

在`App`中，我们使用带有`name`道具集的`x-greeting` web 组件。

然后我们看到:

```
Hello, Jane!
```

已显示。

# 观察到的属性

我们必须使属性可观察，以便它可以响应值的变化。

为了使它们可见，我们可以这样写:

```
import { Component, render } from "preact";
import register from "preact-custom-element";class Greeting extends Component {
  static tagName = "x-greeting";
  static observedAttributes = ["name"]; render({ name }) {
    return <p>Hello, {name}!</p>;
  }
}
register(Greeting);export default function App() {
  return <x-greeting name="Jane"></x-greeting>;
}if (typeof window !== "undefined") {
  render(<App />, document.getElementById("root"));
}
```

我们创建了我们的`Greeting`组件。

然后我们设置`tagName`静态属性来设置它的标签名。

我们将`observedAttributes`设置为要观察的属性名数组。

在`App`中，我们以同样的方式使用`x-greeting`组件。

如果没有指定`observableAttributes`，那么属性的数据类型将从`propTypes`的键中推断出来。

设置`propTypes`将使它们可被观察到。

例如，如果我们有:

```
import { render } from "preact";
import register from "preact-custom-element";function FullName({ first, last }) {
  return (
    <span>
      {first} {last}
    </span>
  );
}FullName.propTypes = {
  first: Object,
  last: Object
};register(FullName, "full-name");
export default function App() {
  return <full-name first="Jane" last="Smith"></full-name>;
}if (typeof window !== "undefined") {
  render(<App />, document.getElementById("root"));
}
```

我们将`first`和`last`道具类型设置为`Object`，它们将变得可以被观察到。

# 服务器端渲染

我们可以将 Preact 组件呈现为字符串。

这让我们可以在服务器端呈现内容，这对提高性能很有用。

例如，我们可以写:

```
import render from "preact-render-to-string";const App = <div class="foo">content</div>;console.log(render(App));
```

`render`将`App`组件呈现为一个字符串。

现在我们应该看到:

```
<div class="foo">content</div>
```

记录在控制台日志中。

# 结论

我们可以从 Preact 组件创建 web 组件。

此外，我们可以将组件呈现为字符串。