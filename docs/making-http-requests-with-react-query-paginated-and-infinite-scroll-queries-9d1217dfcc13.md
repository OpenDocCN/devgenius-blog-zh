# 使用 React Query 发出 HTTP 请求—分页和无限滚动查询

> 原文：<https://blog.devgenius.io/making-http-requests-with-react-query-paginated-and-infinite-scroll-queries-9d1217dfcc13?source=collection_archive---------3----------------------->

![](img/2aa9626825fbf0d9b128c1b9a3d7f721.png)

托马斯·格里斯贝克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

React 查询库让我们可以在 React 应用程序中轻松地发出 HTTP 请求。

在本文中，我们将了解如何使用 React Query 发出 HTTP 请求。

# 分页查询

我们可以使用`useQuery`钩子像处理任何其他查询一样进行分页查询。

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
  const [page, setPage] = React.useState(1);
  const { data: { data: { data, totalPages } } = { data: [] } } = useQuery(
    ["todo", page],
    ({ queryKey: [, page] }) => {
      return axios(
        `https://api.instantwebtools.net/v1/passenger?page=${page}&size=10`
      );
    }
  ); return (
    <div>
      <button onClick={(p) => setPage(Math.max(1, page - 1))}>prev</button>
      <button onClick={(p) => setPage(Math.min(totalPages, page + 1))}>
        next
      </button>
      <div>
        {Array.isArray(data) &&
          data.map((d, i) => <p key={`${i}-${d.name}`}>{d.name}</p>)}
      </div>
    </div>
  );
}
```

我们只需创建`page`状态，并将其传递给请求以进行分页查询。

我们将看到有很多闪烁，因为每个查询都将作为一个新的查询。

为了使查询没有额外的闪烁，我们将`keepPreviousData`选项设置为`true`:

`App.js`

```
import axios from "axios";
import React from "react";
import { useQuery } from "react-query";
export default function App() {
  const [page, setPage] = React.useState(1);
  const { data: { data: { data, totalPages } } = { data: [] } } = useQuery(
    ["todo", page],
    ({ queryKey: [, page] }) => {
      return axios(
        `https://api.instantwebtools.net/v1/passenger?page=${page}&size=10`
      );
    },
    {
      keepPreviousData: true
    }
  ); return (
    <div>
      <button onClick={(p) => setPage(Math.max(1, page - 1))}>prev</button>
      <button onClick={(p) => setPage(Math.min(totalPages, page + 1))}>
        next
      </button>
      <div>
        {Array.isArray(data) &&
          data.map((d, i) => <p key={`${i}-${d.name}`}>{d.name}</p>)}
      </div>
    </div>
  );
}
```

# 无限查询

我们还可以使用 React Query 来请求无限滚动。

为此，我们使用`useInfiniteQuery`钩子。

这让我们可以显示到目前为止已经获取的所有项目。

例如，我们可以写:

```
import axios from "axios";
import React from "react";
import { useInfiniteQuery } from "react-query";
export default function App() {
  const [page, setPage] = React.useState(1);
  const { data, fetchNextPage } = useInfiniteQuery(
    "names",
    ({ pageParam = 1 }) => {
      return axios(
        `https://api.instantwebtools.net/v1/passenger?page=${pageParam}&size=10`
      );
    },
    {
      getNextPageParam: (lastPage) => {
        const { totalPages } = lastPage.data;
        return page < totalPages ? page + 1 : totalPages;
      }
    }
  ); return (
    <div>
      <div>
        {data &&
          Array.isArray(data.pages) &&
          data?.pages.map((group, i) => {
            return group?.data?.data.map((d, i) => (
              <p key={`${i}-${d.name}`}>{d.name}</p>
            ));
          })}
      </div>
      <button
        onClick={() => {
          setPage(page + 1);
          fetchNextPage();
        }}
      >
        load more
      </button>
    </div>
  );
}
```

我们调用`useInfiniteQuery`钩子，将请求的标识符作为第一个参数。

第二个参数是查询函数。

它将带有`pageParam`属性的对象作为参数。

`pageParam`是`getNextPageParam`函数的返回值。

`getNextPageParam`返回`pageParam`的下一个值。

然后在 JSX 中，我们用页面数组返回`data.pages`数组。

并且我们调用`map`用页面中的数据来呈现数据。

加载更多按钮调用`fetchNextPage`获取下一页。

# 结论

我们可以用 React Query 创建分页和无限滚动请求。