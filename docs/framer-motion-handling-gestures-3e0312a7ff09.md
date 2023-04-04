# 成帧器动作-处理手势

> 原文：<https://blog.devgenius.io/framer-motion-handling-gestures-3e0312a7ff09?source=collection_archive---------3----------------------->

![](img/777a17003c2fed99a126fc3e7906f91f.png)

杰西卡·鲁斯切洛在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

有了成帧器运动库，我们可以轻松地在 React 应用程序中渲染动画。

在这篇文章中，我们将看看如何开始与帧运动。

# 手势

成帧器动作能够识别手势。

例如，我们可以写:

```
import { motion } from "framer-motion";
import React from "react";export default function App() {
  return (
    <motion.button
      whileHover={{
        scale: 1.2,
        transition: { duration: 1 }
      }}
      whileTap={{ scale: 0.9 }}
    >
      hello world
    </motion.button>
  );
}
```

当我们悬停或点击时改变按钮的大小。

`scale`改变尺寸。`transition.duration`设置效果的持续时间。

我们也可以设置`variants`道具来设置按钮的效果。

例如，我们可以写:

```
import { motion } from "framer-motion";
import React from "react";const variants = {
  buttonVariants: { opacity: 1 },
  iconVariants: { opacity: 0.5 }
};export default function App() {
  return (
    <motion.button whileTap="tap" whileHover="hover" variants={variants}>
      <svg>
        <motion.path variants={variants} />
      </svg>
    </motion.button>
  );
}
```

我们设置`whileTap`和`whileHover`来设置这些手势的处理程序。

并且我们设置`variants`道具来设置我们想要看到的效果。

# 盘旋

当我们用`whileHover`道具悬停在一个元素上时，我们可以添加动画。

例如，我们可以写:

```
import { motion } from "framer-motion";
import React from "react";export default function App() {
  return (
    <motion.div
      whileHover={{ scale: 1.2 }}
      onHoverStart={(e) => {
        console.log(e);
      }}
      onHoverEnd={(e) => {
        console.log(e);
      }}
    >
      hello
    </motion.div>
  );
}
```

然后我们可以用`onHoverStart`和`onHoverEnd`道具来听悬停开始和悬停结束事件。

`e`我们可以得到关于悬停的对象信息。

# 轻敲，水龙头

我们还可以用`whileTap`道具处理点击事件的动画。

例如，我们可以写:

```
import { motion } from "framer-motion";
import React from "react";export default function App() {
  return <motion.div whileTap={{ scale: 0.8 }}>hello</motion.div>;
}
```

我们用`whileTap`道具设置想要应用的动画效果。

我们可以用`onTap`道具监听敲击声:

```
import { motion } from "framer-motion";
import React from "react";function onTap(event, info) {
  console.log(info.point.x, info.point.y);
}export default function App() {
  return (
    <motion.div onTap={onTap} whileTap={{ scale: 0.8 }}>
      hello
    </motion.div>
  );
}
```

我们用`onTap`功能得到了抽头的位置。

`info.point.x`和`info.point.y`有攻丝的坐标。

# 平底锅

我们还可以监听带有帧运动的平移事件。

例如，我们可以写:

```
import { motion } from "framer-motion";
import React from "react";function onPan(event, info) {
  console.log(info.point.x, info.point.y);
}export default function App() {
  return <motion.div onPan={onPan}>hello</motion.div>;
}
```

去听潘的声音。

当我们拖动 div 时，`onPan`就会运行。

成帧器运动组件也使用`onPanStart`和`onPanEnd`道具来监听平移的开始和结束。

例如，我们可以写:

```
import { motion } from "framer-motion";
import React from "react";function onPan(event, info) {
  console.log(info.point.x, info.point.y);
}function onPanStart(event, info) {
  console.log(info.point.x, info.point.y);
}function onPanEnd(event, info) {
  console.log(info.point.x, info.point.y);
}export default function App() {
  return (
    <motion.div onPan={onPan} onPanStart={onPanStart} onPanEnd={onPanEnd}>
      hello
    </motion.div>
  );
}
```

# 结论

我们可以通过成帧器动作库监听各种手势。