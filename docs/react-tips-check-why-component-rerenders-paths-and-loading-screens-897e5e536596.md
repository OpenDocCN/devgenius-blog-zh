# 反应提示—检查组件重新呈现、路径和加载屏幕的原因

> 原文：<https://blog.devgenius.io/react-tips-check-why-component-rerenders-paths-and-loading-screens-897e5e536596?source=collection_archive---------21----------------------->

![](img/e36a795df66be68afdf6385f3cec3fa8.png)

米歇尔·特雷瑟默在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 向手动类名添加动态类

我们可以用模板字符串添加动态类。

例如，我们可以写:

```
className={`foo bar ${this.state.something}`}
```

其中`this.state.something`是一个字符串。

此外，我们可以使用`classNames`包使动态设置类变得更容易。

例如，我们可以写:

```
render () {
  const btnClass = classNames({
    'btn': true,
    'btn-pressed': this.state.isPressed
  });
  return <button className={btnClass}>click me</button>;
}
```

我们调用包附带的`classNames`函数，然后我们可以设置添加类的条件作为值。

类名在键中。

# 跟踪 React 组件重新呈现的原因

我们可以编写自己的代码来跟踪 React 组件重新呈现的原因。

在一个类组件中，我们可以检查在`componentDidUpdate`生命周期方法中呈现了什么值。

顾名思义，它在组件更新时运行。

例如，我们可以写:

```
componentDidUpdate(prevProps, prevState) {
  for (const [key, val] of Object.entries(this.props)){
     prevProps[key] !== val && console.log(`prop '${key}' changed`)
  } if(this.state) {
    for (const [key, val] of Object.entries(this.props)){
       prevState[key] !== val && console.log(`state'${key}' changed`)
    }
  }
}
```

我们遍历 props 和 state 的键值对，看看发生了什么变化。

我们只记录以前的道具或状态。

如果我们使用函数组件，我们可以创建自己的钩子来记录发生了什么变化。

例如，我们可以写:

```
function useUpdate(props) {
  const prev = useRef(props);
  useEffect(() => {
    const changedProps = Object.entries(props).reduce((ps, [k, v]) => {
      if (prev.current[k] !== v) {
        ps[k] = [prev.current[k], v];
      }
      return ps;
    }, {}); if (Object.keys(changedProps).length > 0) {
      console.log(changedProps);
    }
    prev.current = props;
  });
}function App(props) {
  useUpdate(props);
  return <div>{props.children}</div>;
}
```

我们创建了`useUpdate` gook，它使用`useRef`钩子来获取道具先前存储的值。

然后我们用`useEffect`钩子监听所有的变化。

我们通过循环遍历道具，然后将它们与之前从`useRef`钩子中获得的道具进行比较，来获得改变后的道具。

`props`参数有最新的道具，`prev`有以前的道具。

然后在我们的 React 组件中，我们用道具调用了我们的`useUpdate`钩子。

# <route exact="" path="“/”">和<route path="“/”">的区别</route></route>

`exact`属性让我们区分几个具有相似名称的路径。

例如，我们可以写:

```
<Switch>
  <Route path="/users" exact component={Users} />
  <Route path="/users/create" exact component={CreateUser} />
</Switch>
```

区分`/users`和`/users/create`路线。

如果我们没有`exact`，那么`/users/create`将与`/users`路线匹配。

# 循环并呈现 React.js 中的元素，而不需要映射对象数组

如果我们想将一个对象的属性呈现给一个列表，我们可以使用`Object.entries`和 array 的`map`方法。

例如，我们可以写:

```
render() {
  return (
     <div>
       {obj && Object.entries(obj).map(([key, value], index) => {
          return <span key={index}>{value}</span>;
       }.bind(this))}       
     </div>
  );
}
```

在我们的`render`方法中，我们使用`Object.entries`以数组的形式获取对象的键值对。

然后我们调用`map`将每个属性值渲染到一个 span 中。

# DOM 渲染时显示加载屏幕

我们可以通过在组件中设置加载状态来显示加载屏幕。

例如，我们可以写:

```
import React, { useState, useEffect } from 'react';const App = () => {
  const [loading, setLoading] = useState(true); useEffect(() => {
    setTimeout(() => setLoading(false), 1000)
  }, []); return !loading && <div>hello world</div>;
};
```

我们有由`useState`钩子返回的`loading` 状态和`setLoading` 函数。

我们最初将它设置为`true`来查看我们的加载消息。

然后，当内容被加载时，我们使用`useEffect`钩子将`loading`状态设置为`false`。

然后我们就可以在`loading`状态为`false`的时候渲染我们想要的内容了。

![](img/7658f0f5a2c0c432455c60427992fe1c.png)

照片由 [Sohini](https://unsplash.com/@hempurecbd?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以使用 React 路由器中的`exact`属性来精确匹配路由。

我们可以设置一个加载状态来显示它。

动态类可以用`classnames`包的字符串解释来设置。

我们可以观察组件中的道具和状态变化，以了解组件重新呈现的原因。