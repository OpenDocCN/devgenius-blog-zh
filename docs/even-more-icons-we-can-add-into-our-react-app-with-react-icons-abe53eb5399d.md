# 我们可以用 React 图标将更多图标添加到 React 应用程序中

> 原文：<https://blog.devgenius.io/even-more-icons-we-can-add-into-our-react-app-with-react-icons-abe53eb5399d?source=collection_archive---------6----------------------->

![](img/cf8a64be59a30aadca91daf6ac1348ad.png)

照片由[亚尼斯·萨乌格](https://unsplash.com/@yaza34?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

React Icons 是一个有很多图标的库，我们可以添加到 React 应用程序中。

在本文中，我们将了解如何使用 React 图标向 React 应用程序添加图标。

# 材料设计图标

我们可以从`react-icons/md`系列中添加材料设计图标。

例如，我们可以写:

```
import React from "react";
import { Md3DRotation } from "react-icons/md";export default function App() {
  return (
    <h3>
      <Md3DRotation />
    </h3>
  );
}
```

从集合中添加图标。

完整的图标列表可以在[https://react-icons.github.io/react-icons/icons?name=md](https://react-icons.github.io/react-icons/icons?name=md)找到

# 混音图标

React Icons 附带 Remix Icon collection。

我们可以从`react-icons/ri`模块中导入它们。

例如，我们可以写:

```
import React from "react";
import { RiAncientGateLine } from "react-icons/ri";export default function App() {
  return (
    <h3>
      <RiAncientGateLine />
    </h3>
  );
}
```

完整的图标列表可以在[https://react-icons.github.io/react-icons/icons?name=ri](https://react-icons.github.io/react-icons/icons?name=ri)找到

# 简单图标

简单图标集合包含在反应图标的`react-icons/si`模块中。

例如，我们可以写:

```
import React from "react";
import { Si1001Tracklists } from "react-icons/si";export default function App() {
  return (
    <h3>
      <Si1001Tracklists />
    </h3>
  );
}
```

添加图标。

图标的完整列表在[https://react-icons.github.io/react-icons/icons?name=si](https://react-icons.github.io/react-icons/icons?name=si)

# 典型图标

Typicons 集合包含在 React Icons 的`react-icons/ti`模块中。

例如，我们可以写:

```
import React from "react";
import { TiAdjustBrightness } from "react-icons/ti";export default function App() {
  return (
    <h3>
      <TiAdjustBrightness />
    </h3>
  );
}
```

添加图标。

图标的完整列表在[https://react-icons.github.io/react-icons/icons?name=ti](https://react-icons.github.io/react-icons/icons?name=ti)

# VS 代码图标

React Icons 附带了 Visual Studio 代码编辑器使用的图标。

这些图标包含在`react-icons/vsc`系列中。

例如，我们可以写:

```
import React from "react";
import { VscAccount } from "react-icons/vsc";export default function App() {
  return (
    <h3>
      <VscAccount />
    </h3>
  );
}
```

从集合中添加图标。

图标的完整列表在[https://react-icons.github.io/react-icons/icons?name=vsc](https://react-icons.github.io/react-icons/icons?name=vsc)

# 天气图标

我们可以从天气图标集合中添加天气图标。

它们包含在`react-icons/wi`系列中。

要添加一个，我们写:

```
import React from "react";
import { WiAlien } from "react-icons/wi";export default function App() {
  return (
    <h3>
      <WiAlien />
    </h3>
  );
}
```

完整的图标列表可以在 https://react-icons.github.io/react-icons/icons?name=wi 找到

# css.gg

React Icons 附带 css.gg 图标集合。

它们包含在`react-icons/gg`模块中。

要添加一个，我们写:

```
import React from "react";
import { CgAbstract } from "react-icons/cg";export default function App() {
  return (
    <h3>
      <CgAbstract />
    </h3>
  );
}
```

从集合中添加图标。

完整的图标列表可以在[https://react-icons.github.io/react-icons/icons?name=cg](https://react-icons.github.io/react-icons/icons?name=cg)找到

# 结论

我们可以使用 React 图标将各种图标库中的图标添加到 React 应用程序中。