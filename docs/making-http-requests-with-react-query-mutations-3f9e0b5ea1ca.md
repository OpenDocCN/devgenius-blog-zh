# 使用 React 查询生成 HTTP 请求—突变

> 原文：<https://blog.devgenius.io/making-http-requests-with-react-query-mutations-3f9e0b5ea1ca?source=collection_archive---------5----------------------->

![](img/9f5193d39f5121b8fd96b7e20641f7f5.png)

照片由 [Elena Mozhvilo](https://unsplash.com/@miracleday?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

React 查询库让我们可以在 React 应用程序中轻松地发出 HTTP 请求。

在本文中，我们将了解如何使用 React Query 发出 HTTP 请求。

# 突变

突变允许我们通过创建、更新和删除 HTTP 请求来改变服务器上的数据。

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
import { useMutation } from "react-query";export default function App() {
  const { mutate, isLoading, isError, isSuccess } = useMutation((data) =>
    axios.post("https://jsonplaceholder.typicode.com/posts", data)
  ); return (
    <div>
      <div>{isLoading && "loading"}</div>
      <div>{isError && "error"}</div>
      <div>{isSuccess && "success"}</div>
      <button
        onClick={() => {
          mutate({
            title: "foo",
            body: "bar",
            userId: 1
          });
        }}
      >
        Add Todo
      </button>
    </div>
  );
}
```

我们用一个回调函数调用`useMutation`钩子，这个回调函数让我们向 API 发出 POST 请求以提交一些数据。

回调应该返回一个承诺。

请求有效载荷存储在`payload`参数中。

钩子返回让我们发出请求的`mutate`函数。

当请求正在加载时,`isLoading`为`true`。

`isError`是请求失败时的`true`。

当请求成功完成时`isSuccess`为`true`。

或者，我们可以用属性`status`替换`isLoading`、`isSuccess`和`isError`:

```
import axios from "axios";
import React from "react";
import { useMutation } from "react-query";export default function App() {
  const { mutate, status } = useMutation((data) =>
    axios.post("https://jsonplaceholder.typicode.com/posts", data)
  ); return (
    <div>
      <div>{status === "loading" && "loading"}</div>
      <div>{status === "error" && "error"}</div>
      <div>{status === "success" && "success"}</div>
      <button
        onClick={() => {
          mutate({
            title: "foo",
            body: "bar",
            userId: 1
          });
        }}
      >
        Add Todo
      </button>
    </div>
  );
}
```

`'loading'`状态是加载请求时的状态。

`'error'`状态是请求出错时的状态。

`'success'`状态是请求成功时的状态。

# 重置突变状态

我们可以在请求完成后清除变异请求状态。

为此，我们可以调用`reset`方法:

```
import axios from "axios";
import React, { useState } from "react";
import { useMutation } from "react-query";export default function App() {
  const { reset, mutate } = useMutation((data) =>
    axios.post("https://jsonplaceholder.typicode.com/posts", data)
  );
  const [title, setTitle] = useState(""); const onCreateTodo = (e) => {
    e.preventDefault();
    mutate({
      title
    });
  }; return (
    <div>
      <form onSubmit={onCreateTodo}>
        <input
          type="text"
          value={title}
          onChange={(e) => setTitle(e.target.value)}
        />
        <br />
        <button type="submit">Create Todo</button>
        <button type="button" onClick={() => reset()}>
          reset
        </button>
      </form>
    </div>
  );
}
```

当我们点击重置由`useMutation`钩子返回的状态时，我们调用`reset`方法。

# 结论

我们可以用 React Query `useMutation`钩子请求改变服务器上的数据。