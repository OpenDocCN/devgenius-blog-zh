# 反应提示—处理程序参数、导航和子级

> 原文：<https://blog.devgenius.io/react-tips-handler-arguments-navigation-and-children-3daec4690c92?source=collection_archive---------28----------------------->

![](img/e5770a3ce38a83fd48082a6cf35a5fe0.png)

照片由[维达尔·诺德里-马西森](https://unsplash.com/@vidarnm?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 使用 react-router 以编程方式导航

我们可以通过`useHistory`钩子有计划地使用 React Router。

例如，我们可以写:

```
import { useHistory } from "react-router-dom";function HomeButton() {
  const history = useHistory(); const goHome = () => {
    history.push("/home");
  } return (
    <button type="button" onClick={goHome}>
      go home
    </button>
  );
}
```

我们使用`useHistory`钩子来获取我们的`Button`组件中的历史对象。

然后我们调用`history.push`导航。

它只是使用历史 API 来进行导航。

# 循环内部反应 JSX

我们只能在 JSX 写 JavaScript 表达式，所以我们不能用常规循环遍历所有的条目。

正确的做法是将所有的项目放入一个数组，并放入我们的 JSX。

例如，我们可以写:

```
let rows = [];
for (let i = 0; i < numRows; i++) {
  rows.push(<TableRow key={i} row={rows[i]} />);
}
return <tbody>{rows}</tbody>;
```

我们将所有的条目都推入`rows`，并将其嵌入我们的 JSX 代码中。

为了使它更简短，我们也可以这样写:

```
<tbody>
  {arr.map((a, i) => <TableRow row={a} key={i} />)}
</tbody>
```

# 反应中的三个点

我们可以在 React 中使用三个点来将对象属性扩展到道具中。

例如，我们可以写:

```
<Modal {...this.props} title='Modal heading' animation={false}>
```

展开`this.props`物体作为`Modal`的道具。

键是属性名，值是它们的值。

# 把道具传给 this.props .孩子们

我们可以通过使用`cloneElement`方法将道具传递给`this.props.children`。

例如，我们可以写:

```
<div>
    {React.cloneElement(this.props.children, { foo: this.state.foo })}
</div>
```

我们只是用`React.cloneElement`方法将`foo`道具传递给`this.props.children`。

如果我们想将道具传递给多个子元素，我们可以写:

```
import React, { Children, isValidElement, cloneElement } from 'react';const Child = ({ foo }) => (
  <div>{foo}</div>
);function Parent({ children }) { render() {
    const childrenWithProps = Children.map(children, child => {
      if (isValidElement(child)) {
        return cloneElement(child, { foo })
      }
      return child;
    }); return <div>{childrenWithProps}</div>
  }
};ReactDOM.render(
  <Parent>
    <Child value="1" />
    <Child value="2" />
  </Parent>,
  document.getElementById('container')
);
```

我们调用`Children.map`来用`cloneElement`填充道具。

在我们调用它之前，我们检查`child`是否是一个带有`isValidElement`的有效元素。

现在我们可以将道具传递给多个元素。

# 强制 React 组件在不调用 setState 的情况下重新呈现

我们可以调用`this.forceUpdate`来强制渲染，但是应该避免，因为我们应该使用`setState`或者状态改变函数来更新我们的组件。

# Redux 中的异步流中间件

我们可以使用 redux-thunk 中间件通过我们的操作运行异步代码。

例如，我们可以写:

```
function loadUserProfile(userId) {
  return dispatch => fetch(`http://example.com/${userId}`)
    .then(res => res.json())
    .then(
      data => dispatch({
        type: 'EXAMPLE_LOADED',
        data
      }),
      err => dispatch({
        type: 'EXAMPLE_LOAD_FAILED',
        err
      })
    );
}
```

获取数据以调用调度具有给定动作类型的动作的动作。

然后我们可以写:

```
<div onClick={e => dispatch(actions.loadExample(123)}>load</div>
```

调用 invoke 操作来运行异步代码。

# React 中数组子项的唯一键

我们需要 React 组件中数组子元素的唯一键。

这是因为他们让 React 通过 ID 跟踪条目。

否则，我们可能会得到意想不到的结果。

我们在`key`属性中为每个条目添加了一个唯一的 ID。

例如，我们可以写:

```
<tbody>
  {rows.map((row, i) => {
    return <ObjectRow key={row.id} />;
  })}
</tbody>
```

我们传入`row`的`id`，这是`key`道具独有的。

一个唯一的 ID 会给我们带来预期的结果，不管它在什么位置。

# 将值传递给事件处理程序方法

我们可以用箭头函数传递一个值。

例如，我们可以写:

```
<button onClick={() => this.handleSort(column)}>{column}</button>
```

我们直接将值传递给 arrow 函数。

我们也可以将其提取到一个子组件中。

例如，我们可以写:

```
class Button extends Component {
  handleClick = () => {
    this.props.onHeaderClick(this.props.value);
  } render() {
    return (
      <button onClick={this.handleClick}>
        {this.props.column}
      </button>
    );
  }
}
```

我们通过道具将值传递给`handleClick`。

![](img/5c8f50697e804b03eb4a44085ad865d7.png)

照片由[Duan Smetana](https://unsplash.com/@veverkolog?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们在 JSX 总是有表达方式。

我们应该使用`useHistory`来导航 React 路由器。

唯一键应该在数组中。

我们可以通过各种方式将参数传递给处理函数。