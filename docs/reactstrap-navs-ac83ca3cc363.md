# 反应阱—nav

> 原文：<https://blog.devgenius.io/reactstrap-navs-ac83ca3cc363?source=collection_archive---------4----------------------->

![](img/6acbf3ee5c212db04672c175f32355cd.png)

照片由[丹金](https://unsplash.com/@danielcgold?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Reactstrap 是为 React 制作的一个版本引导程序。

这是一组具有 Boostrap 陷阱样式的 React 组件。

在这篇文章中，我们将看看如何添加一个 navbar 和 nav 与 Reactstrap。

# 垂直导航

导航组件可以是垂直的。

这对于垂直显示链接很有用。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import { Nav, NavItem, NavLink } from "reactstrap";export default function App() {
  return (
    <div>
      <Nav vertical>
        <NavLink href="#">foo</NavLink>
        <NavLink href="#">bar</NavLink>
        <NavLink href="#">baz</NavLink>
        <NavLink disabled href="#">
          disabled
        </NavLink>
      </Nav>
    </div>
  );
}
```

我们所做的就是将`vertical`道具添加到`Nav`上，它将垂直渲染。

我们可以用基于列表的导航做同样的事情:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import { Nav, NavItem, NavLink } from "reactstrap";export default function App() {
  return (
    <div>
      <Nav vertical>
        <NavItem>
          <NavLink href="#">foo</NavLink>
        </NavItem>
        <NavItem>
          <NavLink href="#">bar</NavLink>
        </NavItem>
        <NavItem>
          <NavLink href="#">baz</NavLink>
        </NavItem>
        <NavItem>
          <NavLink disabled href="#">
            disabled
          </NavLink>
        </NavItem>
      </Nav>
    </div>
  );
}
```

# 制表符

我们可以禁用标签形式的导航。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import {
  Nav,
  NavItem,
  Dropdown,
  DropdownItem,
  DropdownToggle,
  DropdownMenu,
  NavLink
} from "reactstrap";export default function App() {
  const [dropdownOpen, setDropdownOpen] = React.useState(false); const toggle = () => setDropdownOpen(!dropdownOpen); return (
    <div>
      <Nav tabs>
        <NavItem>
          <NavLink href="#" active>
            link
          </NavLink>
        </NavItem>
        <Dropdown nav isOpen={dropdownOpen} toggle={toggle}>
          <DropdownToggle nav caret>
            drop down
          </DropdownToggle>
          <DropdownMenu>
            <DropdownItem header>1</DropdownItem>
            <DropdownItem disabled>2</DropdownItem>
            <DropdownItem>3</DropdownItem>
            <DropdownItem divider />
            <DropdownItem>4</DropdownItem>
          </DropdownMenu>
        </Dropdown>
        <NavItem>
          <NavLink href="#">another link</NavLink>
        </NavItem>
        <NavItem>
          <NavLink href="#">some Link</NavLink>
        </NavItem>
        <NavItem>
          <NavLink disabled href="#">
            disabled
          </NavLink>
        </NavItem>
      </Nav>
    </div>
  );
}
```

我们有一个`Nav`和`tabs`道具来显示标签导航。

我们有`NavItem`组件来显示导航项目。

`NavLink`有链接。

我们可以用`Dropdown`组件给它添加一个下拉菜单。

在它里面，我们有`DropdownToggle`和`DropdownMenu`组件。

下拉由`isOpen`道具控制，道具由`toggle`回调控制。

`nav`支柱使其适合导航。

`active`道具高亮显示导航项目。

# 药丸

我们也可以展示药丸形式的 nav。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import {
  Nav,
  NavItem,
  Dropdown,
  DropdownItem,
  DropdownToggle,
  DropdownMenu,
  NavLink
} from "reactstrap";export default function App() {
  const [dropdownOpen, setDropdownOpen] = React.useState(false); const toggle = () => setDropdownOpen(!dropdownOpen); return (
    <div>
      <Nav pills>
        <NavItem>
          <NavLink href="#" active>
            link
          </NavLink>
        </NavItem>
        <Dropdown nav isOpen={dropdownOpen} toggle={toggle}>
          <DropdownToggle nav caret>
            drop down
          </DropdownToggle>
          <DropdownMenu>
            <DropdownItem header>1</DropdownItem>
            <DropdownItem disabled>2</DropdownItem>
            <DropdownItem>3</DropdownItem>
            <DropdownItem divider />
            <DropdownItem>4</DropdownItem>
          </DropdownMenu>
        </Dropdown>
        <NavItem>
          <NavLink href="#">another link</NavLink>
        </NavItem>
        <NavItem>
          <NavLink href="#">some Link</NavLink>
        </NavItem>
        <NavItem>
          <NavLink disabled href="#">
            disabled
          </NavLink>
        </NavItem>
      </Nav>
    </div>
  );
}
```

我们所做的就是将`tabs`道具和`pills`道具互换。

![](img/2a552d97767631cf63731db371562ac2.png)

阿尔瓦罗·雷耶斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以用各种方式显示 nav。

它们可以显示为标签或药丸。

我们也可以添加下拉菜单。

它们也可以是垂直的。