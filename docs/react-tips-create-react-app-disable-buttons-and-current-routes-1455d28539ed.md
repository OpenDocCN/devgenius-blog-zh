# 反应提示-创建-反应-应用程序、禁用按钮和当前路线

> 原文：<https://blog.devgenius.io/react-tips-create-react-app-disable-buttons-and-current-routes-1455d28539ed?source=collection_archive---------12----------------------->

![](img/ea1e4df1d72804135c0263193764459c.png)

丹尼尔·罗伯特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 如何使用 React 路由器获取当前路由

我们可以使用`useLocation`钩子用 React Router 获得当前路由。

例如，我们可以写:

```
import { useLocation } from 'react-router-dom'function App() {
  const location = useLocation();
  return <span>{location.pathname}</span>
}
```

`useLocation`返回本机的`location`对象。

所以我们可以用`location.pathname`得到当前的路线。

有了类组件，我们可以使用`withRouter`高阶组件来设置`location`道具做同样的事情。

例如，我们可以写:

```
import {withRouter} from 'react-router-dom';const App = withRouter(props => <Foo {...props}/>);class Foo extends React.Component {
  bar() {
    const {pathname} = this.props.location;
  }
}
```

我们调用了`withRouter`高阶组件，并将`props`从那里传入我们的`Foo`组件。

这将`Foo`的`locations`道具设置为本机`location`对象。

然后我们可以得到路径的`pathname`。

# React 应用程序中的服务

服务只是一段独立于上下文的代码，我们可以在任何地方重用它。

因此，要在 React 应用程序中创建服务，我们只需创建一个模块，用我们想要调用的共享方法导出一个对象。

例如，我们可以写:

```
const HttpService = {
  getTodos(value) {
     // code to get todos
  }, setTodos(value) {
     // code to save todos
  }
};export default HttpService;
```

然后在我们的组件中，我们可以写:

```
import HttpService from "./services/HttpService.js";function TodosPage() {
  const [todos, setTodos] = useState([]);  
  const getTodos = async () => {
    const todos = await HttpService.getTodos();
    setTodos(todos);
  }

  useEffect(() => {
    getTodos()
  }, []) return <Todos todos={todos} />
}
```

我们只需导入模块，然后在导出对象中使用`getTodos`方法来获取我们想要的数据。

我们可以对任何其他独立于上下文的共享代码做同样的事情。

# 当输入为空时禁用按钮

当输入字段为空时，我们可以通过检查输入的值来禁用按钮，并确定是否需要禁用按钮。

例如，我们可以写:

```
class EmailForm extends React.Component {
  constructor() {
    super();
    this.state = {
      email: '',
    };
  }

  handleEmailChange = (evt) => {
    this.setState({ email: evt.target.value });
  } 

  handleSubmit = () => {
    const { email } = this.state;
    alert(`${email} subscribed`);
  }

  render() {
    const { email} = this.state;
    const enabled = email.length > 0;
    return (
      <form onSubmit={this.handleSubmit}>
        <input
          type="text"
          placeholder="Email"
          value={this.state.email}
          onChange={this.handleEmailChange}
        />

        <button disabled={!enabled}>Subscribe</button>
      </form>
    )
  }
}
```

我们创建了一个用于输入电子邮件地址的输入。

当我们在输入中输入内容时，就会运行`handleEmailChange`方法。

它设置了我们在输入的`value`属性和按钮启用检查的`render`方法中使用的`email`状态。

我们通过检查电子邮件的长度是否大于 0 来设置`enabled`标志。

这意味着盒子里装了东西。

然后我们将按钮的`disabled`属性设置为`!enabled`来禁用它。

# 使用“react-scripts start”命令

`react-scripts`是由`create-react-app`提供的一套脚本。

它在没有配置的情况下启动了我们的项目，因此我们不必自己从头开始建立一个 React 项目。

`react-scripts start`设置开发环境，运行开发服务器并支持热重装。

它支持反应、JSX、ES6 和流语法。

也可以使用 ES6 之外的 ES 功能，如对象扩展操作符。

CSS 是自动前缀的，所以我们不必自己添加前缀。

create-react-app 项目还附带了一个单元测试运行程序。

它还会警告我们开发人员的常见错误。

它会为我们将应用程序编译成一个生产就绪包。

它还支持渐进式 web 应用程序的开发。

所有的过路费都会在我们更新 create-react-app 的时候更新。

![](img/0eaf58868f56889a53141d9d17841d57.png)

由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [okeykat](https://unsplash.com/@okeykat?utm_source=medium&utm_medium=referral) 拍摄的照片

# 结论

我们可以创建一个共享模块来重用上下文无关的代码。

create-react-app 为我们做了很多，所以我们应该用它来创建 react 应用。

使用 React 路由器获取当前路由有几种方法。

我们可以通过设置一个布尔表达式作为`disabled`属性的值来禁用按钮。