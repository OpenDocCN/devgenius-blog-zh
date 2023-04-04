# 反应提示—外部 URL、计时器和悬停效果

> 原文：<https://blog.devgenius.io/react-tips-external-urls-timers-and-hover-effect-9148c1a384a9?source=collection_archive---------3----------------------->

![](img/6a7ee8498a8bf2e9f8ad8ff53c6dc536.png)

Giuseppe Martini 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 在触发函数之前，请等待 setState 完成

我们可以等待`setState`结束，然后通过将函数作为`setState`的第二个参数传入来触发函数。

例如，我们可以写:

```
this.setState({
  firstName,
  lastName,
  age
}, () => { 
  console.log(this.state) ;
})
```

我们称`setState`为先有对象的州。

然后我们运行回调函数来获取`this.state`的最新值。

# 带反应路由器的外部链路

我们可以通过传入一个组件重定向到一个外部链接来添加一个外部链接。

例如，我们可以写:

```
<Route path='/external-site' >
  {() => { 
     useEffect(() => {
       window.location.href = 'https://example.com'; 
     }, [])
     return null;
  }}
</Route>
```

我们使用`Router`组件和`path`道具来设置路径。

然后在我们的组件中，我们通过将 URL 设置为`window.location.href`的值来重定向到一个外部 URL。

我们返回`null`不渲染任何东西。

我们也可以在`componentDidMount`中直接重定向:

```
class RedirectPage extends React.Component {
  componentDidMount(){
    window.location.replace('http://www.example.com')
  } render(){
    return null;
  }
}
```

# 在 React 中访问悬停状态

我们可以通过监听`mouseenter` 和`mouseleave` 事件来设置 hove 状态。

例如，我们可以写:

```
<div
  onMouseEnter={this.onMouseEnter}
  onMouseLeave={this.onMouseLeave}
>
  foo
</div>
```

我们可以将事件处理程序传递给`onMouseEnter`和`onMouseLeave`道具。

然后我们可以在这些方法中运行代码来设置悬停状态。

# 功能性无状态组件中的属性类型

我们可以在功能性的无状态组件中设置适当的类型。

例如，我们可以写:

```
import React from "react";
import PropTypes from "prop-types";const Name = ({ name }) => <div>hi {name}</div>;Name.propTypes = {
  name: PropTypes.string
};Name.defaultProps = {
  name: "james"
};
```

我们用`propTypes`属性创建了`Name`组件。

然后我们用它设置`name`道具的数据类型。

我们还可以为`name`道具设置默认值。

我们必须安装`propt-types`包来设置道具的类型。

# React 组件中的 setTimeout()

我们可以在 React 组件中使用`setTimeout`,方法是调用`componentDidMount`中的`setTimeout`,然后清除`componentWillMount`方法中的计时器。

例如，我们可以写:

```
class App extends React.Component {
  constructor() {
    this.state = { position: 0, timer: undefined };    
  } componentDidMount() {
    const timer = setTimeout(() => this.setState({ position: 1 }), 3000)
    this.setState({ timer });
  } componentWillUnmount(){
    clearTimeout(this.state.timer);
  };

  render() {
    return (
      <div>
        {this.state.position}
      </div>
    ); 
  }
}
```

我们用一个回调函数调用了`setTimeout`,超时时间跨度在`componentDidMount`中，以在组件加载时获取计时器。

它返回一个定时器对象，当组件卸载时，我们可以用它调用`clearTimeout`来清除定时器的资源。/

我们用`timer`状态来设置从`setTimeout`返回的定时器的状态。

然后我们在`componentWillUnmount`，我们调用`clearTimeout`来清零定时器。

# React 组件中的 setInterval

像使用`setTimeout`一样，我们可以在 React 组件中调用`setInterval`。

除了我们可以`setInterval`和`clerInterval`之外，它和`setTimeout`几乎一样。

例如，我们写道:

```
class App extends React.Component {
  constructor() {
    this.state = { position: 0, timer: undefined };    
  } componentDidMount() {
    const timer = setInterval(() => this.setState(state => ({ position: state.position + 1 })), 3000)
    this.setState({ timer });
  } componentWillUnmount(){
    clearInterval(this.state.timer);
  };

  render() {
    return (
      <div>
        {this.state.position}
      </div>
    ); 
  }
}
```

我们在`componentDidMount`中调用了`setInterval`来创建一个定时器来定期运行代码。

在`setInterval`回调中，我们每 3 秒更新一次`position`状态。

然后在`componentWillUnmount`中，我们调用`clearInterval`来清除定时器。

有了功能组件，我们可以做同样的事情。

例如，我们可以写:

```
import React, { useState, useEffect } from 'react';const App = () => {
  const [position, setPosition] = useState(0);
  const timer = () => setPosition(position => position + 1); useEffect(() => {
    const timer = setInterval(timer, 1000);
    return () => clearInterval(timer);
  }, []); return <div>{position}</div>;
};
```

我们用`useState`钩子用`setPosiitin`更新`position`状态。

然后我们用`useEffect`钩子在组件加载时创建一个计时器。

然后我们在`useEffect`回调中返回的函数用`clearInterval`删除计时器。

第二个参数中的空数组确保了`useEffect`回调只在`App`第一次加载时运行。

![](img/be19ac045d18b676cf55054a77c6430d.png)

瑞安·斯通在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以创建一个组件来重定向到外部 URL。

同样，我们可以在组件中使用`setTimeout`和`setInterval`。

要设置道具类型，我们可以使用`prop-types`包。