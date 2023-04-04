# 顶部反应挂钩—共享状态和可见性

> 原文：<https://blog.devgenius.io/top-react-hooks-shared-state-and-visibility-85601f0421cd?source=collection_archive---------7----------------------->

![](img/d96e2b1482121bd72a2bc9ac98829e04.png)

[伊莱恩·卡萨普](https://unsplash.com/@ecasap?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Hooks 包含了 React 应用程序中的逻辑代码。

我们可以创建自己的钩子，使用别人提供的钩子。

在本文中，我们将看看一些有用的 React 挂钩。

# @ rooks/use-visibility-传感器

@rooks/use-visibility-sensor 包让我们检测组件的可见性。

要安装它，我们可以运行:

```
npm install --save @rooks/use-visibility-sensor
```

然后我们可以通过写来使用它:

```
import React from "react";
import useVisibilitySensor from "@rooks/use-visibility-sensor";export default function App() {
  const ref = React.useRef(null);
  const { isVisible, visibilityRect } = useVisibilitySensor(ref, {
    intervalCheck: false,
    scrollCheck: true,
    resizeCheck: true
  });
  return (
    <div ref={ref}>
      <p>{isVisible ? "Visible" : "Not Visible"}</p>
      <p>{JSON.stringify(visibilityRect)}</p>
    </div>
  );
}
```

我们使用`useVisibilitySensor`和一个传递给我们想要检查可见性的元素的 ref。

第二个参数是一个具有各种选项的对象。

`intervalCheck`是一个整数或布尔值，它告诉我们是否定期检查可见性。

如果它是一个数字，那么它就是每次检查之间的时间间隔，以毫秒为单位。

`scrollCheck`是一个决定是否检查滚动行为的布尔值。

`resizeCheck`如果是`true`，让我们检查调整大小。

还有其他节流选项等等。

# 重新同步

重新同步库是一个允许我们在多个组件之间共享状态的库。

要安装它，我们可以运行:

```
yarn add resynced
```

然后我们可以通过写来使用它:

```
import React from "react";
import { createSynced } from "resynced";const initialState = 0;
const [useSyncedState] = createSynced(initialState);const Counter = () => {
  const [count] = useSyncedState(); return (
    <div>
      <p> {count}</p>
    </div>
  );
};export default function App() {
  const [_, setCount] = useSyncedState(); return (
    <div>
      <button onClick={() => setCount(s => s + 1)}>increment</button>
      <Counter />
    </div>
  );
}
```

我们用 th 初始状态调用`createSynced`，用`useSyncedState`钩子返回一个数组。

然后我们在组件中使用返回的钩子。

它返回一个带有状态的数组和按此顺序设置状态的函数。

我们调用`setCount`来设置`App`中的状态

我们在`Counter`中得到`count`状态。

# RRH

RRH 提供了钩子，我们可以用它来获取和设置 React Redux 状态数据。

要安装它，我们可以运行:

```
npm install rrh --save
```

或者

```
yarn add rrh
```

然后我们可以通过连线来使用它:

```
import React from "react";
import { useProvider, useSelector } from "rrh";
import { createLogger } from "redux-logger";
import { applyMiddleware } from "redux";const logger = createLogger();const initState = {
  count: 0
};const reducer = (state, action) => {
  switch (action.type) {
    case "INCREMENT":
      return { count: state.count + 1 };
    default:
      return state;
  }
};const Counter = () => {
  const selected = useSelector(
    {},
    (dispatch, props) => ({
      increment: () => dispatch({ type: "INCREMENT", payload: 1 })
    }),
    (state, props) => ({ count: state.count }),
    state => [state.count]
  ); return (
    <div>
      {selected.count}
      <button onClick={selected.increment}>INC</button>
    </div>
  );
};export default function App() {
  const Provider = useProvider(() => ({
    reducer,
    preloadedState: { count: 1 },
    storeEnhancer: applyMiddleware(logger)
  }));
  return (
    <div>
      <Provider>
        <Counter />
        <Counter />
      </Provider>
    </div>
  );
}
```

我们用`initState`对象创建一个具有初始状态的对象。

我们创建了`createLogger`函数来返回`logger`。

然后我们创建我们的 reducer 来设置状态。

在`Counter`中，我们称之为`useSelector`钩子。

第一个论点是道具。

第二个参数有一个函数，用于返回一个对象，该对象带有调度操作的函数。

第三个参数按照我们想要的方式映射状态。

第四个参数有一个函数，需要注意它的依赖关系。

状态在`selected`变量中。

在`App`中，我们用`Provider`包装我们希望商店可以访问的组件。

`Provider`是用接受一个函数的`useProvider`钩子创建的。

该函数返回一个带有`reducer`、`preloadedState`和`storeEnhancer`的对象。

`reducer`有减速器。

`preloadedState`有初始状态。

`storeEnhancer`有我们要运行的 Redux 中间件。

![](img/86811539388fdd4d64cca6d7704783ab.png)

照片由[凯尔·格伦](https://unsplash.com/@kylejglenn?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

@ rooks/use-visibility-sensor 让我们检测能见度。

重新同步使我们能够为组件创建共享状态。

RRH 让我们用钩子来代替 Redux。