# 使用 React Query-Query 函数发出 HTTP 请求

> 原文：<https://blog.devgenius.io/making-http-requests-with-react-query-query-functions-cc8930830279?source=collection_archive---------11----------------------->

![](img/4bd02c8cc5a713c15c9794a526c5a683.png)

Syed Rifat Hossain 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

React 查询库让我们可以在 React 应用程序中轻松地发出 HTTP 请求。

在本文中，我们将了解如何使用 React Query 发出 HTTP 请求。

# 查询功能

查询函数是返回承诺的函数，它被传递到`useQuery`钩子的第二个参数中。

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
  const { data } = useQuery("yesNo", () => axios("https://yesno.wtf/api")); return <div>{JSON.stringify(data)}</div>;
}
```

通过`axios`功能返回承诺返回。

返回的承诺数据将被设置为`data`属性的值。

# 处理和抛出错误

React Query 期望在发生错误时在查询函数中抛出错误。

如果不行，我们就自己扔了它。

例如，我们可以写:

```
import React from "react";
import { useQuery } from "react-query";
export default function App() {
  const { error } = useQuery("todo", async () => {
    const response = await fetch(
      "https://jsonplaceholder.typicode.com/posts/100000000000"
    );
    if (!response.ok) {
      throw new Error("Network response was not ok");
    }
    return response.json();
  }); return <div>{error && error.message}</div>;
}
```

如果我们使用 Fetch API 发出 HTTP 请求，那么我们必须检查`response.ok`属性是否是`true`。

如果不是，那么我们必须抛出一个错误，让 React Query 知道发生了错误。

这必须完成，因为当我们得到非 200 系列响应时`fetch`不会抛出错误。

如果`response.ok`是`true`，我们通过调用`response.json()`返回响应。

# 查询对象

我们可以传入一个查询对象，而不是传入每个单独的参数来进行 GET 请求。

例如，我们可以写:

```
import axios from "axios";
import React from "react";
import { useQuery } from "react-query";
export default function App() {
  const { data } = useQuery({
    queryKey: ["todo", 1],
    queryFn: ({ queryKey: [, id] }) => {
      return axios(`https://jsonplaceholder.typicode.com/posts/${id}`);
    }
  }); return <div>{JSON.stringify(data)}</div>;
}
```

`queryKey`有请求的标识符。

`queryFn`是发出请求的函数。

# 结论

我们可以向`useQuery`钩子传递返回请求承诺的函数，用 React 发出 GET 请求。