# 使用 React Query 发出 HTTP 请求—查询重试

> 原文：<https://blog.devgenius.io/making-http-requests-with-react-query-query-retries-a138f91fbe10?source=collection_archive---------1----------------------->

![](img/929c2315d080ba3a38637ceb8489c55a.png)

菲利帕·罗斯-泰特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

React 查询库让我们可以在 React 应用程序中轻松地发出 HTTP 请求。

在本文中，我们将了解如何使用 React Query 发出 HTTP 请求。

# 查询重试次数

如果请求失败，`useQuery`钩子将自动重试请求。

例如，我们可以写:

`index.js`

```
import { StrictMode } from "react";
import ReactDOM from "react-dom";
import { QueryClient, QueryClientProvider } from "react-query";
import App from "./App";const queryClient = new QueryClient();const rootElement = document.getElementById("root");
ReactDOM.render(
  <QueryClientProvider client={queryClient}>
    <StrictMode>
      <App />
    </StrictMode>
  </QueryClientProvider>,
  rootElement
);
```

`App.js`

```
import axios from "axios";
import React from "react";
import { useQuery } from "react-query";
export default function App() {
  const { data } = useQuery(
    ["todo", 1],
    ({ queryKey: [, id] }) => {
      return axios(`https://jsonplaceholder.typicode.com/posts/${id}`);
    },
    { retry: 3 }
  ); return (
    <div>
      <div>{JSON.stringify(data)}</div>
    </div>
  );
}
```

我们将`retry`设置为 3，如果请求失败，就重试 3 次。

此外，我们可以将`retry`设置为`true`来重试无限次。

并且我们可以将`retry`设置为`false`永远不重试。

# 重试延迟

我们还可以用`retryDelay`属性设置重试延迟。

该值应以毫秒为单位。

例如，我们可以写:

`index.js`

```
import { StrictMode } from "react";
import ReactDOM from "react-dom";
import { QueryClient, QueryClientProvider } from "react-query";
import App from "./App";const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      retryDelay: (attemptIndex) => Math.min(1000 * 2 ** attemptIndex, 30000)
    }
  }
});const rootElement = document.getElementById("root");
ReactDOM.render(
  <QueryClientProvider client={queryClient}>
    <StrictMode>
      <App />
    </StrictMode>
  </QueryClientProvider>,
  rootElement
);
```

`App.js`

```
import axios from "axios";
import React from "react";
import { useQuery } from "react-query";
export default function App() {
  const { data } = useQuery(["todo", 1], ({ queryKey: [, id] }) => {
    return axios(`https://jsonplaceholder.typicode.com/posts/${id}`);
  }); return (
    <div>
      <div>{JSON.stringify(data)}</div>
    </div>
  );
}
```

在`index.js`中，我们有一个`retryDelay`方法，它接受的`attemptIndex`是尝试次数减 1。

如果请求失败，我们返回重试前的毫秒数。

此外，我们可以用传递给`useQuery`钩子的选项中的`retryDelay`属性覆盖单个请求的重试间隔:

`index.js`

```
import { StrictMode } from "react";
import ReactDOM from "react-dom";
import { QueryClient, QueryClientProvider } from "react-query";
import App from "./App";const queryClient = new QueryClient();const rootElement = document.getElementById("root");
ReactDOM.render(
  <QueryClientProvider client={queryClient}>
    <StrictMode>
      <App />
    </StrictMode>
  </QueryClientProvider>,
  rootElement
);
```

`App.js`

```
import axios from "axios";
import React from "react";
import { useQuery } from "react-query";
export default function App() {
  const { data } = useQuery(
    ["todo", 1],
    ({ queryKey: [, id] }) => {
      return axios(`https://jsonplaceholder.typicode.com/posts/${id}`);
    },
    {
      retryDelay: 1000
    }
  ); return (
    <div>
      <div>{JSON.stringify(data)}</div>
    </div>
  );
}
```

# 结论

我们可以使用 React Query 轻松设置查询重试间隔。