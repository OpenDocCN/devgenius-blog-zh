# 反应提示—输入事件、重复、前向引用和冒泡

> 原文：<https://blog.devgenius.io/react-tips-input-event-redux-forward-ref-and-bubbling-7824a0c3c713?source=collection_archive---------7----------------------->

![](img/e81fb135d913b520e39e89ffbf9914ee.png)

maggie bell 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 在 React 中触发变更事件的最佳方式

我们可以通过获取本机输入值设置器以编程方式触发事件。

然后我们可以用它而不是 React 的版本来触发输入事件。

例如，我们可以写:

```
const nativeInputValueSetter = Object.getOwnPropertyDescriptor(window.HTMLInputElement.prototype, "value").set;
nativeInputValueSetter.call(input, 'something');const ev = new Event('input', { bubbles: true });
input.dispatchEvent(ev);
```

我们通过以下方式获得本机输入值设置器:

```
const nativeInputValueSetter = Object.getOwnPropertyDescriptor(window.HTMLInputElement.prototype, "value").set;
```

然后，我们通过编写以下内容创建一个本地输入事件:

```
nativeInputValueSetter.call(input, 'something');
```

第一个参数是输入元素。

第二个是输入的值。

然后我们创建了事件:

```
const ev = new Event('input', { bubbles: true });
```

然后，我们在`input`元素上调度输入事件:

```
input.dispatchEvent(ev);
```

# 单击时防止嵌套组件中的事件冒泡

我们可以通过使用`stopProphation`方法来防止嵌套组件中的事件冒泡。

例如，我们可以写:

```
class ListItem extends React.Component {
  constructor(){
    super();
    this.handleClick = this.handleClick.bind(this);
  } handleClick(e) {
    e.stopPropagation();
    this.props.onClick();
  } render() {
    return (
      <li onClick={this.handleClick}>
        {this.props.children}
      </li>       
    )
  }
}class List extends React.Component {
  constructor(){
    super();
    this.handleClick = this.handleClick.bind(this);
  } handleClick(e) {
    // ...
  }
  render() {
    return (
      <ul onClick={this.handleClick}>
        <ListItem onClick={this.handleClick}>Item</ListItem> 
      </ul>
    )
  }
}
```

因为我们有一个 ul 元素的 click listenerss 和多个 li 元素的 click listener，我们必须在 li 元素的 click 处理程序上调用`stopPropagation`,这样 click 事件就不会冒泡到 ul 元素和其他元素。

我们是这样做的:

```
e.stopPropagation();
```

`ListItem`中的`handleClick`法。

# 在基于类的组件中使用 React.forwardRef

我们可以在基于类的组件中使用`React.forwardRef`，方法是传入一个回调函数，该函数返回基于类的组件。

例如，我们可以写:

```
class DivComponent extends Component {
  render() {
    return (
      <div ref={this.props.innerRef}>
        foo
      </div>
    )
  }
}const Comp = React.forwardRef((props, ref) => <DivComponent
  innerRef={ref} {...props}
/>);
```

我们传入一个带有`props`和`ref`参数的回调，并返回带有 props 和 ref 的`DivComponent`作为它的 props。

然后我们可以通过引用`this.props.innerRef`来访问 ref，这样我们就可以将它指定为 div 的 ref。

# 使用 connect with Redux 从 this.props 进行简单调度

我们可以通过使用`mapDispatchToProps`方法将我们的调度函数映射到 props。

例如，我们可以写:

```
const mapDispatchToProps = (dispatch) => {
  return {
    onClick: () => dispatch(decrement())
  };
}
```

将减量动作的`dispatch`调用映射到`onClick`属性。

我们也可以将`dispatch`函数放入对象中直接访问它:

```
function mapDispatchToProps(dispatch) {
  return {
    dispatch,
    onClick: () => dispatch(decrement())
  };
}
```

我们也可以使用`bindActionCreators`来转动:

```
function mapDispatchToProps(dispatch) {
  return {
    onPlusClick: () => dispatch(increment()),
    onMinusClick: () => dispatch(decrement())
  };
}
```

收件人:

```
import { bindActionCreators } from 'redux';const mapDispatchToProps = (dispatch) => {
  return bindActionCreators({
    onPlusClick: increment,
    onMinusClick: decrement
  }, dispatch);
}
```

我们将`increment`和`decrement`动作映射到`onPlusClick`和`onMinusClick`道具。

我们可以将其简化为:

```
const mapDispatchToProps = (dispatch) => {
  return bindActionCreators({ increment, decrement }, dispatch);
}
```

然后我们只需要从道具中获取`increment`和`decrement`，并调用它们来调度动作。

我们可以把它变得更短:

```
const mapDispatchToProps = (dispatch) => {
  return bindActionCreators({ increment, decrement }, dispatch);
}export default connect(
  mapStateToProps,
  mapDispatchToProps
)(App);
```

变成:

```
export default connect(
  mapStateToProps,
  { increment, decrement }
)(App);
```

这样会注入`increment`和`decrement`作为没有`mapDispatchToProps`的道具。

# 更新 Redux 中特定数组项内的单个值

我们可以在 Redux 中更新特定数组项中的单个值，通过划分数组、修改要更改的条目，然后将它们重新组合在一起的方式编写 reducer。

例如，我们可以写:

```
case 'MODIFY_ARRAY':
   return { 
       ...state, 
       contents: [
          ...state.contents.slice(0, index),
          { title: "title", text: "text" },
         ...state.contents.slice(index + 1)
       ]
    }
```

我们调用`slice`将数组分成块。然后我们有了下一个物体。

然后，我们使用速度操作符将它们重新组合成一个数组。

![](img/33f1e619c4b9c59e33b188dfc9e14f35.png)

克里斯蒂娜·安妮·科斯特洛在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

`mapDispatchToProps`有很多人手不足。

我们可以通过在 reducer 中返回一个新的数组来改变一个数组条目。

我们可以调用`stopPropagation`来停止事件冒泡。

此外，我们可以使用本机输入值设置器以编程方式触发输入事件。