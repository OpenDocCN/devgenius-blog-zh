# 使用 Wouter 将路由添加到 React 应用程序

> 原文：<https://blog.devgenius.io/add-routing-to-a-react-app-with-wouter-2d2b3de0cab5?source=collection_archive---------5----------------------->

![](img/f2c1787e0309b3f252cd5918613eb09d.png)

照片由[迭戈·希门尼斯](https://unsplash.com/@diegojimenez?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Wouter 是一个库，允许我们根据 URL 加载 React 组件。

在本文中，我们将看看如何使用 Wouter 向 React 应用程序添加路由。

# 装置

要安装它，我们运行:

```
npm i wouter
```

# 入门指南

我们可以通过使用`Route`组件向 React 应用程序添加路线。

例如，我们可以写:

```
import React from "react";
import { Link, Route } from "wouter";const InboxPage = () => <p>inbox</p>;export default function App() {
  return (
    <div>
      <Link href="/users/1">
        <a className="link">Profile</a>
      </Link> <Route path="/about">About Us</Route>
      <Route path="/users/:name">
        {(params) => <div>Hello, {params.name}!</div>}
      </Route>
      <Route path="/inbox" component={InboxPage} />
    </div>
  );
}
```

我们添加了`Router`组件，让我们可以将地图路线添加到 React 组件中。

`:name`是一个 URL 参数。

我们可以用`params.name`得到它的值。

`Link`让我们创建装载路线的链接。

我们还可以添加`component`道具来添加组件，就像我们对`InboxPage`所做的那样。

# 用户出口挂钩

我们可以使用`useRouter`钩子从路由中提取 URL 参数。

例如，我们写道:

```
import React from "react";
import { Transition } from "react-transition-group";
import { useRoute } from "wouter";export default function App() {
  const [match, params] = useRoute("/users/:id"); return (
    <div>
      <Transition in={match}>
        <p>Hi, this is: {params.id}</p>
      </Transition>
    </div>
  );
}
```

我们用 tyhe `useRoute`钩子解析路由 URL，将它传递给钩子。

然后我们获取`params`对象来获取 URL 参数。

我们还有`Transition`组件来在加载路线时添加转换。

# `useLocation Hook`

我们可以使用`useLocation`钩子返回`location`对象，以转到不同的路线。

例如，我们写道:

```
import React from "react";
import { Link, Route, useLocation } from "wouter";const InboxPage = () => {
  const [location, setLocation] = useLocation(); return (
    <div>
      <p>{`The current page is: ${location}`}</p>
      <p>
        <a onClick={() => setLocation("/about")}>Click to update</a>
      </p>
    </div>
  );
};export default function App() {
  return (
    <div>
      <Link href="/inbox">
        <a>Inbox</a>
      </Link>
      <Route path="/about">
        <p>About Us</p>
      </Route>
      <Route path="/users/:name">
        {(params) => <div>Hello, {params.name}!</div>}
      </Route>
      <Route path="/inbox" component={InboxPage} />
    </div>
  );
}
```

向`InboxPage`组件添加链接。

该链接使用`useLocation`挂钩的`setLocation`功能转到`about`路线。

因此，当我们点击“点击更新”链接时，我们会看到关于我们的内容。

# 自定义位置挂钩

我们可以定制`useLocation`钩子来做我们想做的事情。

例如，我们可以从散列中提取 URL 段，而不是从 URL 参数中提取。

为此，我们写道:

```
import React, { useCallback, useEffect, useState } from "react";
import { Link, Route, Router, useLocation } from "wouter";const currentLocation = () => {
  return window.location.hash.replace(/^#/, "") || "/";
};const useHashLocation = () => {
  const [loc, setLoc] = useState(currentLocation()); useEffect(() => {
    const handler = () => setLoc(currentLocation());
    window.addEventListener("hashchange", handler);
    return () => window.removeEventListener("hashchange", handler);
  }, []);
  const navigate = useCallback((to) => (window.location.hash = to), []);
  return [loc, navigate];
};const InboxPage = () => {
  const [location] = useLocation(); return (
    <div>
      <p>{`The current page is: ${location}`}</p>
    </div>
  );
};export default function App() {
  return (
    <div>
      <Link href="/inbox">
        <a>Inbox</a>
      </Link>
      <Router hook={useHashLocation}>
        <Route path="/about">
          <p>About Us</p>
        </Route>
        <Route path="/users/:name">
          {(params) => <div>Hello, {params.name}!</div>}
        </Route>
        <Route path="/inbox" component={InboxPage} />
      </Router>
    </div>
  );
}
```

我们使用了`currentLocation`函数来获取经过散列后的 URL 部分。

然后我们将它传递给`useState`钩子来设置初始的 URL 哈希值。

我们监听`hashchange`事件来获取 URL 散列部分的变化。

如果有任何变化，我们返回`loc`位置。

`navigate`导航到 URL 的哈希部分。

然后我们把钩子传入`hook`道具来使用它。

所以现在当我们转到`/#/users/1`时，我们看到显示的`Hello, 1!`。

当我们到了`/#/inbox`，我们看到了`The current page is: /inbox`

并且转到`/#/about`渲染`About Us`。

# 结论

Wouter 是 React 应用程序的轻量级路由器。