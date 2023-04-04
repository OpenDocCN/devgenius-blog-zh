# 使用 React 套件库开始 React 开发—下拉菜单

> 原文：<https://blog.devgenius.io/getting-started-with-react-development-with-the-react-suite-library-dropdowns-ee41acf90f6?source=collection_archive---------6----------------------->

![](img/880fd2817c230bd993ac48d03ff504eb.png)

由[和](https://unsplash.com/@kazuend?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

React Suite 是一个有用的 UI 库，让我们可以轻松地将许多组件添加到 React 应用程序中。

在本文中，我们将了解如何使用它向 React 应用程序添加组件。

# 下拉菜单

我们可以用`Dropdown`组件添加一个选择下拉菜单。

例如，我们可以写:

```
import React from "react";
import { Dropdown } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div className="App">
      <Dropdown title="Fruit">
        <Dropdown.Item>Apple</Dropdown.Item>
        <Dropdown.Item>Orange</Dropdown.Item>
        <Dropdown.Item>Grape</Dropdown.Item>
      </Dropdown>
    </div>
  );
}
```

`title`道具有占位符。

`Dropdown.Item`有下拉项。

我们可以改变下拉菜单的打开方式。

例如，我们可以通过设置`trigger`道具使它在悬停时打开:

```
import React from "react";
import { Dropdown } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div className="App">
      <Dropdown title="Fruit" trigger="hover">
        <Dropdown.Item>Apple</Dropdown.Item>
        <Dropdown.Item>Orange</Dropdown.Item>
        <Dropdown.Item>Grape</Dropdown.Item>
      </Dropdown>
    </div>
  );
}
```

我们可以通过将`trigger`设置为`'contextMenu'`来制作一个上下文菜单:

```
import React from "react";
import { Dropdown } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div className="App">
      <Dropdown title="Fruit" trigger="contextMenu">
        <Dropdown.Item>Apple</Dropdown.Item>
        <Dropdown.Item>Orange</Dropdown.Item>
        <Dropdown.Item>Grape</Dropdown.Item>
      </Dropdown>
    </div>
  );
}
```

`trigger`的默认值是`'click'`。

我们还可以为每个项目分配一个键，并设置默认键。

为此，我们写道:

```
import React from "react";
import { Dropdown } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div className="App">
      <Dropdown title="Fruit" activeKey="a">
        <Dropdown.Item eventKey="a">Apple</Dropdown.Item>
        <Dropdown.Item eventKey="b">Orange</Dropdown.Item>
        <Dropdown.Item eventKey="c">Grape</Dropdown.Item>
      </Dropdown>
    </div>
  );
}
```

我们设置`activeKey`到`'a'`来设置激活键。

`eventKey`被分配给每一项。

如果`activeKey`和`eventKey`匹配，则该项目将被高亮显示。

要禁用一个项目，我们可以给它添加`disabled`道具:

```
import React from "react";
import { Dropdown } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div className="App">
      <Dropdown title="Fruit">
        <Dropdown.Item disabled>Apple</Dropdown.Item>
        <Dropdown.Item>Orange</Dropdown.Item>
        <Dropdown.Item>Grape</Dropdown.Item>
      </Dropdown>
    </div>
  );
}
```

我们可以用`size`道具改变下拉按钮的大小:

```
import React from "react";
import { Dropdown } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div className="App">
      <Dropdown title="Fruit" size="lg">
        <Dropdown.Item disabled>Apple</Dropdown.Item>
        <Dropdown.Item>Orange</Dropdown.Item>
        <Dropdown.Item>Grape</Dropdown.Item>
      </Dropdown>
    </div>
  );
}
```

`'lg'`大。

我们可以用`'xs'`把它变得特别小。

`'sm'`小。而`'md'`是中等。

我们可以用`noCaret`属性从下拉按钮中删除插入符号:

```
import React from "react";
import { Dropdown } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div className="App">
      <Dropdown title="Fruit" noCaret>
        <Dropdown.Item disabled>Apple</Dropdown.Item>
        <Dropdown.Item>Orange</Dropdown.Item>
        <Dropdown.Item>Grape</Dropdown.Item>
      </Dropdown>
    </div>
  );
}
```

我们可以用`icon`道具给按钮添加一个图标:

```
import React from "react";
import { Dropdown, Icon } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div className="App">
      <Dropdown title="Fruit" icon={<Icon icon="file" />}>
        <Dropdown.Item disabled>Apple</Dropdown.Item>
        <Dropdown.Item>Orange</Dropdown.Item>
        <Dropdown.Item>Grape</Dropdown.Item>
      </Dropdown>
    </div>
  );
}
```

# 结论

我们可以使用 React Suite 将简单的下拉菜单添加到 React 应用程序中。