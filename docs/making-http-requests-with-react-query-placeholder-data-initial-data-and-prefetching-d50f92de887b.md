# 使用 React 查询发出 HTTP 请求—占位符数据、初始数据和预取

> 原文：<https://blog.devgenius.io/making-http-requests-with-react-query-placeholder-data-initial-data-and-prefetching-d50f92de887b?source=collection_archive---------3----------------------->

![](img/424c2d53389df2b6050bd1aae1e6bb64.png)

劳里·吉布森在 Unsplash 的照片

React 查询库让我们可以在 React 应用程序中轻松地发出 HTTP 请求。

在本文中，我们将了解如何使用 React Query 发出 HTTP 请求。

# 占位符数据

我们可以添加在加载请求时设置的占位符数据。

为此，我们通过编写以下代码来设置`placeholderData`属性:

```
import axios from "axios";
import React from "react";
import { useQuery } from "react-query";
export default function App() {
  const { data } = useQuery("yesNo", () => axios("https://yesno.wtf/api"), {
    placeholderData: {}
  }); return <div>{JSON.stringify(data)}</div>;
}
```

我们还可以从缓存中加载占位符数据。

为此，我们写道:

```
import axios from "axios";
import React from "react";
import { useQuery, useQueryClient } from "react-query";
export default function App() {
  const queryClient = useQueryClient();
  const { data } = useQuery("yesNo", () => axios("https://yesno.wtf/api"), {
    placeholderData: () => {
      return queryClient.getQueryData("yesNo");
    }
  }); return <div>{JSON.stringify(data)}</div>;
}
```

我们用带有请求标识符的`queryClient.getQueryData`方法获取缓存的数据。

# 初始查询数据

此外，我们可以设置初始查询数据，这些数据是在第一次进行查询时设置的。

为此，我们写道:

```
import axios from "axios";
import React from "react";
import { useQuery } from "react-query";
export default function App() {
  const { data } = useQuery("yesNo", () => axios("https://yesno.wtf/api"), {
    initialData: {}
  }); return <div>{JSON.stringify(data)}</div>;
}
```

我们还可以用`staleTime`属性设置数据被认为是新的时间，以毫秒为单位。

为此，我们写道:

```
import axios from "axios";
import React from "react";
import { useQuery } from "react-query";
export default function App() {
  const { data } = useQuery("yesNo", () => axios("https://yesno.wtf/api"), {
    initialData: {},
    staleTime: 1000
  }); return <div>{JSON.stringify(data)}</div>;
}
```

我们将`stateTime`设置为 1000 毫秒，以使该时间之后的初始数据无效。

我们还可以将`initialData`设置为一个函数，只在第一次查询时设置初始数据:

```
import axios from "axios";
import React from "react";
import { useQuery } from "react-query";
export default function App() {
  const { data } = useQuery("yesNo", () => axios("https://yesno.wtf/api"), {
    initialData: () => {
      return {};
    }
  }); return <div>{JSON.stringify(data)}</div>;
}
```

# 预取

React Query 允许我们预取数据。

例如，我们可以写:

```
import axios from "axios";
import React, { useEffect } from "react";
import { useQuery, useQueryClient } from "react-query";
export default function App() {
  const queryClient = useQueryClient();
  const { data } = useQuery("yesNo", () => axios("https://yesno.wtf/api")); const prefetch = async () => {
    await queryClient.prefetchQuery("yesNo", () => Promise.resolve({}));
  }; useEffect(() => {
    prefetch();
  }, []); return <div>{JSON.stringify(data)}</div>;
}
```

调用`queryClient.prefetchQuery`来预取对标识符为`'yesNo'`的请求的响应。

# 结论

我们可以使用 React Query 预取数据、设置占位符数据以及为请求设置初始数据。