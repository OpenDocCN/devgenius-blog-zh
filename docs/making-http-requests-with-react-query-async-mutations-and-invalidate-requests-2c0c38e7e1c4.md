# 使用 React Query 发出 HTTP 请求—异步突变和无效请求

> 原文：<https://blog.devgenius.io/making-http-requests-with-react-query-async-mutations-and-invalidate-requests-2c0c38e7e1c4?source=collection_archive---------6----------------------->

![](img/896e0a5b4fe70e8544a94ca179b6536a.png)

史蒂文·拉斯利在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

React 查询库让我们可以在 React 应用程序中轻松地发出 HTTP 请求。

在本文中，我们将了解如何使用 React Query 发出 HTTP 请求。

# `mutateAsync`

`mutateAsync`方法让我们称`mutate`为异步方式。

它返回一个承诺，让我们以异步方式提交突变请求，这不会阻塞 JavaScript 主线程。

例如，我们可以写:

```
import axios from "axios";
import React, { useState } from "react";
import { useMutation } from "react-query";export default function App() {
  const { reset, mutateAsync } = useMutation((data) =>
    axios.post("https://jsonplaceholder.typicode.com/posts", data)
  );
  const [title, setTitle] = useState(""); const onCreateTodo = async (e) => {
    e.preventDefault();
    try {
      const todo = await mutateAsync({
        title
      });
      console.log(todo);
    } catch (error) {
      console.log(error);
    } finally {
      console.log("done");
    }
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

我们调用`mutateAsync`，它返回一个承诺和来自`axios.post`调用的响应数据。

# 重试突变

使用 React Query，如果它返回一个错误，我们可以很容易地重试我们的突变 HTTP 请求。

我们只需将`retry`选项设置为我们想要重试的次数。

例如，我们可以写:

```
import axios from "axios";
import React, { useState } from "react";
import { useMutation } from "react-query";export default function App() {
  const { reset, mutateAsync } = useMutation(
    (data) => axios.post("https://jsonplaceholder.typicode.com/posts", data),
    {
      retry: 3
    }
  );
  const [title, setTitle] = useState(""); const onCreateTodo = async (e) => {
    e.preventDefault();
    try {
      const todo = await mutateAsync({
        title
      });
      console.log(todo);
    } catch (error) {
      console.log(error);
    } finally {
      console.log("done");
    }
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

如果变异请求失败，我们用一个属性设置为 3 的对象调用`useMutation`钩子来重试 3 次。

# 使查询无效

我们可以使查询无效，这样我们就可以将查询请求标记为陈旧，并自动再次发出请求。

例如，我们可以写:

`index.js`

```
import { StrictMode } from "react";
import ReactDOM from "react-dom";
import { QueryClient, QueryClientProvider } from "react-query";
import App from "./App";const queryClient = new QueryClient();
queryClient.invalidateQueries("yesNo", { exact: true });const rootElement = document.getElementById("root");
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

我们称之为:

```
queryClient.invalidateQueries("yesNo", { exact: true });
```

通过键使查询无效。

`exact`设置为`true`意味着查询请求的键在失效前必须完全匹配。

# 结论

我们异步运行变异请求，并使查询请求无效，以便用 Reacr Query 再次发出请求。