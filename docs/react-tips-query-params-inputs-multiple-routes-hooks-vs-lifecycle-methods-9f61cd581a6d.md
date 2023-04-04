# React 提示—查询参数、输入、多路径、挂钩与生命周期方法

> 原文：<https://blog.devgenius.io/react-tips-query-params-inputs-multiple-routes-hooks-vs-lifecycle-methods-9f61cd581a6d?source=collection_archive---------2----------------------->

![](img/eff4afbd6702801517ebbb694bf8f5a8.png)

杰克·卡塔拉诺在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 以编程方式更新 React 路由器中的查询参数

我们可以通过调用`history.push`用 React Router 更新查询参数。

我们可以用一个对象来调用它:

```
history.push({
  pathname: '/cars',
  search: '?color=green'
})
```

或者:

```
history.push('/car?color=green')
```

# contentEditable 的更改事件

我们不是通过监听 change 事件来监听`contentEditable`元素中的内容变化，而是监听输入事件。

例如，我们可以写:

```
<div
  contentEditable
  onInput={e => console.log(e.currentTarget.textContent)}
>
  foo bar
</div>
```

我们向`onInput`道具传递一个事件处理程序。

然后我们可以用`e.currentTarget.textContent`得到内容的值。

# ReactJS 函数组件内的生命周期方法

我们用功能组件中的钩子代替生命周期方法。

`useEffect`相当于生命周期挂钩。

而`useState`相当于`setState`。

例如，我们可以写:

```
const Grid = (props) => {
  const [data, setData] = useState();

  const getData = () => {
    const data = await fetchData();
    setData(data);
  } useEffect(() => {
    getData();
  }, []); return(
    <div>
      {data.map(d => <Row data={data} />)}
    </div>
  )
}
```

我们有`useState`钩子，它返回带有最新状态值的数组和设置状态的函数。

然后我们有了`useEffect`钩子，它让我们产生副作用。

第二个参数是需要注意的值。

如果它是空的，那么`useEffect`中的回调只在组件被挂载时运行。

我们可以在一个组件中有多个`useEffect`钩子，不像类组件的生命周期方法。

然后，我们在返回的 JSX 表达式中呈现项目。

与`componentWillUnmount`等价的是我们在`useEffect`回调中返回的函数。

例如，我们可以写:

```
useEffect(() => {
  window.addEventListener('click', handler);
  return () => {
    window.removeEventListener('click', handler);
  }
}, [])
```

我们调用任何代码来清除我们在`useEffect`回调中返回的函数中的资源。

`componentDidUpdate`的等效操作是将值传递给`useEffect`的第二个参数中的数组。

例如，如果我们想观察`foo`状态和`bar`属性的值的变化，我们可以写:

```
useEffect(() => {
  //...
}, [foo, props.bar])
```

我们只是把它们传入数组，然后我们总是得到最新的值。

# 无法在 React 输入文本字段中键入内容

如果我们希望能够在输入文本字段中键入内容，那么我们必须使它成为一个受控的输入。

这意味着我们用事件处理程序中最新输入的值来设置状态的值。

我们将它传递给`onChange` prop。

我们将`value`属性设置为该状态的值。

例如，我们写道:

```
<input
  type="text"
  value={this.props.value}
  onChange={this.handleChange}
/>
```

为了做到这一点。

完整的例子，我们可以写成:

```
class Form extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''}; this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  } handleChange(event) {
    this.setState({ value: event.target.value });
  } handleSubmit(event) {
    alert(this.state.value);
    event.preventDefault();
  } render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          <input type="text" value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

`handleChange`方法设置输入的最新值，该值作为`value`状态的值存储在`event.target.value`中。

然后，当我们单击 Submit 时，我们可以在`handleSubmit`方法中得到它。

# React 路由器中同一组件的多个路径名

我们可以通过向`path`属性传递一个包含所有路径名的数组来分配多个路径重定向到同一个路径。

例如，我们可以写:

```
<Router>
  <Route path={["/home", "/user", "/profile"]} component={Home} />
</Router>
```

我们只需将它们传递到`Router`组件的`path` prop 中，它们就会全部匹配。

数组中的每个 pat 都是一个正则表达式字符串。

![](img/8e415a14242f44c03849bd25fea7408a.png)

[黎明](https://unsplash.com/@dawnhylon?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

可以指定多个路径名来访问同一个组件。

我们可以使用`history.push`来更新查询参数。

在功能组件的类组件中有组件生命周期方法的等价物。

为了让我们键入一个输入，我们必须用最近输入的值来设置状态，并将其设置为值。