# React 引导—自定义导航条

> 原文：<https://blog.devgenius.io/react-bootstrap-customize-navbars-6e6963dcc515?source=collection_archive---------0----------------------->

![](img/75542c11bcb0e5a992901113fd840695.png)

约瑟芬·巴兰在 Unsplash 的照片

React Bootstrap 是为 React 开发的 Bootstrap 的一个版本。

这是一组具有引导风格的 React 组件。

在本文中，我们将了解如何使用 React Bootstrap 为 React 应用程序定制导航条。

# 容器

我们可以在`Container`组件中包装一个导航条，使其在页面上居中。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import Navbar from "react-bootstrap/Navbar";
import Nav from "react-bootstrap/Nav";
import Container from "react-bootstrap/Container";export default function App() {
  return (
    <>
      <Container>
        <Navbar bg="primary" variant="dark">
          <Navbar.Brand href="#home">Navbar</Navbar.Brand>
          <Nav className="mr-auto">
            <Nav.Link href="#home">Home</Nav.Link>
            <Nav.Link href="#foo">foo</Nav.Link>
            <Nav.Link href="#bar">bar</Nav.Link>
          </Nav>
        </Navbar>
      </Container>
    </>
  );
}
```

我们用`Container`将导航条居中。

# 安置

我们可以用 Bootstrap 自带的定位工具来改变导航条的位置。

我们传入`fixed`或`sticky`道具来让导航贴在顶部或底部。

例如，我们可以写:

```
<Navbar fixed="top" />
```

使导航条固定在顶部。

我们可以写:

```
<Navbar fixed="bottom" />
```

把它固定在底部。

我们可以用`sticky`代替`fixed`。

相对于视口显示固定。

而 sticky 是基于用户的滚动位置来定位的。

所以我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import Navbar from "react-bootstrap/Navbar";
import Nav from "react-bootstrap/Nav";
import Container from "react-bootstrap/Container";export default function App() {
  return (
    <>
      <Container>
        <Navbar fixed="top">
          <Navbar.Brand href="#home">Navbar</Navbar.Brand>
          <Nav className="mr-auto">
            <Nav.Link href="#home">Home</Nav.Link>
            <Nav.Link href="#foo">foo</Nav.Link>
            <Nav.Link href="#bar">bar</Nav.Link>
          </Nav>
        </Navbar>
      </Container>
    </>
  );
}
```

使导航栏始终保持在视口的顶部。

# 响应行为

为了使导航条有响应性，我们添加了`Navbar.Toggle`和`Navbar.Collkapse`组件，以便当屏幕比给定的断点更窄时，将折叠的导航条显示为菜单。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import Navbar from "react-bootstrap/Navbar";
import Nav from "react-bootstrap/Nav";
import NavDropdown from "react-bootstrap/NavDropdown";export default function App() {
  return (
    <>
      <Navbar collapseOnSelect expand="lg" bg="dark" variant="dark">
        <Navbar.Brand href="#home">App</Navbar.Brand>
        <Navbar.Toggle />
        <Navbar.Collapse>
          <Nav className="mr-auto">
            <Nav.Link href="#foo">foo</Nav.Link>
            <Nav.Link href="#bar">bar</Nav.Link>
            <NavDropdown title="Dropdown" id="collasible-nav-dropdown">
              <NavDropdown.Item href="#action/1">action 1</NavDropdown.Item>
              <NavDropdown.Item href="#action/2">action 2</NavDropdown.Item>
              <NavDropdown.Item href="#action/3">action 3</NavDropdown.Item>
              <NavDropdown.Divider />
              <NavDropdown.Item href="#action/4">action 4</NavDropdown.Item>
            </NavDropdown>
          </Nav>
          <Nav>
            <Nav.Link href="#baz">baz</Nav.Link>
            <Nav.Link eventKey={2} href="#qux">
              qux
            </Nav.Link>
          </Nav>
        </Navbar.Collapse>
      </Navbar>
    </>
  );
}
```

创建一个导航栏，当屏幕比`lg`断点更窄时，它可以折叠成一个汉堡包菜单。

我们有`Navbar.Toggle`来显示导航栏折叠时的切换。

并且`Navbar.Collapse`让我们为窄屏幕显示一个折叠的导航条。

如果屏幕在`lg`断点或更高位置，它将扩展到整个导航栏。

在`Navbar.Collapse`组件中，我们有导航条的所有内容，比如下拉菜单、链接和表单。

`Navbar.Brand`显示在左侧。

![](img/7001e54d41826567171cc04067f12af3.png)

照片由 [Amy Tran](https://unsplash.com/@minhanh258?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 结论

我们可以用自己的风格定制导航条。

此外，我们可以通过添加一些组件和道具来使其具有响应性。