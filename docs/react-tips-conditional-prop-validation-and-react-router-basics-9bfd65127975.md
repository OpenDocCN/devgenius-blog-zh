# React 提示—条件属性验证和 React 路由器基础

> 原文：<https://blog.devgenius.io/react-tips-conditional-prop-validation-and-react-router-basics-9bfd65127975?source=collection_archive---------17----------------------->

![](img/783f66818b7dd83d54f602d28ec7803c.png)

由[托阿·海夫蒂巴](https://unsplash.com/@heftiba?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 如果另一个 Prop 为 null 或 Prop-Types 为空，则需要在 Prop 上设置 is

Prop-types 允许我们使用自定义验证来验证另一个 Prop。

例如，我们可以写:

```
import PropTypes from 'prop-types';//...Item.propTypes = {
  showDelete: PropTypes.bool,
  handleDelete: (props, propName, componentName) => {
    if (props.showDelete === true && typeof props[propName] !== 'function') {
      return new Error('handleDelete must be a function');
    }
  },
}
```

我们有一个`Item`组件，我们在其中设置了`propTypes`属性来进行一些验证。

我们将`showDelete`设置为一个布尔属性。

并且通过检查`showDelete`道具是否为`true`以及`handleDelete`道具是否为函数的函数来验证`handleDelete`。

`propName`是我们当前正在验证的道具的名称。

所以`propName`就是`handleDelete`道具。

我们确保如果`props.showDelete`是`true`，那么`handleDelete`属性是一个函数。

还有 react-required-if 包，让我们可以简化代码。

例如，我们可以写:

```
import requiredIf from 'react-required-if';
import PropTypes from 'prop-types';//...Item.propTypes = {
  showDelete: PropTypes.bool,
  handleDelete: requiredIf(PropTypes.func, props => props.showDelete === true)
};
```

我们确定当`showDelete`是带有`requiredIf`功能的`true`时`handleDelete`是一个功能。

# React 呈现的列表中有错误的组件

如果我们正在呈现一个项目列表，我们应该为列表中的每个项目分配一个唯一的`key`属性值。

例如，我们可以写:

```
<div>
  <div className="packages">
    <div key={0}>
      <button>X</button>
      <Package name="a" />
    </div>
    <div key={1}>
      <button>X</button>
      <Package name="b" />
    </div>
    <div key={2}>
      <button>X</button>
      <Package name="c" />
    </div>
  </div>
</div>
```

我们有一个物品列表，并且我们为每个`key`道具分配了一个唯一的值。

这样，无论我们对它们做什么，它们都会被正确渲染。

如果我们渲染一个项目，我们应该写:

```
consr packages = this.state.packages.map((package, i) => {
  return (
    <div key={package}>
      <button onClick={this.removePackage.bind(this, package)}>X</button>
      <Package name={package.name} />
    </div>
  );
});
```

我们确保为`key`属性传入一个唯一的值。

然后当我们操作数组并重新渲染它时，一切都会被正确渲染。

我们不应该使用数组索引作为键，因为它们可以改变，并且可能不是唯一的。

# 如何使用 React 创建具有多个页面的应用程序

为了让我们在 React 中创建一个具有多个页面的应用程序，我们必须使用 React Router 来进行路由。

我们通过运行以下命令来安装它:

```
npm install react-router-dom
```

然后我们写道:

```
import {
  BrowserRouter as Router,
  Switch,
  Route,
  Link
} from "react-router-dom";const Home = () => {
  return <h2>Home</h2>;
}const About = () => {
  return <h2>About</h2>;
}const Profile = () => {
  return <h2>Profile</h2>;
}export default function App() {
  return (
    <Router>
      <div>
        <nav>
          <ul>
            <li>
              <Link to="/">Home</Link>
            </li>
            <li>
              <Link to="/about">About</Link>
            </li>
            <li>
              <Link to="/profile">Profile</Link>
            </li>
          </ul>
        </nav> <Switch>
          <Route path="/about">
            <About />
          </Route>
          <Route path="/profile">
            <Profile />
          </Route>
          <Route path="/">
            <Home />
          </Route>
        </Switch>
      </div>
    </Router>
  );
}
```

我们创建了 3 个功能组件，用作路线的内容。

然后在`App`组件中，我们导入`Switch`组件并在其中添加路由。

`Route`组件让我们创建路线。

`path`是我们想要映射到组件的 URL 路径。

我们将想要显示的组件添加到`Route`组件中。

`Link`由 React 路由器提供，以便我们转到我们想去的 URL。

它还可以处理嵌套路线、自定义链接、默认路线、查询字符串和 URL 参数。

我们可以通过编写以下内容来添加嵌套路由:

```
loads the Topics component, which renders any further <Route>'s conditionally on the paths :id value.import React from "react";
import {
  BrowserRouter as Router,
  Switch,
  Route,
  Link,
  useRouteMatch,
  useParams
} from "react-router-dom";export default function App() {
  return (
    <Router>
      <div>
        <ul>
          <li>
            <Link to="/">Home</Link>
          </li>
          <li>
            <Link to="/about">About</Link>
          </li>
          <li>
            <Link to="/topics">Topics</Link>
          </li>
        </ul> <Switch>
          <Route path="/topics">
            <Topics />
          </Route>
        </Switch>
      </div>
    </Router>
  );
}function Topics() {
  let match = useRouteMatch(); return (
    <div>
      <h2>Topics</h2> <ul>
        <li>
          <Link to='about'>About</Link>
        </li>
      </ul> <Switch>
        <Route path='/about' >
          <About />
        </Route>
      </Switch>
    </div>
  );
}function About() {
  return <h2>About</h2>;
}
```

我们只是在组件中嵌套路由来嵌套它们。

![](img/a71adb323607e07b7c48dca2c26a575f.png)

由[Teddy sterblom](https://unsplash.com/@teddyosterblom?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们用 React Router 创建了一个包含多个页面的应用程序。

此外，我们可以有条件地用函数或第三方库来验证 props。

我们应该总是为每个列表项添加一个具有唯一值的`key`道具。