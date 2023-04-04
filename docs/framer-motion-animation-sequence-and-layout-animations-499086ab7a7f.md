# 帧运动-动画序列和布局动画

> 原文：<https://blog.devgenius.io/framer-motion-animation-sequence-and-layout-animations-499086ab7a7f?source=collection_archive---------1----------------------->

![](img/12574c4e4566a42f063f37d8e079f3a7.png)

JOSHUA COLEMAN 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

有了成帧器运动库，我们可以轻松地在 React 应用程序中渲染动画。

在这篇文章中，我们将看看如何开始与帧运动。

# 定序

`controls.start`方法返回一个承诺，因此我们可以用它来对动画进行排序。

例如，我们可以写:

```
import React, { useEffect } from "react";
import { motion, useAnimation } from "framer-motion";export default function App() {
  const controls = useAnimation(); const sequence = async () => {
    await controls.start({ x: 0 });
    return await controls.start({ opacity: 1 });
  }; useEffect(() => {
    sequence();
  }, []); return (
    <motion.div
      animate={controls}
      style={{ backgroundColor: "red", width: 100, height: 100 }}
    />
  );
}
```

我们有调用不同风格的`controls.start`的`sequence`函数。

这就是我们在加载组件时看到的。

# 动态 S `tart`

我们可以动态地调用`controls.start`。

例如，我们可以写:

```
import React, { useEffect } from "react";
import { motion, useAnimation } from "framer-motion";export default function App() {
  const controls = useAnimation(); useEffect(() => {
    controls.start((i) => ({
      opacity: 0,
      x: 100,
      transition: { delay: i * 0.3 }
    }));
  }, []); return (
    <ul>
      <motion.li custom={0} animate={controls}>
        foo
      </motion.li>
      <motion.li custom={1} animate={controls}>
        bar
      </motion.li>
      <motion.li custom={2} animate={controls}>
        baz
      </motion.li>
    </ul>
  );
}
```

我们用参数`i`传入一个回调，这是我们传入属性`custom`的值。

我们传递给`animate`道具来设置`controls`来控制动画。

# 布局动画

我们可以用帧运动来激活布局

例如，我们可以写:

`App.js`

```
import React, { useState } from "react";
import { motion } from "framer-motion";
import "./styles.css";const spring = {
  type: "spring",
  stiffness: 700,
  damping: 30
};export default function App() {
  const [isOn, setIsOn] = useState(false); const toggleSwitch = () => setIsOn(!isOn); return (
    <div className="switch" data-isOn={isOn} onClick={toggleSwitch}>
      <motion.div className="handle" layout transition={spring} />
    </div>
  );
}
```

`styles.css`

```
html,
body {
  min-height: 100vh;
  padding: 0;
  margin: 0;
}* {
  box-sizing: border-box;
}.App {
  font-family: sans-serif;
  text-align: center;
}.switch {
  width: 160px;
  height: 100px;
  background-color: green;
  display: flex;
  justify-content: flex-start;
  border-radius: 50px;
  padding: 10px;
  cursor: pointer;
}.switch[data-isOn="true"] {
  justify-content: flex-end;
}.handle {
  width: 80px;
  height: 80px;
  background-color: white;
  border-radius: 40px;
}
```

我们添加了`spring`动画效果，并将其传递到`transition`道具中。

`onClick`支柱具有控制`isOn`状态的功能。

道具让我们制作 div 布局的动画。

我们添加样式来将 div 样式化为一个开关。

`justify-content`设置为`flex-start`开关底部在左侧。`flex-end`将开关按钮置于右侧。

# 结论

我们可以用帧运动来控制我们的动画进度。

我们可以将动画应用于布局。