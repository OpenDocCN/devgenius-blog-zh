# 顶部反应挂钩—悬停、全屏、事件和地理定位

> 原文：<https://blog.devgenius.io/top-react-hooks-hover-full-screen-events-and-geolocation-f1b3706b6b6e?source=collection_archive---------9----------------------->

![](img/73c24aa3916f88407fd64bbcb497db04.png)

约翰·西门子在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Hooks 包含了 React 应用程序中的逻辑代码。

我们可以创建自己的钩子，使用别人提供的钩子。

在本文中，我们将看看一些有用的 React 挂钩。

# 反应配方

React Recipes 附带了许多钩子，我们可以用来做各种事情。

挂钩让我们可以在 React 组件中轻松地监听各种事件。

要使用它，我们可以写:

```
import React, { useState, useCallback } from "react";
import { useEventListener } from "react-recipes";export default function App() {
  const [coords, setCoords] = useState({ x: 0, y: 0 }); const handler = useCallback(
    ({ clientX, clientY }) => {
      setCoords({ x: clientX, y: clientY });
    },
    [setCoords]
  ); useEventListener("mousemove", handler); return (
    <p>
      The mouse position is ({coords.x}, {coords.y})
    </p>
  );
}
```

我们使用`useState`钩子来设置初始鼠标坐标。

然后我们给用作`mousemove`事件监听器的`useCallback`钩子添加了一个回调。

然后我们将这个处理程序传递给`useEventListener`钩子。

`useEventListener`获取元素要监听的可选第三个参数。

默认值为`window`。

然后我们从`coords`状态得到鼠标坐标。

`useFullScreen`是一个让我们决定和切换屏幕或元素是否处于全屏模式的好工具。

我们可以通过书写来使用它:

```
import React from "react";
import { useFullScreen } from "react-recipes";export default function App() {
  const { fullScreen, toggle } = useFullScreen(); return (
    <>
      <button onClick={toggle}>Toggle Full Screen</button>
      <p>{fullScreen.toString()}</p>
    </>
  );
}
```

我们使用`useFullScreen`钩子返回一个带有`toggle`函数的对象。

它让我们切换全屏模式。

它还返回`fullScreen`属性来指示文档是否全屏显示。

`open`让我们打开全屏模式。

`close`让我们关闭全屏模式。

钩子让我们获得并观察用户的地理位置。

要使用它，我们可以写:

```
import React from "react";
import { useGeolocation } from "react-recipes";export default function App() {
  const { latitude, longitude, timestamp, accuracy, error } = useGeolocation(
    true
  ); return (
    <p>
      latitude: {latitude}
      longitude: {longitude}
      timestamp: {timestamp}
      accuracy: {accuracy && `${accuracy}m`}
      error: {error}
    </p>
  );
}
```

我们使用带有参数`true`的`useGeolocation`来表示我们想要跟踪位置。

它还需要第二个参数和一些选项。

我们可以启用高准确性、设置超时以及设置最大缓存时间。

它返回一个带有纬度、经度、时间戳、精确度和误差的对象。

`useHover`钩子让我们知道什么时候 most 停留在一个元素上。

要使用它，我们可以写:

```
import React from "react";
import { useHover } from "react-recipes";export default function App() {
  const [hoverRef, isHovered] = useHover(); return <div ref={hoverRef}>{isHovered.toString()}</div>;
}
```

我们在组件中调用了`useHover`钩子，它返回 2 样东西。

`hoverRef`是一个 ref，我们将它传递给想要观察的组件。

`isHovered`表示我们是否悬停在我们正在观察的元素上。

![](img/043ad7a620595d90d1b22063479c7068.png)

照片由[埃里佐·迪亚斯](https://unsplash.com/@elishavision?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 结论

React Recipes 附带了帮助我们获取地理位置数据、观察悬停、切换全屏等功能的钩子。