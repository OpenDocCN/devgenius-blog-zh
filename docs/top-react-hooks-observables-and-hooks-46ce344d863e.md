# 顶部反应挂钩—可观察和滚动

> 原文：<https://blog.devgenius.io/top-react-hooks-observables-and-hooks-46ce344d863e?source=collection_archive---------3----------------------->

![](img/0ba78c45f35f1f78b24090c9a7dec1c9.png)

[钱杰夫](https://unsplash.com/@junfungchin?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Hooks 包含了 React 应用程序中的逻辑代码。

我们可以创建自己的钩子，使用别人提供的钩子。

在本文中，我们将看看一些有用的 React 挂钩。

# RxJS 的反应挂钩

Rxjs 库的 React 挂钩让我们可以在 React 组件中使用 RxJS 可观察对象。

要安装它，我们可以运行:

```
npm i --save rxjs-hooks
```

或者:

```
yarn add rxjs-hooks
```

我们可以通过书写来使用它:

```
import React from "react";
import { useObservable } from "rxjs-hooks";
import { interval } from "rxjs";
import { map } from "rxjs/operators";export default function App() {
  const value = useObservable(() => interval(2000).pipe(map(val => val * 2))); return (
    <div className="App">
      <h1>{value}</h1>
    </div>
  );
}
```

我们用`useObservable`钩子来观察`interval`。

它返回可观察值。

`pipe`操作符让我们修改从`interval`可观察值生成的值。

它还带有`useEventCallback`钩子，让我们观察事件对象的变化。

例如，我们可以写:

```
import React from "react";
import { useEventCallback } from "rxjs-hooks";
import { map } from "rxjs/operators";export default function App() {
  const [clickCallback, [description, x, y]] = useEventCallback(
    event$ =>
      event$.pipe(
        map(event => [event.target.innerHTML, event.clientX, event.clientY])
      ),
    ["nothing", 0, 0]
  ); return (
    <div className="App">
      <p>
        click position: {x}, {y}
      </p>
      <p>{description} was clicked.</p>
      <button onClick={clickCallback}>click foo</button>
      <button onClick={clickCallback}>click bar</button>
      <button onClick={clickCallback}>click baz</button>
    </div>
  );
}
```

我们用一个回调函数调用了`useEventCallback`钩子，这个回调函数可以观察到 th `event$`。

它返回从`pipe`操作符返回的管道结果。

`map`让我们返回一个对象，我们可以从中获取点击事件对象属性。

钩子返回一个带有`clickCallback`的数组，我们可以将它用作点击处理程序。

第二个条目是 click 事件对象。

第三个参数是一个数组，包含 click 事件对象中属性的初始值。

# 滚动数据挂钩

滚动数据钩子库让我们返回关于滚动速度、距离、方向等信息。

要使用它，我们可以通过运行以下命令来安装它:

```
yarn add scroll-data-hook
```

然后我们可以通过写来使用它:

```
import React from "react";
import { useScrollData } from "scroll-data-hook";export default function App() {
  const {
    scrolling,
    time,
    speed,
    direction,
    position,
    relativeDistance,
    totalDistance
  } = useScrollData({
    onScrollStart: () => {
      console.log("Started scrolling");
    },
    onScrollEnd: () => {
      console.log("Finished scrolling");
    }
  }); return (
    <div>
      <div style={{ position: "fixed" }}>
        <p>{scrolling ? "Scrolling" : "Not scrolling"}</p>
        <p>Scrolling time: {time} milliseconds</p>
        <p>Horizontal speed: {speed.x} pixels per second</p>
        <p>Vertical speed: {speed.y} pixels per second</p>
        <p>
          Position: {position.x} {position.y}
        </p>
        <p>
          Direction: {direction.x} {direction.y}
        </p>
        <p>
          Relative distance: {relativeDistance.x}/{relativeDistance.y}
        </p>
        <p>
          Total distance: {totalDistance.x}/{totalDistance.y}
        </p>
      </div>
      <div>
        {Array(1000)
          .fill()
          .map((_, i) => (
            <p key={i}>{i}</p>
          ))}
      </div>
    </div>
  );
}
```

我们用对象调用`useScrollData`钩子，该对象有一个在我们滚动时运行的回调函数。

`onScrollStart`开始滚动时运行。

`onScrollEnd`停止滚动时运行。

钩子返回一个具有各种属性的对象。

`scrolling`是一个布尔值，表示我们是否在滚动。

`time`是我们滚动的时间。

`speed`是滚动速度。

`direction`是滚动方向。

`position`是滚动位置。

![](img/2943f0b5350e0c0fe5cb001dc9ea700d.png)

安德斯·维德斯科特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

React Hooks for RxJS 允许我们在 React 应用程序中使用 Rxjs observables。

滚动数据钩让我们在应用程序中观察不同类型的滚动数据。