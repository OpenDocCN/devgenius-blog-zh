# Top React 挂钩—状态和请求

> 原文：<https://blog.devgenius.io/top-react-hooks-states-and-requests-e5ea438c9dee?source=collection_archive---------6----------------------->

![](img/1e85fbb06032ddf8cba906d2ba0c44c3.png)

照片由[维达尔·诺德里-马西森](https://unsplash.com/@vidarnm?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Hooks 包含了 React 应用程序中的逻辑代码。

我们可以创建自己的钩子，使用别人提供的钩子。

在本文中，我们将看看一些有用的 React 挂钩。

# 钩子状态

我们可以使用 Hookstate 钩子来改变应用程序中的全局和局部状态。

为了使用它，我们运行:

```
npm install --save @hookstate/core
```

或者:

```
yarn add @hookstate/core
```

来安装它。

然后，我们可以通过编写以下内容来管理全局状态:

```
import React from "react";
import { createState, useState } from "@hookstate/core";
const globalState = createState(0);export default function App() {
  const state = useState(globalState);
  return (
    <>
      <p>Counter value: {state.get()}</p>
      <button onClick={() => state.set(p => p + 1)}>Increment</button>
    </>
  );
}
```

我们用`createState`函数创建一个全局状态对象，然后将它传递给 Hookstate 的`useState`钩子。

然后我们可以用`get`和`set`分别获取和设置状态。

要改变本地状态，我们可以写:

```
import React from "react";
import { useState } from "@hookstate/core";export default function App() {
  const state = useState(0);
  return (
    <>
      <p>Counter value: {state.get()}</p>
      <button onClick={() => state.set(p => p + 1)}>Increment</button>
    </>
  );
}
```

唯一的区别是我们将状态值直接传递给了`useState`钩子。

状态可以嵌套。

例如，我们可以写:

```
import React from "react";
import { useState, self } from "@hookstate/core";export default function App() {
  const state = useState([{ name: "Untitled" }]);console.log(state);
  return (
    <>
      {state.map((task, index) => (
        <p>
          {task.name.get()} {index}
        </p>
      ))}
      <button onClick={() => state[self].merge([{ name: "Untitled" }])}>
        add task
      </button>
    </>
  );
}
```

我们对数组使用了`useState`钩子。

为了向数组中添加新值，我们使用`state[self]`对象的`merge`方法向数组中添加一个条目。

然后，我们可以像在`map`回调中一样，通过使用属性的`get`方法来获取条目。

我们还可以添加作用域、异步和递归状态。

我们可以使用@jzone/react-request-hook 来帮助我们简化请求。

我们可以通过运行以下命令来安装它:

```
npm install @jzone/react-request-hook
```

或者:

```
yarn add @jzone/react-request-hook
```

然后我们可以通过写来使用它:

```
import React from "react";
import useFetchData from "@jzone/react-request-hook";
import axios from "axios";export default function App() {
  const [{ data, isLoading, error }, fetchData] = useFetchData(data =>
    axios.get("https://api.agify.io/?name=michael", { params: data })
  ); React.useEffect(() => {
    fetchData();
  }, [fetchData]); if (!data) {
    return !error ? <div>loading...</div> : <div>error</div>;
  } return (
    <div>
      <p>{JSON.stringify(data)}</p>
    </div>
  );
}
```

我们导入了`useFetchData`钩子来导入钩子。

此外，我们安装了 Axios 来将其用作 HTTP 客户端。

然后我们使用钩子返回的`fetchData`函数来获取数据。

`data`有响应数据。

所以我们可以随心所欲地渲染。

它还返回`isLoading`状态以指示加载。

`error`有错误。

我们可以根据其他要求定制。

![](img/670ea066b18617613751dd4240db02cf.png)

内特·切尼在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

Hookstate 是管理状态的一个有用的钩子。

@jzone/react-request-hook 是一个让我们更容易获取数据的钩子。