# Preact —浅渲染、漂亮打印和外部 DOM 突变

> 原文：<https://blog.devgenius.io/preact-shallow-rendering-pretty-print-and-external-dom-mutations-15f396cb28f1?source=collection_archive---------3----------------------->

![](img/97f934d82cffd3afad38a101cc387fa3.png)

照片由[Klemen tuar](https://unsplash.com/@techouse?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Preact 是一个前端 web 框架，类似于 react。

它比 React 小，也不复杂。

在本文中，我们将了解如何开始使用 Preact 进行前端开发。

# 浅层渲染

我们只能用浅渲染来浅渲染组件的一个级别。

例如，我们可以写:

```
import { shallowRender } from "preact-render-to-string";
import { h } from "preact";const Foo = () => <div>foo</div>;
const App = (
  <div class="foo">
    <Foo />
  </div>
);console.log(shallowRender(App));
```

呈现`App`组件而不呈现`Foo`组件。

然后，当我们记录返回的字符串时，我们得到:

```
'<div class="foo"><Foo></Foo></div>'
```

记录在案。

# 漂亮模式

我们可以用`pretty`选项以更人性化的方式呈现字符串。

为了使用它，我们写:

```
import { render } from "preact-render-to-string";
import { h } from "preact";const Foo = () => <div>foo</div>;
const App = (
  <div class="foo">
    <Foo />
  </div>
);console.log(render(App, {}, { pretty: true }));
```

然后我们看到:

```
<div class="foo">
 <div>foo</div>
</div>
```

记录在案。

# JSX 模式

如果我们做任何类型的快照测试，JSX 渲染模式是有用的。

它呈现的输出就好像是用 JSX 写的一样。

例如，我们可以写:

```
import render from "preact-render-to-string/jsx";
import { h } from "preact";const App = <div data-foo={true} />;console.log(render(App));
```

然后我们看到:

```
<div data-foo={true}></div>
```

记录在案。

# 外部 DOM 突变

我们可以在组件中添加外部 DOM 突变代码。

为此，我们必须禁用虚拟 DOM 渲染和区分算法，这样它就不会撤销组件中的任何外部 DOM 操作。

例如，我们可以写:

```
import { Component, render } from "preact";class Example extends Component {
  shouldComponentUpdate() {
    return false;
  } componentDidMount() {
    let thing = document.createElement("maybe-a-custom-element");
    this.base.appendChild(thing);
  } componentWillUnmount() {} render() {
    return <div class="example" />;
  }
}const App = () => <Example />;if (typeof window !== "undefined") {
  render(<App />, document.getElementById("root"));
}
```

我们返回`shouldComponentUpdate`中的`false`来禁用虚拟 DOM 差分算法。

然后在`componentDidMount`中，我们调用`document.createElement`向 DOM 中添加一个新元素。

# 结论

我们可以浅渲染和漂亮地打印字符串中呈现的组件。

此外，我们可以在 Preact 组件中直接操作 DOM。