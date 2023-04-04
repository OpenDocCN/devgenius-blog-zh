# React 提示—加载数据、还原存储和样式化链接

> 原文：<https://blog.devgenius.io/react-tips-loading-data-redux-stores-and-styled-links-e5665c6f1f11?source=collection_archive---------14----------------------->

![](img/f4c46260d25f1c7d3b7d40a276acf954.png)

Ramiz deda kovi 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 在子组件内的父组件上设置状态

我们可以将一个函数从父组件传递给子组件。

然后我们可以在子进程中调用它来设置父进程的状态。

例如，我们可以写:

```
import React, { useState, useEffect, useCallback, useRef } from "react";function Parent() {
  const [parentState, setParentState] = useState(0); const wrapperSetParentState = useCallback(val => {
    setParentState(val);
  }, [setParentState]); return (
    <div>
      <Child
        setParentState={wrapperSetParentState}
      />
      <div>{parentState}</div>
    </div>
  );
};function Child({ setParentState }) {
  const [childState, setChildState] = useState(0); useEffect(() => {
    setParentState(childState);
  }, [setParentState, childState]); const onSliderChange = e => {
    setChildState(e.target.value);
  }; return (
    <div>
      <input
        type="range"
        min="1"
        max="100"
        value={childState}
        onChange={onSliderChange}
      ></input>
    </div>
  );
};
```

我们创建了带有滑块输入的`Child`组件，它接受一个取值范围。

此外，我们在`onChange`属性中定义了`onSliderChange`函数来观察变化。

我们设置了`e.target.value`，它将滑块的值作为`childState.`的值

我们也关注`childState`的变化，关注它的变化。

我们调用`setParentState`来调用我们传递给道具的`Parent`中的函数。

它设置来自父节点的`parentState`值，

然后我们在`Parent`中显示`parentState`的最新值。

`useCallback`让我们缓存`parentState`的值，如果它没有改变的话。

# 反应路由器 v4 <navlink>vs <link></navlink>

`NavLink`在链接被导航到的时候添加一个活动类，这样我们就可以用不同于其他链接的方式来设计它。

需要一个`activeClassName`道具来让我们更改类名。

例如，我们可以写:

```
<NavLink to="/" activeClassName="active">profile</NavLink>
```

我们可以动态地按照我们想要的方式来设计链接。

# 使用不间断空格反应渲染字符串

我们可以设置`white-space: nowrap`样式来显示带有不间断空格的字符串。

例如，我们可以写:

```
<div style="white-space: nowrap">no breaks</div>
```

或者

```
<div style={{ whiteSpace: 'nowrap' }}>no breaks</div>
```

# React 组件的异步呈现

我们可以通过提供在加载时显示的加载消息来呈现异步数据。

如果有数据，我们就显示数据而不是加载消息。

例如，我们可以写:

```
import React from 'react';class App extends React.PureComponent {
  constructor(props){
    super(props);
    this.state = {
      data: null
    }       
  } componentDidMount(){
    fetch('https://randomuser.me/api')
      .then((resp) => resp.json())
      .then((response) => {
        const [data] = response.results;
        setState({ data });
      });
    } render(){
    return (<div>
      {this.state.data === null ? 
        <div>Loading</div>:
        <div>{this.state.data.name.first}</div>
      }
    </div>);
  }
}
```

当`data`状态为`null`时，我们显示加载信息。

否则，我们从 API 中显示我们想要显示的数据。

我们在`componentDidMount`中获取数据，这意味着数据将在组件挂载时获取。

# 使用 React-Redux 调度应用程序加载操作

我们可以调用`mapStateToProps`将 Redux 存储中的状态映射为组件的道具。

`mapDispatchToProps`将动作调度函数与组件的道具进行匹配。

例如，我们可以写:

```
class App extends Component {
  componentDidMount() {
    this.props.getUser()
  } render() {
    return this.props.isReady
      ? <div> ready </div>
      : <div>not ready</div>
  }
}const mapStateToProps = (state) => ({
  isReady: state.isReady,
})const mapDispatchToProps = dispatch => {
  return {
    getUser: () => dispatch(getUserActionCreator())
  }
}export default connect(mapStateToProps, mapDispatchToProps)(App)
```

我们创建一个接受一个`state`参数的`mapStateToProps`函数，该函数有一个带有 Redux 状态的 state 参数。

然后我们可以从中获得`isReady`状态，并将其映射到键中的`isReady`道具。

`getUser`被映射到一个调用`dispatch`来分派动作的函数。

`getUseractionCreator`返回一个具有`type` 和`payload` 属性的对象，将这些属性传递给 reducer 并运行正确的操作。

使用函数组件，我们可以编写:

```
import { appInit } from '../store/actions';
import { useDispatch } from 'react-redux';const appInit = () => ({ type: APP_INIT });export default App() {
  const dispatch = useDispatch();
  useEffect(() => dispatch(appInit()), [ dispatch ]); return (<div>something</div>);
}
```

我们调用`useDispatch`钩子让我们调度从`appInit`函数返回的动作，该函数也有`type`和`payload`属性。

![](img/17b8e1b4413615d6912550a9b11e549d.png)

Robina Weermeijer 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以使用 React-Redux 调度操作来操纵存储。

此外，我们可以在子组件中调用父组件的函数。

当我们加载数据时，不同的东西可以按照我们的方式呈现。