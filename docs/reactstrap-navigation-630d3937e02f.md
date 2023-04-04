# 反应陷阱—导航

> 原文：<https://blog.devgenius.io/reactstrap-navigation-630d3937e02f?source=collection_archive---------2----------------------->

![](img/7c14303bb52251f12982a2c32a4f5485.png)

维多利亚博物馆在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Reactstrap 是为 React 制作的一个版本引导程序。

这是一组具有 Boostrap 陷阱样式的 React 组件。

在这篇文章中，我们将看看如何添加一个 navbar 和 nav 与 Reactstrap。

# 基本导航条

我们可以创建一个包含各种组件的导航栏。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import {
  Collapse,
  Navbar,
  NavbarToggler,
  NavbarBrand,
  Nav,
  NavItem,
  NavLink,
  UncontrolledDropdown,
  DropdownToggle,
  DropdownMenu,
  DropdownItem,
  NavbarText
} from "reactstrap";export default function App() {
  const [isOpen, setIsOpen] = React.useState(false);const toggle = () => setIsOpen(!isOpen);return (
    <div>
      <Navbar color="light" light expand="md">
        <NavbarBrand href="/">app</NavbarBrand>
        <NavbarToggler onClick={toggle} />
        <Collapse isOpen={isOpen} navbar>
          <Nav className="mr-auto" navbar>
            <NavItem>
              <NavLink href="/foo/">foo</NavLink>
            </NavItem>
            <NavItem>
              <NavLink href="https://example.com">example</NavLink>
            </NavItem>
            <UncontrolledDropdown nav inNavbar>
              <DropdownToggle nav caret>
                options
              </DropdownToggle>
              <DropdownMenu right>
                <DropdownItem>option 1</DropdownItem>
                <DropdownItem>option 2</DropdownItem>
                <DropdownItem divider />
                <DropdownItem>reset</DropdownItem>
              </DropdownMenu>
            </UncontrolledDropdown>
          </Nav>
          <NavbarText>more text</NavbarText>
        </Collapse>
      </Navbar>
    </div>
  );
}
```

`Narbar`是整个 navbar 容器。我们可以用`color`道具改变颜色。

`light`使颜色变淡。

`expand`有断点将导航条扩展到完整的导航条。

`NavbarBrand`有商标名称或标志。

`NavItem`有导航项

`NavLink`是导航栏上的一个链接。

`UncontrolledDropdown`是下拉菜单。

我们需要`nav`和`inNavbar`道具来使它适合导航条。

`DropdownToggle`有下拉开关。

`NavbarText`有正文。

`NavbarToggler`具有导航条菜单的切换功能。

我们将它们都放在`Collapse`组件中，这样当断点小于用`expand`指定的断点时，它们就会折叠。

# NavbarToggler

`NavbarToggler`应该在`NavbarBrand`之后，这样它就出现在右边。

我们可以翻转订单，让它出现在左侧。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import {
  Collapse,
  Navbar,
  NavbarToggler,
  NavbarBrand,
  Nav,
  NavItem,
  NavLink
} from "reactstrap";export default function App() {
  const [collapsed, setCollapsed] = React.useState(true);const toggleNavbar = () => setCollapsed(!collapsed);return (
    <div>
      <Navbar color="faded" light>
        <NavbarBrand href="/" className="mr-auto">
          app
        </NavbarBrand>
        <NavbarToggler onClick={toggleNavbar} className="mr-2" />
        <Collapse isOpen={!collapsed} navbar>
          <Nav navbar>
            <NavItem>
              <NavLink href="/foo/">foo</NavLink>
            </NavItem>
            <NavItem>
              <NavLink href="https://example.com">GitHub</NavLink>
            </NavItem>
          </Nav>
        </Collapse>
      </Navbar>
    </div>
  );
}
```

使用`Navbar`组件添加导航条。

然后我们加上`NavbarBrand`和`NavbarToggler`赢得我们想要的顺序。

需要一个`onClick`道具让我们通过设置`collapsed`状态来切换导航条。

# 航行

`Nav`组件是用于导航的另一个组件。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import { Nav, NavItem, NavLink } from "reactstrap";export default function App() {
  return (
    <div>
      <Nav>
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

我们有`Nav`组件，其中有`NavItem`和`NavLink`组件来添加链接。

此外，我们可以通过编写以下代码将`NavLink`与`Nav`组件嵌套在一起:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import { Nav, NavItem, NavLink } from "reactstrap";export default function App() {
  return (
    <div>
      <Nav>
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

在这两个例子中，`disabled`属性禁用了`NavLink`。

![](img/241ed2121b56aa348e239d688edd158f.png)

由[维多利亚博物馆](https://unsplash.com/@museumsvictoria?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以用 Reactstrap 很容易地添加 navbars 和 nav。

它们反应灵敏，因此适用于各种尺寸的屏幕。