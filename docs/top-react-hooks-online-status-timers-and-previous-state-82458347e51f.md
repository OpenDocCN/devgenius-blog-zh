# Top React 挂钩—在线状态、计时器和以前的状态

> 原文：<https://blog.devgenius.io/top-react-hooks-online-status-timers-and-previous-state-82458347e51f?source=collection_archive---------6----------------------->

![](img/5a8dbfecf58113b3c9cd3d905291dfe2.png)

[百合银行](https://unsplash.com/@lvnatikk?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Hooks 包含了 React 应用程序中的逻辑代码。

我们可以创建自己的钩子，使用别人提供的钩子。

在本文中，我们将看看一些有用的 React 挂钩。

# 反应冷淡

React hookedUp 是一个有很多钩子的库，让我们的生活更轻松。

要安装它，我们运行:

```
npm install react-hookedup --save
```

或者:

```
yarn add react-hookedup
```

`useMergeState`让我们像在 React 类组件中使用`setState`一样设置状态。

例如，我们可以这样使用它:

```
import React from "react";
import { useMergeState } from "react-hookedup";export default function App() {
  const { state, setState } = useMergeState({ loading: false }); return (
    <div>
      <button onClick={() => setState({ loading: !state.loading })}>
        toggle
      </button>
      <p>{state.loading.toString()}</p>
    </div>
  );
}
```

我们有`useMergeState`来让我们轻松管理对象状态。

它返回一个具有`state`和`setState`属性的对象。

`state`有当前状态值。

`setState`是设置状态的功能。

我们通过传入一个具有属性和值的对象来使用它。

`usePrevious`钩子让我们从`useState`钩子中获得先前的值。

例如，我们可以写:

```
import React from "react";
import { usePrevious } from "react-hookedup";export default function App() {
  const [count, setCount] = React.useState(0);
  const prevCount = usePrevious(count);
  return (
    <>
      <button onClick={() => setCount(count => count + 1)}>increment</button>
      <p>
        Now: {count}, before: {prevCount}
      </p>
    </>
  );
}
```

我们通过将状态值传入来使用`usePrevious`钩子。

然后我们就可以用钩子返回的值得到之前的值。

`useInterval`钩子让我们以 React 方式运行`setInterval`。

例如，我们可以写:

```
import React from "react";
import { useInterval } from "react-hookedup";export default function App() {
  const [count, setCount] = React.useState(0);
  useInterval(() => setCount(count => count + 1), 1000); return <h1>{count}</h1>;
}
```

我们使用了带有回调的`useInterval`钩子，正如我们在第二个参数中指定的那样，回调每秒被调用一次。

数字以毫秒为单位。

然后`count`每秒更新一次。

`useTimeout`钩子让我们用一个方便的钩子运行`setTimeout`。

例如，我们可以写:

```
import React from "react";
import { useTimeout } from "react-hookedup";export default function App() {
  useTimeout(() => alert("hello world"), 1500); return (
    <>
      <p>hello</p>
    </>
  );
}
```

`useTimeout`钩子接受回调，该回调在第二个参数给定的时间后运行。

数字以毫秒为单位。

`useOnlineStatus`钩子让我们获得当前的在线状态。

例如，我们可以这样使用它:

```
import React from "react";
import { useOnlineStatus } from "react-hookedup";export default function App() {
  const { online } = useOnlineStatus(); return <h1>{online ? "online" : "offline"}</h1>;
}
```

我们只需调用钩子，它就会返回一个带有`online`属性的对象。

它表明我们的应用程序是否在线。

![](img/4073b06d5d8e40d75c33047a36ffa75a.png)

布鲁克·拉克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

# 结论

React hookedUp 有许多有用的钩子。

我们可以设置以前的状态，使用计时器，并通过提供的钩子获得在线状态。