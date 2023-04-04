# React 提示—列跨度、活动链接、API 调用和多个引用

> 原文：<https://blog.devgenius.io/react-tips-colspan-active-links-api-calls-and-multiple-refs-90db250e6785?source=collection_archive---------11----------------------->

![](img/bc21ab2dfbd9620ade2dafab47ffd0d0.png)

由 [Leila Boujnane](https://unsplash.com/@leilaboujnane?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 用 React 中的钩子进行 API 调用

我们可以在`useEffect`钩子中进行 API 调用。

例如，我们可以写:

```
function User() {
  const [firstName, setFirstName] = React.useState();

  React.useEffect(() => {
    fetch('https://randomuser.me/api/')
      .then(results => results.json())
      .then(data => {
        const {name} = data.results[0];
        setFirstName(name.first);
      });
  }, []);   return (
    <div>
      Name: {firstName}
    </div>
  );
}
```

我们在`useEffect`回调中调用`fetch`来获取数据。

然后我们用`setFirstName`设置`firstNam,e`状态。

我们向第二个参数`useEffect`传递一个空数组，使回调只在组件挂载上运行。

最后，我们在 return 语句中呈现`firstName`。

如果我们希望在属性改变时加载数据，那么我们将属性传递给第二个参数中的数组。

# 反应跨列

我们可以用`colSpan`属性添加 colspan 属性。

例如，我们可以写:

```
<td colSpan={6} />
```

# 如何在 React 路由器中设置活动链接的样式

我们可以用 React Router 的`NavLink`组件来设计一个活动链接。

例如，我们可以写:

```
<NavLink to="/profile" activeClassName="selected">
  profile
</NavLink>
```

当链接处于活动状态时，`activeClassName`允许我们设置的类名。

还有一个`activeStyle`道具，让我们为活动链接传入样式。

例如，我们可以写:

```
<NavLink
  to="/profile"
  activeStyle={{
    fontWeight: "bold",
    color: "green"
  }}
>
  profile
</NavLink>
```

我们传入一个带有我们想要应用的样式的对象。

CSS 属性名称全部改为 camel case。

# 如果输入值按状态变化，则触发 onChange

我们可以在处理程序中设置状态。

我们可以用`Event`构造函数触发我们想要的事件。

例如，我们可以写:

```
class App extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
      value: 'foo'
    }
  } handleChange (e) {
    console.log('changed')
  } handleClick () {
    this.setState({ value: 'something' })
    const event = new Event('input', { bubbles: true });
    this.input.dispatchEvent(event);
  }
  render () {
    return (
      <div>
        <input readOnly value={this.state.value} onChange={(e) => this.handleChange(e)} ref={(input)=> this.input = input} />
        <button onClick={this.handleClick.bind(this)}>update input</button>
      </div>
    )
  }
}
```

我们有一个按钮，它有一个点击处理程序。

那就是`handleClick`法。

然后我们在它上面调用`setState`来设置`value`状态。

然后我们创建一个输入事件，并让它冒泡。

然后用`dispatchEvent`触发输入上的事件。

# 用 React 添加 Google 登录按钮

我们可以在 React 组件中添加一个 google 登录按钮，方法是在组件加载时加载 Google 登录代码。

例如，我们可以写:

```
componentDidMount() {
  gapi.signin2.render('g-signin2', {
    'scope': 'https://www.googleapis.com/auth/plus.login',
    'width': 200,
    'height': 50,
    'longtitle': true,
    'theme': 'dark',
    'onsuccess': this. onSignIn
  });  
}
```

我们调用 API 附带的`gapi.signin2.render`方法。

它被添加到`componentDidMount`中，以便在组件加载时运行。

使用函数组件，我们可以编写:

```
useEffect(() => {
  window.gapi.signin2.render('g-signin2', {
    'scope': 'https://www.googleapis.com/auth/plus.login',
    'width': 200,
    'height': 50,
    'longtitle': true,
    'theme': 'dark',
    'onsuccess': onSignIn
  })
}, [])
```

我们在`useEffect`回调中调用相同的方法。

第二个参数中的空数组确保它仅在组件加载时加载。

# 对带挂钩的元素数组使用多个引用

我们可以在`useEffect`回调中创建多个引用。

例如，我们可以写:

```
const List = ({ arr }) => {
  const arrLength = arr.length;
  const [elRefs, setElRefs] = React.useState([]); React.useEffect(() => {
    setElRefs(elRefs => (
      Array(arrLength).fill().map((_, i) => elRefs[i] || ReactcreateRef())
    ));
  }, [arrLength]); return (
    <div>
      {arr.map((el, i) => (
        <div ref={elRefs[i]}>...</div>
      ))}
    </div>
  );
}
```

我们用`useState`钩子创建了一个数组。

然后我们调用从`useState`返回的`setElRefs`并调用`fill`来创建一个空槽数组。

然后我们调用`map`来创建引用或返回一个现有的引用。

然后我们通过索引将 ref 传递给 div。

![](img/dc40ea788d999c415dfbf31beca55448.png)

照片由 [Hari Nandakumar](https://unsplash.com/@hariprasad000?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以通过将多个引用映射到数组来添加它们。

此外，我们可以在`useEffect`回调中进行 API 调用。

`NavLink`当处于活动状态时，可以对其进行设计。