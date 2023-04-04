# 如何使用 React 路由器以编程方式导航

> 原文：<https://blog.devgenius.io/how-to-navigate-programmatically-using-react-router-19a315f2dafd?source=collection_archive---------7----------------------->

![](img/5a0d2b172f78b50423e5c73d030d9e53.png)

通常，现代网站(如 SPAs 或单页应用程序)中的网页没有任何传统的加载过程。这些没有处理新页面的常规方法。通常，客户端路由是节省更多时间来路由资源以进行加载的合适选择。

这些主要是在完整应用程序上的结构变化。基于独特的方法，这些方法是可用的。当动作被执行时，这些可以很容易地被知道。React 是最流行的适合前端开发的库之一。

利用 react js 开发服务是创建响应性用户界面的一个方便的选择。这些主要有独特的路由器适合执行客户端路由。

# 使用 React 路由器导航:

React 路由器中的程序导航是执行操作或单击按钮时的重定向结果。这是一个快速的过程，主要保证你节省时间，没有任何麻烦。

您可以轻松地进行快速注册或登录操作。在编程方面，有很多种导航 React 路由器的方法。React 主要由三个核心概念支持，例如:

*   用户事件
*   渲染功能
*   状态管理

编程路由被称为符合这种思想。编程路由的效果将非常类似于任何以稳定性著称的变更。对于以编程方式改善稳定的结果来说，这是一个非常方便的选择。

该操作不能通过简单地点击链接来触发。它们是在所有场景中使用 Link 组件的合适选项。React-router I 使用 Link 元素，这样他们就可以很容易地创建链接。

这些可以用高级的 react-router 进行本地处理，它们被称为程序导航。它在内部也称为 this.context.transitionTo(…)。当你想进行导航或者从按钮添加链接时，你有很多选择。这些有一个广泛的下拉选择。

```
import { useNavigate } from 'react-router-dom'; 
const handleClick = (event) => {
}
function ActionLink() {
    function handleClick(e) {
      e.preventDefault();
      console.log('The link was clicked.');
    }
  }
  return (   
<a href="/" onClick={handleClick}>
      Click me
</a>
  );
}
```

在这个过程下，React 路由器的设计主要遵循意识形态。这些可以使用 React 路由器以编程方式导航。这是使用这三个概念对齐的主要合适选项。

# 什么是程序化导航？

通过重定向主要发生在路线上的动作的结果来实现编程导航。注册或登录操作是在该过程中快速节省更多时间的合适选项。对于使用高级技术以编程方式导航，这是一个方便的选项。流程中使用了重定向组件，它们是快速导航的合适选项。

这些是使用 React 路由器 v4+导航的合适选项。通过<redirect>组件启用。推荐该过程主要是为了帮助用户容易地浏览路线。</redirect>

下面是使用重定向组件的示例代码:

```
import React, { useState } from 'react';
import { Redirect } from 'react-router-dom';
import { userLogin } from './userAction';
import Form from './Form';
const Login = () => {
 const [isLoggedIn, setIsLoggedIn] = useState(false);  
 const handleLogin = async (userDetail) => {
  const success = await userLogin(userDetail); 
if(success) setIsLoggedIn(true);
 }  
  if (isLoggedIn) {
   return <Redirect to='/profile' />
  }
  return (  
<>   
<h1>Login</h1>   
<Form onSubmit={handleLogin} />  
</>
  )
}
export default Login;
```

**创建 React 应用:**

路由器特别提供了历史对象，它们主要是可访问的，绕过了路由上的东西。这是手动控制浏览器历史的最佳选择之一。主要原因是 React 路由器随当前 URL 而变化。

因此，对象的历史将为应用程序的各个部分提供更好的控制。使用命令行创建一个简单的 React 应用程序非常方便。经历这一过程后，它有助于节省获得结果的时间。

```
$ npx create-react-app router-sample
```

通过移动到项目目录来启动应用程序

```
$ cd router-sample$ npm start
```

这些主要从本地主机上的服务器开始，默认浏览器启动并提供服务。它也是创建新文件以帮助端点的一个方便选项，并且是安装 react-router-dom 的一个有效选项。

**安装 React 路由器:**

安装带有 npm 的 React 包是运行这个简单流程的合适选择

```
npm i react-router-dom
```

react-router-dom 包为创建新路由提供了更好的方面。访问 react js 开发服务为您咨询专业团队提供了更好的选择。

当使用<browserrouter>标签添加应用程序时，您可以轻松地访问历史对象。定义路由器链路以及路由的其他组件更容易。创建主要由 About.js 启用的新文件也是从/src 文件夹开始的</browserrouter>

```
const About = () => {
  return (   
<div>     
<h1>About page here!</h1>
<p>
Welcome! To Bosc Tech Labs
</p>   
</div>
  );
};
export default About;
```

# 使用 React 路由器 V4:

React Router v4 主要使用三种关键方法来获得组件的编程路由。

*   构图随渲染一道
*   使用上下文
*   不使用高阶组件

通常，React 路由器被包装在历史库中。这种交互主要通过窗口上的历史和浏览器中的历史来实现。这些特别提供了适合环境的记忆历史。对于 react js 开发服务和节点上的单元测试来说，这是一个非常方便的选择。

**使用 Withrouter 高阶组件:**
通常情况下，withRouter 高阶组件会随着组件的正确使用注入一个历史对象。它尤其提供了对推送和替换方法的更好访问。

```
import { withRouter } from 'react-router-dom'
// this also works with react-router-native
const Button = withRouter(({ history }) => ( 
<button   
type='button'   
onClick={() => { history.push('/new-location') }}
  >
    Click Me! 
</button>
))
```

## 结论:

上述文章的主要焦点是分享一种使用 React Router 导航的更好方法。React 有一种声明式的方法来沿着重定向构建更好的 ui，这是最值得推荐的方法。即使不使用链接，这些也适合导航。

找 [**react js 开发服务**](https://bosctechlabs.com/hire-react-developer/) ？从我们这里获得免费咨询。我们将把你的想法变成现实。

感谢您的阅读！希望你喜欢阅读，并从这篇文章中学到一些新的东西。与我们分享你的想法。因此，我们可以改善我们的内容。继续学习！！！继续分享！！！