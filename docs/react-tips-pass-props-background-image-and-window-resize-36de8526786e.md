# 反应提示—传递道具、背景图像和调整窗口大小

> 原文：<https://blog.devgenius.io/react-tips-pass-props-background-image-and-window-resize-36de8526786e?source=collection_archive---------8----------------------->

![](img/0737d905dd0bc0cdaeddcae297ee7c2b.png)

照片由 [Troy T](https://unsplash.com/@ttcollect?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 使用 React 路由器将属性传递给处理程序组件

我们可以用渲染属性将属性传递给路由处理器组件。

例如，我们可以写:

```
<Route path="/greeting/:name" render={(props) => <Greeting greeting="Hello, " {...props} />} />
```

然后，我们可以通过编写以下代码来定义我们的`Greeting`组件:

```
class Greeting extends React.Component {
  render() {
    const { text, match: {params} } = this.props;
    const { name } = params; return (
      <>
        <p>
          {text} {name}
        </p>
      </>
    );
  }
}
```

我们将`props`从参数传递给`Greeting`组件。

然后我们从`Greetings`的`this.props`物业拿道具。

那我们就可以随心所欲了。

# 调整浏览器大小时重新渲染视图

如果我们使用函数组件，我们可以用钩子监视窗口的大小调整。

例如，我们可以写:

```
import React, { useLayoutEffect, useState } from 'react';function useWindowSize() {
  const [size, setSize] = useState([0, 0]);
  useLayoutEffect(() => {
    function updateSize() {
      setSize([window.innerWidth, window.innerHeight]);
    }
    window.addEventListener('resize', updateSize);
    updateSize();
    return () => window.removeEventListener('resize', updateSize);
  }, []);
  return size;
}function App() {
  const [width, height] = useWindowSize();
  return <span>{width} x {height}</span>;
}
```

我们创建了一个`useWindowSize`钩子，通过为`resize`事件添加一个事件监听器来监视窗口大小的变化。

然后当我们返回函数来清理代码时，我们调用`removeEventListener`来移除事件监听器。

我们在事件处理程序中设置了`size`状态。

`window.innerWidth`表示窗口宽度，`window.innerHeight`表示窗口高度。

然后，我们可以通过在组件中使用钩子来获取最新的值。

我们创建自己的钩子来封装我们的逻辑，这样我们就不必在代码中散布它们。

此外，我们可以在任何地方重用它，这是另一个好处。

在类组件中，我们还可以观察`resize`事件。

例如，我们可以写:

```
import React from 'react';class App extends React.Component {
  state = { width: 0, height: 0 };
  render() {
    return <span>{this.state.width} x {this.state.height}</span>;
  } updateDimensions = () => {
    this.setState({ width: window.innerWidth, height: window.innerHeight });
  }; componentDidMount() {
    window.addEventListener('resize', this.updateDimensions);
  } componentWillUnmount() {
    window.removeEventListener('resize', this.updateDimensions);
  }
}
```

我们有`updateDiemsions`的方法用`setState`设置窗口的宽度和高度。

然后我们调用`componentDidMount`中的`addEventListener`来监听窗口大小的变化。

当组件卸载时，我们调用`removeEventListener`来移除监听器。

在`render`方法中，我们渲染窗口大小。

# Fix“未捕获的类型错误:无法读取未定义错误的属性“setState”

我们可以通过在类组件中调用的方法上调用`bind`来修复`setState`未定义的错误。

例如，我们写道:

```
constructor(props) {
  super(props);
  this.state = {
    count: 1
  }; this.setCount = this.setCount.bind(this);
}
```

我们调用`this.setCount.bind(this)`来设置`this.setCount`的`this`的值给组件。

我们在构造函数中调用它，这样我们就不必在其他地方多次调用`bind`。

此外，我们可以使用箭头函数来代替。

例如，我们可以写:

```
setCount  = () => {
  this.setState({
    count : this.state.count + 1
  });
}
```

在我们的组件类中。

# 带有可选路径参数的反应路由器

我们可以通过向 path 参数添加一个问号来添加一个可选的 path 参数。

例如，我们可以写:

```
<Route path="/to/page/:pathParam?" component={Page} />
```

对于一个可选参数。

如果我们想要更多，我们可以写:

```
<Route path="/to/page/:foo?/:bar?" component={Page} />
```

这适用于版本 4 或更高版本。

# 使用 React 内联样式设置背景图像

我们可以用 React inline 样式设置背景图像，方法是:

```
backgroundImage: `url(${Background})`
```

在我们传递给`styles`道具的对象内部。

`Backgeround`是我们作为模块导入的图像。

可以使用 Webpack 将图像作为模块导入。

![](img/704b1f61e6f7be8b4d07bdf038cd4c9f.png)

[韦斯利·廷吉](https://unsplash.com/@wesleyphotography?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以通过将图像作为一个模块导入并将其设置为背景图像的值来设置背景图像。

为了得到窗口的大小，我们可以监听`resize`事件。

我们可以用 React Router 将 props 传递给路由处理组件。