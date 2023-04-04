# 使用反应弹簧库制作动画——使用弹簧挂钩

> 原文：<https://blog.devgenius.io/animate-with-the-react-spring-library-usespring-hook-a1987d7567db?source=collection_archive---------2----------------------->

![](img/5af646d1ee50f461e0b0ae345c194103.png)

阿诺·斯密特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

有了 react-spring 库，我们可以轻松地在 react 应用程序中渲染动画。

在本文中，我们将了解如何开始使用 react-spring。

# 使用弹簧

我们可以使用`useSpring`钩子来添加我们的动画。

例如，我们可以写:

```
import React, { useEffect, useState } from "react";
import { useSpring, animated } from "react-spring";export default function App() {
  const [props, set, stop] = useSpring(() => ({ opacity: 1 }));
  const [toggle, setToggle] = useState(); useEffect(() => {
    set({ opacity: toggle ? 1 : 0 });
  }, [toggle]); return (
    <>
      <button onClick={() => setToggle(!toggle)}>start</button>
      <button onClick={() => stop()}>stop</button>
      <animated.div style={props}>i will fade</animated.div>
    </>
  );
}
```

我们将回调函数传入`useSpring`钩子来创建我们的动画。

我们还有`toggle`状态，让我们用`set`功能切换不透明度。

因此，当我们点击开始，我们会看到文本切换褪色。

我们有`stop`功能来停止动画。

我们也可以通过传入一个对象来使用`useSpring`。

例如，我们可以写:

```
import React from "react";
import { useSpring, animated } from "react-spring";export default function App() {
  const props = useSpring({
    from: { opacity: 0 },
    to: { opacity: 1, color: "red" }
  }); return (
    <>
      <animated.div style={props}>i will fade</animated.div>
    </>
  );
}
```

我们从带有`from`属性的样式到带有`to`属性的样式制作动画。

我们可以将其简化为:

```
import React from "react";
import { useSpring, animated } from "react-spring";export default function App() {
  const props = useSpring({
    from: { opacity: 0 },
    opacity: 1,
    color: "red"
  }); return (
    <>
      <animated.div style={props}>i will fade</animated.div>
    </>
  );
}
```

# 异步链和脚本

我们可以用`next`函数异步制作动画。

例如，我们可以写:

```
import React from "react";
import { useSpring, animated } from "react-spring";export default function App() {
  const props = useSpring({
    to: async (next, cancel) => {
      await next({ opacity: 1, color: "#ffaaee" });
      await next({ opacity: 0, color: "rgb(14,26,19)" });
    },
    from: { opacity: 0, color: "red" }
  }); return (
    <>
      <animated.div style={props}>i will fade</animated.div>
    </>
  );
}
```

`to`方法是一个异步函数，将`next`和`cancel`函数作为参数。

`next`让我们设置想要在动画中应用的样式。

并且`cancel`让我们取消动画。

`from`仍然是一个具有 div 初始样式的对象。

此外，我们可以通过编写以下代码来创建带有数组的`to`属性:

```
import React from "react";
import { useSpring, animated } from "react-spring";export default function App() {
  const props = useSpring({
    to: [
      { opacity: 1, color: "#ffaaee" },
      { opacity: 0, color: "rgb(14,26,19)" }
    ],
    from: { opacity: 0, color: "red" }
  }); return (
    <>
      <animated.div style={props}>i will fade</animated.div>
    </>
  );
}
```

该数组包含具有我们要应用的样式的对象。

# 结论

`useSpring`钩子让我们可以很容易地为单个元素创建动画。