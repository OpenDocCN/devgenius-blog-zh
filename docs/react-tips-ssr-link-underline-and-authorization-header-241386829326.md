# 反应提示— SSR、链接下划线和授权标题

> 原文：<https://blog.devgenius.io/react-tips-ssr-link-underline-and-authorization-header-241386829326?source=collection_archive---------10----------------------->

![](img/dcbdac1d1b18e0c95cc85d9c6ecadef6.png)

照片由[陈彦蓉](https://unsplash.com/@spdumb2025?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# React 组件的服务器端呈现

我们可以使用 React 和 React 路由器进行服务器端渲染。

例如，我们可以写:

```
const Router = require('react-router');
const React = require("react");
const url = require("fast-url-parser");const routeHandler = (req, res, next) => {
   const path = url.parse(req.url).pathname;
   if (/^\/?api/i.test(path)) {
     return next();
   }
   Router.run(routes, path, (Handler, state) => {
     const markup = React.renderToString(<Handler routerState={state} />);
     const locals = { markup };
     res.render("main", locals);
  });
};app.get('/', routeHandler);
app.listen(3000);
```

我们调用`Router.run`将前端的路线路由到我们想要的地方。

在`Router.run`回调中，我们运行`React.renderToString`来将我们的`Handler`组件呈现为一个静态字符串。

然后我们将它传递给`locals`对象，并传递给`'main'`模板。

如果没有 React 路由器，我们可以编写:

```
import React from 'react'
import ReactDOM from 'react-dom/server'
const express = require('express');
const app = express();const Hello = (props) => {
  return (
    <div>
      <p>hello</p>
    </div>
  )
}app.get('/', (req, res) => {
  const app = ReactDOM.renderToString(<Hello />)
  const html = `<!doctype html>
    <html>
      <head>${styles}</head>
      <body>
        <div id="root">${app}</div>
      </body>
    </html>`
  res.end(html)
})app.listen(3000)
```

我们像在前面的例子中一样使用 Express，但是我们删除了 Reactv 路由器部分。

我们只需调用`ReactDOM.renderToString`将组件呈现为一个字符串。

然后我们把它直接渲染成 HTML。

在这两个示例中，我们不能向组件添加任何动态内容，比如事件处理程序或组件的状态更改，因为它被呈现为静态字符串。

如果我们需要所有这些，那么我们可以使用像 Next.js 这样的框架来使添加动态特性变得更加容易。

# 去除 React 路由器的链接组件的下划线

我们可以通过改变一些样式来去掉`Link`组件的下划线。

例如，我们可以使用`styled-components`并写出:

```
import React, { Component } from 'react';
import { Link } from 'react-router-dom';
import styled from 'styled-components';const StyledLink = styled(Link)`
  text-decoration: none;
  &:focus, &:hover, &:visited, &:link, &:active {
    text-decoration: none;
  }
`;export default (props) => <StyledLink {...props} />;
```

我们将`Link`组件传递给`styled`标签，这样我们就可以对它进行样式化。

我们在链接及其所有伪类上应用了`text-decoration: none`来移除下划线。

如果我们只设计一个链接，我们也可以写:

```
<Link style={{ textDecoration: 'none' }} to="/home">
  Home
</Link>
```

我们将一个对象传递给`style`标签来对其进行样式化。

我们还添加了`textDecoration: 'none'`来删除下划线。

# 为所有 Axios 请求附加授权头

如果我们在 React 应用程序中使用 Axios，我们可以使用它的请求拦截器特性为所有请求添加一个授权头。

例如，我们可以写:

```
axios.interceptors.request.use((config) => {
  const token = store.getState().token;
  config.headers.Authorization =  token;
  return config;
});
```

我们从中央存储中获取头，然后通过将返回值设置为`config.headers.Authorization`属性的值，将它设置为`Authorization`头的值。

我们还可以创建自己的共享模块，如下调用 Axios 请求:

```
import axios from 'axios';const httpClient = (token) => {
  const defaultOptions = {
    headers: {
      Authorization: token,
    },
  }; return {
    get: (url, options = {}) => axios.get(url, { ...defaultOptions, ...options }),
    post: (url, data, options = {}) => axios.post(url, data, { ...defaultOptions, ...options }),
    put: (url, data, options = {}) => axios.put(url, data, { ...defaultOptions, ...options }),
    delete: (url, options = {}) => axios.delete(url, { ...defaultOptions, ...options }),
  };
};
```

我们有一个`client`函数，它获取令牌并返回一个带有普通 HTTP 请求方法和请求选项的对象，包括合并到选项中的令牌。

然后我们可以在任何地方重复使用它:

```
const request = httpClient('secret');
request.get('http://example.com');
```

![](img/4c5e0f1a5fbfd5afe999bf2e42369bec.png)

由 [Karlijn Prot](https://unsplash.com/@karlijnfrederique?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

# 结论

我们可以使用 React、React Router 和 Express 进行服务器端渲染，

为了去掉 React 路由器链接的下划线，我们可以通过各种方式传递样式来删除它。

还有多种方法可以将令牌传递给所有 Axios 请求的所有路由。