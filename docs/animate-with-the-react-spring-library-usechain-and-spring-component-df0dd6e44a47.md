# 使用 react-spring 库制作动画——使用链和弹簧组件

> 原文：<https://blog.devgenius.io/animate-with-the-react-spring-library-usechain-and-spring-component-df0dd6e44a47?source=collection_archive---------3----------------------->

![](img/32dbeec22309ce78ffab624d64bfaa91.png)

由[斯拉夫·斯图帕琴科](https://unsplash.com/@mrstupachenko?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

有了 react-spring 库，我们可以轻松地在 react 应用程序中渲染动画。

在本文中，我们将了解如何开始使用 react-spring。

# 使用链

钩子让我们一个接一个地渲染过渡。

例如，我们可以写:

```
import React, { useRef, useState } from "react";
import {
  useTransition,
  animated,
  useChain,
  useSpring,
  useTrail
} from "react-spring";export default function App() {
  const [on, toggle] = useState(false); const springRef = useRef();
  const spring = useSpring({
    ref: springRef,
    from: { opacity: 0.5 },
    to: { opacity: on ? 1 : 0.5 },
    config: { tension: 250 }
  }); const trailRef = useRef();
  const trail = useTrail(5, {
    ref: trailRef,
    from: { fontSize: "10px" },
    to: { fontSize: on ? "35px" : "10px" }
  }); useChain(on ? [springRef, trailRef] : [trailRef, springRef]); return (
    <div>
      {trail.map((animation, index) => (
        <animated.div style={{ ...animation, ...spring }} key={index}>
          Hello World
        </animated.div>
      ))} <button onClick={() => toggle(!on)}>toggle</button>
    </div>
  );
}
```

动画切换“Hello World”文本的大小。

我们用`toggle`函数创建`on`状态来切换`on`状态。

然后我们用`useRef`钩子创建一个 ref，用`useSpring`钩子创建一个过渡。

我们将`ref`属性设置为`springRef`，这样我们就可以在`useChain`钩子中使用它。

同样，我们使用`useTrail`来创建第二个过渡。

然后我们使用 th `useChain`钩子来创建两个结合在一起的动画。

它们的顺序取决于`on`状态的值。

然后我们通过调用`trail.map`方法来渲染 Hello World 文本，为动画的每个阶段应用样式。

现在，当我们单击“切换”时，我们会看到 Hello World 文本大小发生变化。

# 渲染道具 API

react-spring 库附带了一个渲染道具 API。

为了使用它来创建动画，我们使用了`Spring`组件。

例如，我们可以写:

```
import React from "react";
import { Spring } from "react-spring/renderprops";export default function App() {
  return (
    <div>
      <Spring from={{ opacity: 0 }} to={{ opacity: 1 }}>
        {(props) => <div style={props}>hello</div>}
      </Spring>
    </div>
  );
}
```

添加`Spring`组件。

我们设置`from`道具来添加在动画开始时应用的样式。

我们还添加了`to`道具来添加要在动画结尾应用的样式。

在 render prop 中，我们传递一个函数，其中包含我们想要呈现的内容。

`props`参数具有在动画的每个阶段应用的样式，所以我们将其传递给`style`道具来应用它们。

此外，我们可以通过写下以下内容来激活数字:

```
import React from "react";
import { Spring } from "react-spring/renderprops";export default function App() {
  return (
    <div>
      <Spring from={{ number: 0 }} to={{ number: 1 }}>
        {(props) => <div>{props.number}</div>}
      </Spring>
    </div>
  );
}
```

我们用`from`和`to`道具分别设置`number`在动画的开头和结尾显示。

然后我们将数字从 0 到 1 动画化。

# 结论

我们可以使用`useChain`钩子来链接动画。

我们可以使用`Spring`组件来渲染动画。