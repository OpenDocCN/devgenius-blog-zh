# 成帧器运动-拖动动画

> 原文：<https://blog.devgenius.io/framer-motion-drag-animation-a3b0749ea5b4?source=collection_archive---------0----------------------->

![](img/d598765794a2a8fa50ac0f8d5707bc16.png)

照片由[安格尔·坎普](https://unsplash.com/@angelekamp?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

有了成帧器运动库，我们可以轻松地在 React 应用程序中渲染动画。

在这篇文章中，我们将看看如何开始与帧运动。

# 拖动和布局动画

当我们拖动它时，我们可以使我们的元素有动画效果。

例如，我们可以写:

```
import { motion } from "framer-motion";
import React from "react";export default function App() {
  return (
    <motion.div
      style={{ backgroundColor: "red", width: 100, height: 100 }}
      drag="x"
      dragConstraints={{ left: 0, right: 300 }}
    />
  );
}
```

添加一个 div 并使其可拖动。

我们通过将`drag`属性设置为`'x'`让我们水平拖动来使它可拖动。

`dragConstraints`道具让我们设定拖动的限制。

此外，我们可以将拖动限制设置为另一个元素的边界。

例如，我们可以写:

```
import { motion } from "framer-motion";
import React, { useRef } from "react";export default function App() {
  const constraintsRef = useRef(null); return (
    <motion.div
      ref={constraintsRef}
      style={{ backgroundColor: "green", width: 200, height: 200 }}
    >
      <motion.div
        style={{ backgroundColor: "red", width: 100, height: 100 }}
        drag
        dragConstraints={constraintsRef}
      />
    </motion.div>
  );
}
```

将拖动限制设置为外部 div 的边界。

我们还可以设置允许超出约束的移动程度。

例如，我们可以写:

```
import { motion } from "framer-motion";
import React from "react";export default function App() {
  return (
    <motion.div
      drag
      dragConstraints={{ left: 0, right: 300 }}
      dragElastic={0.2}
      style={{ backgroundColor: "red", width: 100, height: 100 }}
    />
  );
}
```

我们设置`dragElastic`道具来设置阻力约束之外允许的移动量。

`dragMomentum`道具让我们在拖动结束时应用组件平移姿态的动量。

默认是`true`。

我们可以通过书写来使用它:

```
import { motion } from "framer-motion";
import React from "react";export default function App() {
  return (
    <motion.div
      drag
      dragConstraints={{ left: 0, right: 300 }}
      dragMomentum={false}
      style={{ backgroundColor: "red", width: 100, height: 100 }}
    />
  );
}
```

拖动完成后移除多余的移动。

属性让我们允许拖拽手势传播到子组件。

例如，我们可以写:

```
import { motion } from "framer-motion";
import React from "react";export default function App() {
  return (
    <motion.div
      drag="x"
      style={{ backgroundColor: "red", width: 100, height: 100 }}
    >
      <motion.div
        drag="x"
        dragConstraints={{ left: 0, right: 300 }}
        dragPropagation
        style={{
          backgroundColor: "green",
          width: 50,
          height: 50
        }}
      ></motion.div>
    </motion.div>
  );
}
```

然后当我们拖动子 div 时，父 div 也会移动。

我们可以用`useDragControls`钩子控制拖拽。

例如，我们可以写:

```
import { motion, useDragControls } from "framer-motion";
import React from "react";export default function App() {
  const dragControls = useDragControls(); function startDrag(event) {
    dragControls.start(event, { snapToCursor: true });
  } return (
    <>
      <div onPointerDown={startDrag}>click to drag</div>
      <motion.div
        style={{ backgroundColor: "red", width: 100, height: 100 }}
        drag="x"
        dragControls={dragControls}
      />
    </>
  );
}
```

我们将`onPointerDown`属性设置为`starDrag`函数。

`startDrag`调用`dragControls.start`方法移动红色 div。

`snapToCursor`设置为`true`意味着 div 将移动以对齐光标所在的位置。

因此，当我们单击`'click to drag'`文本时，div 将向光标移动。

# 结论

当我们用帧运动拖动一个元素时，我们可以制作动画。