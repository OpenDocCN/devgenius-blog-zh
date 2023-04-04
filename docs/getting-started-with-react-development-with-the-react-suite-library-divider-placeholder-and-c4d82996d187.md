# 使用 React 套件库(分隔线、占位符和进度条)开始 React 开发

> 原文：<https://blog.devgenius.io/getting-started-with-react-development-with-the-react-suite-library-divider-placeholder-and-c4d82996d187?source=collection_archive---------6----------------------->

![](img/566c54ee237e21e081de04d6734a8479.png)

照片由[戴恩·托普金](https://unsplash.com/@dtopkin1?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

React Suite 是一个有用的 UI 库，让我们可以轻松地将许多组件添加到 React 应用程序中。

在本文中，我们将了解如何使用它向 React 应用程序添加组件。

# 圆规

我们可以用`Divider`组件在我们的应用程序中添加一个分隔线。

例如，我们可以写:

```
import React from "react";
import { Divider } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div>
      <p>hello</p>
      <Divider />
      <p>world</p>
    </div>
  );
}
```

在 2 个`p`元素之间添加分隔线。

我们可以添加带文本的分隔线:

```
import React from "react";
import { Divider } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div>
      <Divider>Divider</Divider>
    </div>
  );
}
```

它将显示在分隔线的中间。

我们可以用`vertical`道具添加垂直分隔线:

```
import React from "react";
import { Divider } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div>
      <span>Edit</span>
      <Divider vertical />
      <span>Update</span>
      <Divider vertical />
      <span>Save</span>
    </div>
  );
}
```

# 进度条

我们可以用`Progress`模块添加一个进度条。

例如，我们可以写:

```
import React from "react";
import { Progress } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div>
      <Progress.Line percent={30} status="active" />
    </div>
  );
}
```

添加进度条。

`percent`是进度百分比。

`status`有条形的颜色。

我们可以隐藏右侧的百分比显示，将`showInfo`设置为`false`:

```
import React from "react";
import { Progress } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div>
      <Progress.Line percent={30} status="active" showInfo={false} />
    </div>
  );
}
```

此外，我们可以用`Progress.Circle`组件添加一个进度圈:

```
import React from "react";
import { Progress } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div>
      <Progress.Circle percent={30} strokeColor="#ffc107" />
    </div>
  );
}
```

# 占位符

我们可以使用`Placeholder`模块将占位符添加到应用程序中。

例如，我们可以写:

```
import React from "react";
import { Placeholder } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div>
      <Placeholder.Paragraph style={{ marginTop: 30 }} graph="circle" />
    </div>
  );
}
```

添加段落占位符。

我们将`graph`设置为`'circle'`在左侧添加一个圆。

我们也可以设置为`'square'`和`'image'`。

同样，我们可以用`Placeholder.Grid`添加一个网格占位符:

```
import React from "react";
import { Placeholder } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div>
      <Placeholder.Grid rows={5} columns={6} active />
    </div>
  );
}
```

我们可以用`Placeholder.Graph`添加一个图形占位符:

```
import React from "react";
import { Placeholder } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div>
      <Placeholder.Graph active />
    </div>
  );
}
```

# 结论

我们可以使用 React Suite 在应用程序中添加分隔线、进度条和占位符。