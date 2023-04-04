# 如何在 React useEffect Hook 中使用 Axios HTTP 客户端？

> 原文：<https://blog.devgenius.io/how-to-use-the-axios-http-client-in-react-useeffect-hook-804680a9a3a9?source=collection_archive---------0----------------------->

![](img/ce03bca8cde0a1c5b532e4a8c835f0f7.png)

由[路易斯·里德](https://unsplash.com/@_louisreed?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在许多情况下，我们需要在代码中发出 HTTP 请求，以获取或提交 React 组件中的数据。

Axios 是一个流行的 HTTP 客户端库，我们可以使用它轻松地发出 HTTP 请求。

在本文中，我们将了解如何使用带有`useEffect`钩子的 Axios HTTP 客户端来发出 HTTP 请求。

# 使用带有 React useEffect 挂钩的 Axios HTTP 客户端

当组件挂载时，我们可以通过在第二个参数中使用空数组调用`useEffect`钩子来发出 HTTP 请求。

例如，我们可以写:

```
import React, { useEffect, useState } from "react";
import axios from "axios";export default function App() {
  const [data, setData] = useState([]); const getData = async () => {
    const { data } = await axios.get(`https://yesno.wtf/api`);
    setData(data);
  }; useEffect(() => {
    getData();
  }, []); return <div>{JSON.stringify(data)}</div>;
}
```

我们定义了`getData`函数，用`axios.get`方法发出 GET 请求。

该函数是异步的，因为`axios`方法返回一个承诺。

我们只是传入一个 URL 来发出 GET 请求。

我们必须在`useEffect`钩子之外定义`getData`函数，因为`useEffect`回调应该是同步函数。

第二个参数中的空数组意味着我们只在组件挂载时发出请求。

我们调用`setData`来设置`data`状态，并在 JSX 代码中呈现数据。

如果我们想在状态或属性改变时发出 HTTP 请求，那么我们应该将这些请求传递给第二个参数中的数组。

# 使用 axios-hooks 库

我们还可以使用 axios-hooks 库，让我们通过使用 axios 库的定制钩子发出请求。

要使用它，我们通过编写以下内容来安装它:

```
npm install axios axios-hooks
```

然后我们写道:

```
import React from "react";
import useAxios from "axios-hooks";export default function App() {
  const [{ data, loading, error }, refetch] = useAxios("https://yesno.wtf/api"); if (loading) return <p>Loading...</p>;
  if (error) return <p>Error!</p>; return <div>{JSON.stringify(data)}</div>;
}
```

我们调用`useAxios`来返回一个包含具有`data`、`loading`和`error`属性的对象的数组。

`data`有响应数据。

`loading`已加载状态。如果请求正在加载，则为`true`。

`error`有错误状态。如果请求导致错误，则为`true`。

我们可以调用`refetch`函数再次发出请求。

# 结论

要将 Axios 与`useEffect`挂钩一起使用，我们只需定义一个异步函数，并在`useEffect`回调中调用它。

或者，我们可以使用 axios-hooks 库提供的`useAxios`钩子。