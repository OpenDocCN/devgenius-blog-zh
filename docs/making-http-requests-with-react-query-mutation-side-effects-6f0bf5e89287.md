# 使用 React Query 生成 HTTP 请求—变异副作用

> 原文：<https://blog.devgenius.io/making-http-requests-with-react-query-mutation-side-effects-6f0bf5e89287?source=collection_archive---------7----------------------->

![](img/c1709fbc96bd40f4aa7716adae5b79c9.png)

照片由[Aylin obano Lu](https://unsplash.com/@zynpayln?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

React 查询库让我们可以在 React 应用程序中轻松地发出 HTTP 请求。

在本文中，我们将了解如何使用 React Query 发出 HTTP 请求。

# 突变副作用

我们可以观察发生突变时发出的事件。

例如，我们可以写:

```
import axios from "axios";
import React, { useState } from "react";
import { useMutation } from "react-query";export default function App() {
  const { reset, mutate } = useMutation(
    (data) => axios.post("https://jsonplaceholder.typicode.com/posts", data),
    {
      onMutate: (variables) => {
        console.log(variables);
        return {};
      },
      onError: (error, variables, context) => {
        console.log(error, variables, context);
      },
      onSuccess: (data, variables, context) => {
        console.log(data, variables, context);
      },
      onSettled: (data, error, variables, context) => {
        console.log(data, error, variables, context);
      }
    }
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

发出突变请求时，运行`onMutate`方法。

`variables`有来自`data`参数的突变数据。

`onError`当变异出现错误时运行。

`error`有错误的对象。

`variables`和以前一样。

`context`具有包含变更请求数据的上下文数据。

`onSuccess`在变异请求成功时运行。

`data`有突变反应数据。

`variables`和`context`与其他回调参数相同。

`onSettled`在变异请求完成时运行，无论成功与否。

所有参数都和之前一样。

我们也可以向`mutate`方法调用添加相同的回调。

例如，我们可以写:

```
import axios from "axios";
import React, { useState } from "react";
import { useMutation } from "react-query";export default function App() {
  const { reset, mutate } = useMutation(
    (data) => axios.post("https://jsonplaceholder.typicode.com/posts", data),
    {
      onMutate: (variables) => {
        console.log(variables);
        return {};
      },
      onError: (error, variables, context) => {
        console.log(error, variables, context);
      },
      onSuccess: (data, variables, context) => {
        console.log(data, variables, context);
      },
      onSettled: (data, error, variables, context) => {
        console.log(data, error, variables, context);
      }
    }
  );
  const [title, setTitle] = useState(""); const onCreateTodo = (e) => {
    e.preventDefault();
    mutate(
      {
        title
      },
      {
        onMutate: (variables) => {
          console.log(variables);
          return {};
        },
        onError: (error, variables, context) => {
          console.log(error, variables, context);
        },
        onSuccess: (data, variables, context) => {
          console.log(data, variables, context);
        },
        onSettled: (data, error, variables, context) => {
          console.log(data, error, variables, context);
        }
      }
    );
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

我们添加到作为第二个参数`mutate`传入的对象的回调将在我们添加到`useMutation`钩子的回调之后运行。

# 结论

我们可以向传入`useMutation`钩子或`mutate`方法的对象添加回调，以观察在使用 React Query 发出突变请求时触发的任何事件。