# React 提示—重定向、错误处理和 Redux，以及。环境文件

> 原文：<https://blog.devgenius.io/react-tips-redirect-error-handling-and-redux-and-env-files-4978e058fbe5?source=collection_archive---------4----------------------->

![](img/938ab6f05040259cc06d1d8d32c110c3.png)

凯文·基思在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 点击时反应按钮重定向页面

通过获取`history`对象，我们可以在单击元素时重定向到页面。

例如，我们可以写:

```
import React from 'react';
import { useHistory } from "react-router-dom";
function LoginPage () {
  const history = useHistory();
  const onClick = () => {  
    history.push('new-page');
  } return (
    <div>               
      <button
        onClick={onClick}
      >
        Login
      </button>
    </div>
  );
}
export default LoginPage;
```

我们用`useHistory`得到本机`history`对象

然后我们定义`onClick`函数来调用`history.push`导航到一个新页面。

如果我们使用类组件，我们可以写:

```
import { withRouter } from 'react-router-dom';class LoginPage extends Component {
  constuctor() {
    this.routeChange = this.routeChange.bind(this);
  } onClick() {
    this.props.history.push('forgot-password');
  } render() {
    return (
      <div>
        <button onClick={onClick}>Forgot password?</button>
      </div>
    );
  }
}export default withRouter(LoginPage);
```

我们有`LoginPage`类组件。

为了使`history` prop 对组件可用，我们将它传递给`withRouter`。

我们有调用`this.props.history.push`方法导航到新页面的`onClick`方法。

`routerChange`方法必须绑定到带有`bind`的组件，因此我们将组件类设置为`this`的值。

# 在 JSX 渲染布尔值

我们可以通过先将布尔值转换成字符串来呈现 JSX。

例如，我们可以写:

```
Boolean Value: {bool.toString()}
```

或者:

```
Boolean Value: {String(bool)}
```

或者:

```
Boolean Value: {'' + bool}
```

或者:

```
{`Boolean Value: ${bool}`}
```

或者:

```
Boolean Value: {JSON.stringify(bool)}
```

有许多方法可以将布尔值转换成字符串。

它们都很好，除了连接，这可能会令人困惑。

# 将 event.target 与 React 组件一起使用

我们可以使用`event.target`来获取 DOM 元素的属性。

我们可以使用`dataset`属性来获取带有`data-`前缀的属性。

它们被认为是自定义属性，允许我们通过元素传递数据。

例如，如果我们有一个按钮，那么我们可以写:

```
<button 
  data-foo="home" 
  data-bar="home" 
  onClick={this.props.onClick} 
/> 
  Button
</button>
```

然后我们可以写:

```
onClick(e) {
   console.log(e.target.dataset.bar, e.target.dataset.foo);
}
```

从我们用`dataset`属性单击的按钮中获取`foo`和`bar`属性。

`dataset.foo`具有`data-foo`属性的值。

`dataset.bar`具有`data-bar`属性的值。

# 向 React 项目添加. env 文件

如果我们有一个 create-react-app 项目，我们可以添加一个`.env`文件来添加环境变量。

在文件内部，任何以`REACT_APP_`前缀为前缀的键都被读入应用程序。

所以在我们的`.env`文件中，我们可以写:

```
REACT_APP_API_KEY=secret
```

然后，我们可以通过编写以下内容来使用环境变量值:

```
fetchData() { 
  fetch(`https://example.com?api-key=${process.env.REACT_APP_API_KEY}`)
    .then((res) => res.json())
    .then((data) => console.log(data))
}
```

我们用`process.env`对象读取环境变量值。

# 如何向 Input 的 onChange 处理程序传递多个参数

我们可以向输入的`onChange`处理程序传递多个参数。

为此，我们可以写:

```
<Input 
  type="text"
  value={this.state.name}
  onChange={this.onChange.bind(this, this.state.user.id)} 
/>
```

然后，我们可以通过编写以下代码来定义我们的`onChange`方法:

```
onChange(userId, event) {
  const newName = event.target.value;
},
```

我们把传入的东西作为第一个参数。

最后一个参数是对象。

# 如何用 Redux 和 Axios 捕获和处理错误响应 422

我们可以调用`dispatch`来调度 React 组件中的动作。

例如，我们可以写:

```
axios({
  method: 'post',
  responseType: 'json',
  url: `${SERVER_URL}/login`,
  data: {
    username,
    password
  }
})
 .then(response => {
   dispatch(setUser(response));
 })
 .catch(error => {
   dispatch({ type: AUTH_FAILED });
   dispatch({ type: ERROR, payload: error.data.error.message });
 });
```

我们向 Axios 发出 POST 请求。

然后在`then`回调中，我们获取`response`并调用`dispatch`和我们的`setUser`动作创建者。

然后我们调用带有回调的`catch`方法来处理错误。

我们称`dispatch`有无有效载荷。

调用`dispatch`会用新的状态值更新存储。

![](img/8b0b659c58a1cf3d90aafe9f7f76a2bf.png)

[主持人李](https://unsplash.com/@anchorlee?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

# 结论

我们可以点击重定向页面。

我们可以通过调用`dispatch`更新商店来处理 Redux 的错误。

如果我们将布尔值转换成字符串，就可以呈现它们。