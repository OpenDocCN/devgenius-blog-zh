# React 提示—样式化组件、批量更新、子验证

> 原文：<https://blog.devgenius.io/react-tips-styled-components-batch-updates-child-validation-e49930b881d7?source=collection_archive---------11----------------------->

![](img/3dd21b3cf9804c0a58aeb3548307b73f.png)

托马斯·博罗夫斯基在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 用钩子反应批处理状态更新批处理

如果状态改变并且异步触发，那么它们不会被批处理。

另一方面，如果它们被直接触发，那么它们将被批量处理。

例如，如果我们在一个承诺中调用多个状态改变函数，那么它们不会被批处理:

```
import React, { useState } from 'react';
import ReactDOM from 'react-dom';function Component() {
  const [foo, setFoo] = useState('a');
  const [bar, setBar] = useState('b'); const handleClickPromise = () => {
    Promise.resolve().then(() => {
      setFoo('aa');
      setBar('bb');
    });
  } return (
    <>
      <button onClick={handleClickPromise}>
        click me
      </button>
      <p>{foo} {bar}</p>
   </>
  );
}
```

如果状态改变函数在一个承诺中被调用，就像我们在`then`回调中所做的那样，那么它们不会被批处理。

另一方面，如果我们有:

```
import React, { useState } from 'react';
import ReactDOM from 'react-dom';function Component() {
  const [foo, setFoo] = useState('a');
  const [bar, setBar] = useState('b'); const handleClick = () => {
    setFoo('aa');
    setBar('bb');
  } return (
    <>
      <button onClick={handleClick}>
        click me
      </button>
      <p>{foo} {bar}</p>
   </>
  );
}
```

然后他们会被分批。

如果它们是批处理的，那么更新会立即显示出来。

# 样式化组件中的条件呈现

为了用样式化组件创建的组件有条件地呈现某些内容，我们可以用 props 作为参数将函数传入字符串。

例如，我们可以写:

```
const Button = styled.button`
  width: 100%;
  outline: 0;
  border: 0;
  height: 100%;
  justify-content: center;
  align-items: center;
  line-height: 0.2; ${({ active }) => active && `
    background: green;
  `}
`;
```

我们有很多静态风格。

最后，我们有一个带`active`道具的函数。

那么我们返回的`'background: green'`跟`active`就是`true`。

`styled.button`模板标签是一个将样式字符串转换成带有样式的组件的函数。

然后可以通过书写来使用它:

```
<Button active={this.state.active}></Button>
```

我们还可以通过编写以下代码向现有组件添加条件样式:

```
const StyledFoo = styled(Foo)`
  background: ${props => props.active ? 'blue' : 'green'}
`
```

我们使用`styled`函数，它接受一个组件并返回一个模板标签。

然后我们创建一个字符串，其中插入了一个函数，该函数接受属性并根据属性是否为 T10 返回 T7 或 T8。

# React 组件中只允许特定类型的子组件

为了在 React 组件中允许特定类型的子组件，我们可以遍历`children`的每个条目，然后在看到我们不想要的东西时抛出一个错误。

为此，我们可以在对象中创建一个定制的验证函数，并将其设置为`propTypes`属性的值。

例如，我们可以写:

```
class ItemGroup extends Component {
  render() {
    return (
      <div>{this.props.children}</div>
    )
  }
}ItemGroup.propTypes = {
  children(props, propName, componentName) {
    const prop = props[propName]
    let error = null
    React.Children.forEach(prop, (child) => {
      if (child.type !== Item) {
        error = new Error(`${componentName} children should be Items`);
      }
    })
    return error;
  }
}
```

我们创建了自己的道具验证方法来验证`children`道具。

我们得到所有带有`React.children`的子节点，并在其上调用`forEach`来循环每个项目。

在回调中，我们得到了正在循环的`child`元素，我们可以用`type`属性检查它的类型。

如果它没有返回类型为`Item`的东西，我们设置`error`并返回它。

# 用 React 防止多次按键

为了防止 React 多次按下按钮，我们可以在单击按钮后使用`disabled`道具禁用按钮。

例如，我们可以写:

```
class App extends React.Component {
  this.state = {
    disabled : false
  }; handleClick = (event) => {
    if (this.state.disabled) {
      return;
    }
    this.setState({ disabled: true }); 
  } render() {
    return (
     <button onClick={this.handleClick} disabled={this.state.disabled}>
      {this.state.disabled ? 'Sending...' : 'Send'}
     <button>
    );
}
```

我们有检查`disabled`状态的`handleClick`方法。

如果是`true`，我们什么都不做。

否则，我们将`disabled`状态设置为`true`，按钮将被禁用，因为我们将该值传递给了`disabled`属性。

![](img/858ea22cc9a6a56fbe901beee874dbc1.png)

安德鲁·斯潘塞在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以用`disabled`道具禁用点击按钮。

如果状态更新在函数组件中被同步调用，那么它们是批处理的。

可以有条件地使用样式化组件进行样式化。