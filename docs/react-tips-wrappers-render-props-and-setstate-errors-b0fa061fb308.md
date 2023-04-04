# 反应提示—包装器、渲染道具和设置状态错误

> 原文：<https://blog.devgenius.io/react-tips-wrappers-render-props-and-setstate-errors-b0fa061fb308?source=collection_archive---------22----------------------->

![](img/0d089a9eba0cdc1f80e6810fb68e9b2d.png)

由 [Mysaell Armendariz](https://unsplash.com/@mysa21?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 修复“警告:setState(…):在现有状态转换期间无法更新”错误

为了修复这个错误，我们不应该在`render`方法中调用方法。

例如，在构造函数中，我们将状态改变方法:

```
this.handleButtonTrue = this.handleButtonChange.bind(this, true);
this.handleButtonFalse = this.handleButtonChange.bind(this, false);
```

然后用我们的`render`方法。我们写道:

```
<Button onClick={this.handleButtonTrue}>true</Button>
<Button onClick={this.handleButtonFalse}>false/Button>
```

# 何时使用 React setState 回调

当我们需要运行一些总是在状态改变后运行的代码时，我们应该使用 React `setState`的回调。

例如，我们可以写:

```
changeTitle(event) {
  this.setState({ title: event.target.value }, () => {
    this.validateTitle();
  });},
validateTitle() {
  if (this.state.title.length === 0) {
    this.setState({ error: 'no blank title' });
  }
},
```

我们调用`setState`来改变`title`状态。

然后我们运行`validateTitle`来验证`title`状态的最新值。

# 对日期对象进行适当验证

我们可以用`instanceof`方法验证日期对象是否作为道具传入。

我们可以写:

```
PropTypes.instanceOf(Date)
```

来做验证。

# 在 React 组件外部访问 Redux 存储的最佳方式

我们可以通过导出从`createStore`返回的值来访问 React 组件外部的 Redux 存储。

然后我们可以在任何我们喜欢的地方使用这个值。

例如，在`store.js`中，我们写道:

```
const store = createStore(myReducer);
export store;
```

然后在`app.js`中，我们写道:

```
import { store } from './store'
store.dispatch(
  //...
)
```

如果我们想使用多个商店，我们可以编写一个函数，如果商店不存在，就创建一个商店，并返回一个带有商店的承诺。

然后我们可以利用这一点来得到商店。

# 将一个组件包装到另一个组件中

为了让我们将一个组件包装在另一个组件中，我们创建了一个带有`children`属性的包装器组件

例如，我们可以写:

```
const Wrapper = ({children}) => (
  <div>
    <div>header</div>
    <div>{children}</div>
    <div>footer</div>
  </div>
);const App = ({name}) => <div>Hello {name}</div>;const App = ({name}) => (
  <Wrapper>
    <App name={name}/>
  </Wrapper>
);
```

我们创建一个接受`children`属性的`Wrapper`组件。

然后，我们创建一个`App`组件，并将其放入`Wrapper`组件中。

我们还可以使用渲染道具将组件传递给另一个组件。

这意味着我们将整个`render`函数传递给另一个组件。

例如，我们可以写:

```
class Wrapper extends React.Component {
  state = {
    count: 0
  }; increment = () => {
    const { count } = this.state;
    return this.setState({ count: count + 1 });
  }; render() {
    const { count } = this.state; return (
      <div>
        {this.props.render({
          increment: this.increment,
          count: count
        })}
      </div>
    );
  }
}class App extends React.Component {
  render() {
    return (
      <Wrapper
        render={({ increment, count }) => (
          <div>
            <div>
              <p>{count}</p>
              <button onClick={() => increment()}>Increment</button>
            </div>
          </div>
        )}
      />
    );
  }
}
```

我们创建了一个`Wrapper`，它采用一个`render`道具，该道具采用一个呈现组件的函数。

我们为`App`中的`render`属性传入了一个值。

然后在`Wrapper`内调用，对象为`increment`和`count`属性。

从`App`和`count`状态传入`increment`方法。

# 在 React 中验证嵌套对象的属性类型

我们可以使用`shape`方法验证嵌套对象 bu 的属性类型。

例如，我们可以写:

```
import PropTypes from 'prop-types';propTypes: {
  data: PropTypes.shape({
    firstName: PropTypes.string.isRequired,
    lastName: PropTypes.string
  })
}
```

我们用描述`data`道具结构的对象调用`PropTypes.shape`方法。

我们有 2 个字符串，`isRequired`表示它是必需的。

# 如何在 React 中添加评论？

我们可以通过将注释放在花括号中来添加注释。

例如，我们可以写:

```
<div>
  {/* button click */}
  <Button whenClicked={this.handleClick}>click me </Button>
  <List />
</div>
```

Out 注释在花括号内。

它们和普通的 JavaScript 注释一样。

![](img/a9dbe8dc2f9bdf3671e22fc44e18b2e8.png)

[乔伊斯·罗梅罗](https://unsplash.com/@joyceromero?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

如果我们把注释放在花括号中，我们可以添加注释。

我们不应该在我们的`render`方法中调用`setState`。

Redux 存储可以在 React 组件外部访问。

我们可以通过创建一个接受`children`属性的包装组件来嵌套子组件。

或者我们可以在我们的组件中接受一个`render` prop 来呈现组件。