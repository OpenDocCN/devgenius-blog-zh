# 顶部反应挂钩—辅助挂钩

> 原文：<https://blog.devgenius.io/top-react-hooks-helper-hooks-49e24a8503f4?source=collection_archive---------4----------------------->

![](img/b115d826eb0ef6bdf48cad2d481fa02c.png)

迪米特里·豪特曼在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Hooks 包含了 React 应用程序中的逻辑代码。

我们可以创建自己的钩子，使用别人提供的钩子。

在本文中，我们将看看一些有用的 React 挂钩。

# 反抗海盗

React Pirate 是一个带有各种钩子的库，我们可以使用。

要安装它，我们可以运行:

```
npm install react-pirate
```

或者:

```
yarn add react-pirate
```

它带有各种钩子，包括获取状态的前一个值的`usePrevious`钩子。

然后我们可以通过写来使用它:

```
import React from "react";
import { usePrevious } from "react-pirate";export default function App() {
  const [count, setCount] = React.useState(0);
  const prevCount = usePrevious(count); return (
    <div>
      <button onClick={() => setCount(count => count + 1)}>increment</button>
      <p>
        {count} {prevCount}
      </p>
    </div>
  );
}
```

`usePrevious`钩子让我们获得一个状态的前一个值。

我们只是将`count`钩子传递给`usePrevious`钩子。

它返回`count`的前一个值。

`useToggle` 钩子让我们创建一个布尔状态并切换它。

例如，我们可以写:

```
import React from "react";
import { useToggle } from "react-pirate";export default function App() {
  const { value, toggle } = useToggle(false); return (
    <div>
      <button onClick={toggle}>toggle</button>
      <p>{value.toString()}</p>
    </div>
  );
}
```

使用`useToggle`挂钩。

钩子接受一个布尔值作为参数。

它被用作初始值。

它返回一个具有`value`和`toggle`属性的对象。

`value`具有布尔值。

`toggle`让我们切换`value`。

当组件被挂载时,`useMount`钩子允许我们运行代码。

例如，我们可以写:

```
import React, { useLayoutEffect } from "react";
import { useMount } from "react-pirate";export default function App() {
  useMount(
    () => {
      console.log("mounted");
    },
    { hook: useLayoutEffect }
  ); return <p>hello</p>;
}
```

我们在第一个参数中使用带有回调的`useMount`钩子，该回调在组件被挂载时运行。

它做的事情与`componentDidMount`方法类似。

它还附带了一个相当于`componentDidUpdate`的方法。

例如，我们可以写:

```
import React from "react";
import { useUpdate } from "react-pirate";export default function App() {
  const [count, setCount] = React.useState(0); useUpdate(() => {
    console.log("updated");
  }); return (
    <>
      <button onClick={() => setCount(count => count + 1)}>increment</button>
      <p>{count}</p>
    </>
  );
}
```

我们添加了带有回调的`useUpdate`钩子，当`count`状态被更新时，回调就会运行。

它还附带了一个`useUnmount`钩子，该钩子接受一个回调来运行一个组件卸载。

我们可以这样写:

```
import React from "react";
import { useUnmount } from "react-pirate";export default function App() {
  useUnmount(() => {
    console.log("unmount");
  }); return <></>;
}
```

# 反应动力挂钩

react-powerhooks 库附带了各种可以在我们的应用程序中使用的钩子。

要安装它，我们运行:

```
yarn add react-powerhooks
```

然后我们可以使用它附带的钩子。

例如，我们可以通过编写以下代码来使用`useToggle`钩子:

```
import React from "react";
import { useToggle } from "react-powerhooks";export default function App() {
  const [value, toggle] = useToggle(true); return (
    <>
      <button onClick={() => toggle()}>toggle</button>
      <p>{value.toString()}</p>
    </>
  );
}
```

我们在组件中使用了`useToggle`钩子。

它需要一个参数作为初始值。

它返回一个数组，其值作为`value`变量。

`toggle`是切换`value`的功能。

它有更多的钩子来帮助我们。

![](img/680b4ee21442b0b599a6099262b98dfe.png)

由 [Ravi Roshan](https://unsplash.com/@ravi_roshan_inc?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

React Pirate 和 react-powerhooks 都为我们提供了很多辅助钩子。