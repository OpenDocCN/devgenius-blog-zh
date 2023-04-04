# 使用 React 套件库—导航菜单开始 React 开发

> 原文：<https://blog.devgenius.io/getting-started-with-react-development-with-the-react-suite-library-nav-menus-602dacee2f16?source=collection_archive---------7----------------------->

![](img/ae5445862d2ee71e38bef03253981f22.png)

照片由[日出照片](https://unsplash.com/@sunrisephotography?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

React Suite 是一个有用的 UI 库，让我们可以轻松地将许多组件添加到 React 应用程序中。

在本文中，我们将了解如何使用它向 React 应用程序添加组件。

# 导航菜单

我们可以用`Nav`组件添加一个导航菜单。

为了补充它，我们写道:

```
import React, { useState } from "react";
import { Nav, Icon } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  const [activeKey, setActiveKey] = useState(); const handleSelect = (activeKey) => {
    setActiveKey(activeKey);
  }; return (
    <div className="App">
      <Nav activeKey={activeKey} onSelect={handleSelect}>
        <Nav.Item eventKey="home" icon={<Icon icon="home" />}>
          Home
        </Nav.Item>
        <Nav.Item eventKey="news">News</Nav.Item>
        <Nav.Item eventKey="solutions">Solutions</Nav.Item>
      </Nav>
    </div>
  );
}
```

我们将`activeKey`传递给`activeKey`道具。

`handleSelect`让我们拿起`activeKey`并设置它。

`Nav.Item`有该菜单项。

当`activeKey`与`eventKey`匹配时，它会高亮显示。

我们可以用`icon`道具添加一个图标。

我们可以添加`vertical`道具使导航垂直:

```
import React, { useState } from "react";
import { Nav, Icon } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  const [activeKey, setActiveKey] = useState(); const handleSelect = (activeKey) => {
    setActiveKey(activeKey);
  }; return (
    <div className="App">
      <Nav vertical activeKey={activeKey} onSelect={handleSelect}>
        <Nav.Item eventKey="home" icon={<Icon icon="home" />}>
          Home
        </Nav.Item>
        <Nav.Item eventKey="news">News</Nav.Item>
        <Nav.Item eventKey="solutions">Solutions</Nav.Item>
      </Nav>
    </div>
  );
}
```

我们可以使用`active`道具将导航项目设置为活动状态:

```
import React, { useState } from "react";
import { Nav, Icon } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  const [activeKey, setActiveKey] = useState(); const handleSelect = (activeKey) => {
    setActiveKey(activeKey);
  }; return (
    <div className="App">
      <Nav onSelect={handleSelect}>
        <Nav.Item eventKey="home" active={activeKey === "home"}>
          Home
        </Nav.Item>
        <Nav.Item eventKey="news" active={activeKey === "news"}>
          News
        </Nav.Item>
        <Nav.Item eventKey="solutions" active={activeKey === "solutions"}>
          Solutions
        </Nav.Item>
      </Nav>
    </div>
  );
}
```

我们可以用`disabled`道具禁用一个物品:

```
import React, { useState } from "react";
import { Nav, Icon } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  const [activeKey, setActiveKey] = useState(); const handleSelect = (activeKey) => {
    setActiveKey(activeKey);
  }; return (
    <div className="App">
      <Nav activeKey={activeKey} onSelect={handleSelect}>
        <Nav.Item eventKey="home">Home</Nav.Item>
        <Nav.Item eventKey="news" disabled>
          News
        </Nav.Item>
        <Nav.Item eventKey="solutions">Solutions</Nav.Item>
      </Nav>
    </div>
  );
}
```

要使导航项目大小相等，我们可以添加`justified`道具:

```
import React, { useState } from "react";
import { Nav, Icon } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  const [activeKey, setActiveKey] = useState(); const handleSelect = (activeKey) => {
    setActiveKey(activeKey);
  }; return (
    <div className="App">
      <Nav activeKey={activeKey} justified onSelect={handleSelect}>
        <Nav.Item eventKey="home">Home</Nav.Item>
        <Nav.Item eventKey="news">News</Nav.Item>
        <Nav.Item eventKey="solutions">Solutions</Nav.Item>
      </Nav>
    </div>
  );
}
```

此外，我们可以使用`Dropdown`组件将下拉菜单添加到导航中:

```
import React, { useState } from "react";
import { Nav, Dropdown } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  const [activeKey, setActiveKey] = useState(); const handleSelect = (activeKey) => {
    setActiveKey(activeKey);
  }; return (
    <div className="App">
      <Nav activeKey={activeKey} onSelect={handleSelect}>
        <Nav.Item eventKey="home">Home</Nav.Item>
        <Nav.Item eventKey="news">News</Nav.Item>
        <Nav.Item eventKey="solutions">Solutions</Nav.Item>
        <Dropdown title="Fruit">
          <Dropdown.Item>Apple</Dropdown.Item>
          <Dropdown.Item>Orange</Dropdown.Item>
          <Dropdown.Menu title="Grape">
            <Dropdown.Item>Red</Dropdown.Item>
            <Dropdown.Item active>Green</Dropdown.Item>
          </Dropdown.Menu>
        </Dropdown>
      </Nav>
    </div>
  );
}
```

# 结论

我们可以使用 React Suite 将导航菜单添加到 React 应用程序中。