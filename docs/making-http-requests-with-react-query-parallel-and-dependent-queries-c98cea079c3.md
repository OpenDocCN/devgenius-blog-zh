# 使用 React Query 发出 HTTP 请求—并行和相关查询

> 原文：<https://blog.devgenius.io/making-http-requests-with-react-query-parallel-and-dependent-queries-c98cea079c3?source=collection_archive---------4----------------------->

![](img/e23954c662c5181bfd5d8f7a59873adb.png)

照片由 [Paulius Dragunas](https://unsplash.com/@paulius005?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

React 查询库让我们可以在 React 应用程序中轻松地发出 HTTP 请求。

在本文中，我们将了解如何使用 React Query 发出 HTTP 请求。

# 并行查询

有时，我们可能希望同时发出多个 GET 请求。

为此，我们可以添加多个`useQuery`挂钩:

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
  const { data } = useQuery({
    queryKey: ["todo", 1],
    queryFn: ({ queryKey: [, id] }) => {
      return axios(`https://jsonplaceholder.typicode.com/posts/${id}`);
    }
  });
  const { data: yesNoData } = useQuery("yesNo", () =>
    axios("https://yesno.wtf/api")
  ); return (
    <div>
      <div>{JSON.stringify(data)}</div>
      <div>{JSON.stringify(yesNoData)}</div>
    </div>
  );
}
```

我们有`useQuery`钩子来并行发出 2 个请求。

# 用`useQueries Hook`进行动态并行查询

我们可以用`useQueries`钩子添加动态并行查询。

它返回一组查询结果。

例如，我们可以写:

```
import axios from "axios";
import React from "react";
import { useQueries } from "react-query";
export default function App() {
  const todos = useQueries(
    Array(5)
      .fill()
      .map((_, i) => i + 1)
      .map((id) => {
        return {
          queryKey: ["todo", id],
          queryFn: ({ queryKey: [, id] }) => {
            return axios(`https://jsonplaceholder.typicode.com/posts/${id}`);
          }
        };
      })
  ); return (
    <div>
      <div>{JSON.stringify(todos)}</div>
    </div>
  );
}
```

我们用一组查询对象调用`useQueries`。

我们调用`map`来映射我们创建的数字数组:

```
Array(5)
  .fill()
  .map((_, i) => i + 1)
```

来查询对象。

而`todos`是查询对象的数组。

# 相关查询

我们还可以发出多个相互依赖的 HTTP 请求。

为此，我们写道:

```
import axios from "axios";
import React from "react";
import { useQuery } from "react-query";
export default function App() {
  const { data: id } = useQuery("id", () => Promise.resolve(1)); const { data } = useQuery(
    ["todo", id],
    ({ queryKey: [, id] }) => {
      return axios(`https://jsonplaceholder.typicode.com/posts/${id}`);
    },
    {
      enabled: Boolean(id)
    }
  ); return (
    <div>
      <div>{JSON.stringify(data)}</div>
    </div>
  );
}
```

进行一个接一个的查询。

我们调用第一个`useQuery`钩子来为我们的 todo 获取一个`id`。

然后我们再次调用`useQuery`将`id`传递给回调函数。

我们将`enabled`属性设置为一个布尔表达式，以指示我们希望查询何时运行。

我们将它设置为当`id`为真时运行第二个查询，这应该只发生在我们从第一个`useQuery`钩子得到`id`的时候。

# 结论

我们可以使用 React Query 轻松地进行并行或顺序查询。