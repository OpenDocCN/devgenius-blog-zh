# 使用 setQueryData 方法更新 React 查询缓存

> 原文：<https://blog.devgenius.io/update-react-query-cache-with-the-setquerydata-method-95533be3c407?source=collection_archive---------4----------------------->

![](img/eeba9d840338806d164ed679e71372db.png)

莎伦·麦卡琴在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

有时，我们可能希望更新 React 查询缓存，以便在客户端更新用于呈现 HTML 的数据，从而显示最新的数据，而无需再次从服务器获取数据。

在本文中，我们将了解如何使用 React Query 的`setQueryData`方法在客户端更新查询缓存数据。

# 使用 setQueryData 方法更新发出请求时缓存的数据

我们可以使用 React 查询的`queryClient`的`setQueryData`方法来更新来自 HTTP 请求的缓存响应数据。

为此，我们写道:

`index.js`

```
import { StrictMode } from "react";
import ReactDOM from "react-dom";
import { QueryClient, QueryClientProvider } from "react-query";import App from "./App";
export const queryClient = new QueryClient();const rootElement = document.getElementById("root");
ReactDOM.render(
  <StrictMode>
    <QueryClientProvider client={queryClient}>
      <App />
    </QueryClientProvider>
  </StrictMode>,
  rootElement
);
```

`App.js`

```
import axios from "axios";
import React from "react";
import { useQuery } from "react-query";
import { queryClient } from "./index";export default function App() {
  const { data } = useQuery("yesno", () => axios.get("https://yesno.wtf/api"));const setData = () => {
    queryClient.setQueryData("yesno", (data) => {
      return {
        data: {
          ...data.data,
          answer: data.data.answer === "yes" ? "no" : "yes"
        }
      };
    });
  }; return (
    <div>
      <button onClick={setData}>toggle</button>
      <div>{data?.data?.answer}</div>
    </div>
  );
}
```

在`index.js`中，我们创建了一个与`QueryClientProvider`组件一起使用的新的`QueryClient`实例，这样我们就可以使用 React 查询钩子来发出请求。

然后在`App.js`中，我们使用`useQuery`钩子来发出请求。

我们将查询的名称作为第一个参数传入。

第二个参数是一个函数，它通过 HTTP 请求响应返回一个承诺。

返回对象的`data`属性有响应数据。

在`setData`函数中，我们用传递给`useQuery`第一个参数的我们想要更新的请求的标识符来调用`queryClient.setQueryData`。

第二个参数是一个回调，它使用第一个参数中的标识符获取请求的缓存响应数据，我们返回的对象将是使用给定标识符缓存的项目的新值。

我们从`setData`中的响应切换`data.data.answer`的值。

当我们点击切换按钮时，我们称之为。

因此，当我们点击切换按钮时，我们应该看到`data.data.answer`值在`'yes'`和`'no'`之间切换。

这是因为我们用传入第一个参数`sertQueryData`的标识符来更新请求的查询缓存数据条目，这是我们在客户端想要的。

# 结论

我们可以用 React Query 的查询客户端的`setQueryData`方法在客户端更新缓存的响应数据。