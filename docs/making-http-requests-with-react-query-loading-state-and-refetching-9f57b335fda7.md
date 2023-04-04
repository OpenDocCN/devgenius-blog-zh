# 使用 React 查询发出 HTTP 请求—加载状态和重新提取

> 原文：<https://blog.devgenius.io/making-http-requests-with-react-query-loading-state-and-refetching-9f57b335fda7?source=collection_archive---------3----------------------->

![](img/eb6a09fa698271e6a7acaed8331f630a.png)

照片由[迈克尔](https://unsplash.com/@michael75?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

React 查询库让我们可以在 React 应用程序中轻松地发出 HTTP 请求。

在本文中，我们将了解如何使用 React Query 发出 HTTP 请求。

# 全局后台获取加载状态

我们可以在加载任何请求的地方获取所有请求的加载。

为此，我们可以使用`useIsFetching`钩子。

例如，我们可以写:

```
import axios from "axios";
import React from "react";
import { useQuery, useIsFetching } from "react-query";
export default function App() {
  const isFetching = useIsFetching();
  const { data } = useQuery(["todo", 1], ({ queryKey: [, id] }) => {
    return axios(`https://jsonplaceholder.typicode.com/posts/${id}`);
  }); if (isFetching) {
    return "loading";
  } return (
    <div>
      <div>{JSON.stringify(data)}</div>
    </div>
  );
}
```

调用`useIsFetching`钩子来检查我们的应用程序中是否有任何请求正在加载。

# 窗口焦点重取

默认情况下，当我们关注窗口时，React Query 会自动在后台请求新数据。

要禁用它，我们可以在创建`QueryClient`实例时将`refrechOnWindowFocus`设置为`false`。

为了对所有请求都这样做，我们编写:

`index.js`

```
import { StrictMode } from "react";
import ReactDOM from "react-dom";
import { QueryClient, QueryClientProvider } from "react-query";
import App from "./App";const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      refetchOnWindowFocus: false
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
import { useQuery, useIsFetching } from "react-query";
export default function App() {
  const isFetching = useIsFetching();
  const { data } = useQuery(["todo", 1], ({ queryKey: [, id] }) => {
    return axios(`https://jsonplaceholder.typicode.com/posts/${id}`);
  }); if (isFetching) {
    return "loading";
  } return (
    <div>
      <div>{JSON.stringify(data)}</div>
    </div>
  );
}
```

我们还可以通过编写以下内容来禁用每个查询的此选项:

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
import { useQuery, useIsFetching } from "react-query";
export default function App() {
  const isFetching = useIsFetching();
  const { data } = useQuery(
    ["todo", 1],
    ({ queryKey: [, id] }) => {
      return axios(`https://jsonplaceholder.typicode.com/posts/${id}`);
    },
    { refetchOnWindowFocus: false }
  ); if (isFetching) {
    return "loading";
  } return (
    <div>
      <div>{JSON.stringify(data)}</div>
    </div>
  );
}
```

# 禁用或暂停查询

我们可以通过将`enabled`属性设置为`false`来禁止查询自动运行。

例如，我们可以写:

```
import axios from "axios";
import React from "react";
import { useQuery } from "react-query";
export default function App() {
  const { data, refetch } = useQuery(
    ["todo", 1],
    ({ queryKey: [, id] }) => {
      return axios(`https://jsonplaceholder.typicode.com/posts/${id}`);
    },
    { enabled: false }
  ); return (
    <div>
      <button onClick={() => refetch()}>Fetch Todo</button>
      <div>{JSON.stringify(data)}</div>
    </div>
  );
}
```

我们将`enabled`设置为`false`,以便在组件挂载时不会发出请求。

要发出请求，我们可以使用`refetch`函数发出请求。

# 结论

我们可以获得请求的加载状态，并控制何时使用 React Query 发出请求。