# 顶部反应挂钩—生命周期和切换

> 原文：<https://blog.devgenius.io/top-react-hooks-lifecycle-and-toggles-7dd553998ae1?source=collection_archive---------6----------------------->

![](img/495e30bb9eab6ac82a5479faade2f633.png)

安德鲁·庞斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Hooks 包含了 React 应用程序中的逻辑代码。

我们可以创建自己的钩子，使用别人提供的钩子。

在本文中，我们将看看一些有用的 React 挂钩。

# 反应钩子库

React Hooks Lib 是一个库，有许多可重用的 React Hooks。

要安装它，我们可以运行:

```
npm i react-hooks-lib --save
```

然后我们可以使用库附带的钩子。

然后我们可以使用`useDidUpdate`钩子。

这类似于 React 类组件中的`componentDidUpdate`方法。

它只在更新时运行。

例如，我们可以这样使用它:

```
import React from "react";
import { useDidUpdate, useCounter } from "react-hooks-lib";export default function App() {
  const { count, inc } = useCounter(0);
  useDidUpdate(() => {
    console.log("update");
  });
  return (
    <div>
      {`count: ${count}`}
      <button onClick={() => inc(1)}>increment</button>
    </div>
  );
}
```

我们使用`useCounter`钩子来创建一个计数器状态。

我们使用`inc`函数来增加它。

而`count`有这个州。

`useDidUpdate`的回调在`count`更新时运行。

`useMergeState`钩子让我们创建一个状态设置函数，它的工作方式类似于 React 类组件的`setState`方法。

例如，我们可以这样使用它:

```
import React from "react";
import { useMergeState } from "react-hooks-lib";export default function App() {
  const { state, set } = useMergeState({ name: "james", age: 1 }); return (
    <div>
      <button onClick={() => set(({ age }) => ({ age: age + 1 }))}>
        increment age
      </button>
      {JSON.stringify(state)}
    </div>
  );
}
```

`useMergeState`以初始状态为自变量。

它返回带有状态值的`state`。

`set`让我们通过传入一个对象来设置凝视。

该对象将与现有的状态值合并。

我们向它传递一个函数，该函数将当前状态对象作为参数，我们返回新的状态对象作为值。

`useCounter`钩子让我们创建一个可以更新的数字状态。

例如，我们可以写:

```
import React from "react";
import { useCounter } from "react-hooks-lib";export default function App() {
  const { count, inc, dec, reset } = useCounter(1); return (
    <div>
      {count}
      <button onClick={() => inc(1)}>increment</button>
      <button onClick={() => dec(1)}>decrement</button>
      <button onClick={reset}>reset</button>
    </div>
  );
}
```

用`useCounter`钩子创建一个带有数字状态`count`的组件。

自变量是`count`的初始值。

`inc`增量`count`。

`dec`增量`count`。

`reset`将`count`重置为初始值。

`useToggle`钩子创建一个布尔状态，让我们在`true`或`false`之间切换。

为了使用它，我们写:

```
import React from "react";
import { useToggle } from "react-hooks-lib";export default function App() {
  const { on, toggle, reset } = useToggle(false); return (
    <div>
      <button onClick={toggle}>toggle</button>
      <button onClick={reset}>reset</button>
      <p>{String(on)}</p>
    </div>
  );
}
```

我们使用带有初始值的`useToggle`钩子作为自变量。

`on`属性具有切换状态的值。

`toggle`让我们切换`on`的值。

并且`reset`将`on`重置为初始值。

![](img/9b27791d822d816a641abbeee6a76520.png)

照片由 [Yomex Owo](https://unsplash.com/@yomex4life?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

React Hooks Lib 自带钩子，模仿类组件的生命周期方法。

它还附带了钩子，让我们可以更容易地操纵各种状态。