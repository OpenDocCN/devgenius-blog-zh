# 使用 React 图标将图标添加到我们的 React 应用程序中

> 原文：<https://blog.devgenius.io/add-icons-into-our-react-app-with-react-icons-4208ba8594ff?source=collection_archive---------3----------------------->

![](img/8c610a7c4da58b66b55094c7a3e57b1d.png)

埃文·德沃金在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

React Icons 是一个有很多图标的库，我们可以添加到 React 应用程序中。

在本文中，我们将了解如何使用 React 图标向 React 应用程序添加图标。

# 装置

我们可以通过运行以下命令来安装该软件包:

```
npm install react-icons --save
```

# 添加图标

安装完软件包后，我们可以通过导入图标组件来添加图标:

```
import React from "react";
import { FaBeer } from "react-icons/fa";export default function App() {
  return (
    <h3>
      Lets go for a <FaBeer />?
    </h3>
  );
}
```

# 蚂蚁设计图标

我们可以用`react-icons/ai`模块添加 Ant 设计图标:

```
import React from "react";
import { AiFillAccountBook } from "react-icons/ai";export default function App() {
  return (
    <h3>
      <AiFillAccountBook />
    </h3>
  );
}
```

图标的完整列表在[https://react-icons.github.io/react-icons/icons?name=ai](https://react-icons.github.io/react-icons/icons?name=ai)

# 引导图标

我们可以用`react-icon/bs`模块添加引导图标:

```
import React from "react";
import { BsFillAlarmFill } from "react-icons/bs";export default function App() {
  return (
    <h3>
      <BsFillAlarmFill />
    </h3>
  );
}
```

图标的完整列表在[https://react-icons.github.io/react-icons/icons?name=bs](https://react-icons.github.io/react-icons/icons?name=bs)

# 方框图标

我们可以从`react-icon/bi`模块添加方框图标:

```
import React from "react";
import { BiAbacus } from "react-icons/bi";export default function App() {
  return (
    <h3>
      <BiAbacus />
    </h3>
  );
}
```

完整的图标列表在[https://react-icons.github.io/react-icons/icons?name=bi](https://react-icons.github.io/react-icons/icons?name=bi)

# Devicons

我们可以从`react-icons/di`模块添加 Devicons。

例如，我们写道:

```
import React from "react";
import { DiAndroid } from "react-icons/di";export default function App() {
  return (
    <h3>
      <DiAndroid />
    </h3>
  );
}
```

添加 Android 图标。

设计者的完整名单在 https://react-icons.github.io/react-icons/icons?name=di

# 羽毛图标

我们可以从`react-icons/fi`模块添加羽毛图标。

例如，我们写道:

```
import React from "react";
import { FiActivity } from "react-icons/fi";export default function App() {
  return (
    <h3>
      <FiActivity />
    </h3>
  );
}
```

添加活动图标。

完整的图标列表在[https://react-icons.github.io/react-icons/icons?name=fi](https://react-icons.github.io/react-icons/icons?name=fi)

# 单色图标

我们可以从`react-icons/fc`模块添加单色图标。

例如，我们写道:

```
import React from "react";
import { FcAbout } from "react-icons/fc";export default function App() {
  return (
    <h3>
      <FcAbout />
    </h3>
  );
}
```

添加“关于”图标。

平面彩色图标的完整列表在 https://react-icons.github.io/react-icons/icons?name=fc[的](https://react-icons.github.io/react-icons/icons?name=fc)

# 字体真棒

字体牛逼图标可以用`react-icons/fa`模块添加。

要添加一个，我们写:

```
import React from "react";
import { Fa500Px } from "react-icons/fa";export default function App() {
  return (
    <h3>
      <Fa500Px />
    </h3>
  );
}
```

添加字体牛逼图标。

完整的图标列表可以在[https://react-icons.github.io/react-icons/icons?name=fa](https://react-icons.github.io/react-icons/icons?name=fa)找到

# 结论

我们可以使用 React 图标将各种图标库中的图标添加到 React 应用程序中。