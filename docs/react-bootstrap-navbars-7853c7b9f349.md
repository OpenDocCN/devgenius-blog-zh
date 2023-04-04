# React 引导程序—导航条

> 原文：<https://blog.devgenius.io/react-bootstrap-navbars-7853c7b9f349?source=collection_archive---------3----------------------->

![](img/9f1247d576e737412d2c4adbc55dfb7e.png)

由[玛丽亚·特内娃](https://unsplash.com/@miteneva?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

React Bootstrap 是为 React 开发的 Bootstrap 的一个版本。

这是一组具有引导风格的 React 组件。

在本文中，我们将了解如何使用 React Bootstrap 向 React 应用程序添加导航条。

# 导航条

我们可以添加导航栏来添加一个支持品牌和导航的导航标题。

我们可以使用`expand`属性来允许在较低的断点处折叠导航条。

默认情况下，导航条及其内容是不固定的。

要制作导航条，我们可以使用`Navbar`组件:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import Navbar from "react-bootstrap/Navbar";
import Nav from "react-bootstrap/Nav";
import Form from "react-bootstrap/Form";
import Button from "react-bootstrap/Button";
import FormControl from "react-bootstrap/FormControl";
import NavDropdown from "react-bootstrap/NavDropdown";export default function App() {
  return (
    <>
      <Navbar bg="light" expand="lg">
        <Navbar.Brand href="#home">nav bar</Navbar.Brand>
        <Navbar.Toggle />
        <Navbar.Collapse>
          <Nav className="mr-auto">
            <Nav.Link href="#home">home</Nav.Link>
            <Nav.Link href="#foo">foo</Nav.Link>
            <NavDropdown title="Dropdown">
              <NavDropdown.Item href="#action/1">action 1</NavDropdown.Item>
              <NavDropdown.Item href="#action/2">action 2</NavDropdown.Item>
              <NavDropdown.Item href="#action/3">action 3</NavDropdown.Item>
              <NavDropdown.Divider />
              <NavDropdown.Item href="#action/4">action 4</NavDropdown.Item>
            </NavDropdown>
          </Nav>
          <Form inline>
            <FormControl type="text" placeholder="search" className="mr-sm-2" />
            <Button variant="outline-primary">search</Button>
          </Form>
        </Navbar.Collapse>
      </Navbar>
    </>
  );
}
```

我们用`NavBar`组件创建了一个导航栏。

`bg`道具有背景色，。

`Navbar.Brand`左侧有品牌名称。

`Navbar.Toggle`是在折叠的导航栏中打开菜单的开关。

`Navbar.Collapse`是一个导航条，当屏幕变窄时会自动折叠。

`Nav.Link`有链接。

`NavDropdown`有下拉菜单。

`title`有显示在下拉开关中的文本。

我们还有一个内嵌的`Form`元素，让我们在导航中添加一个表单。

# Navbar 品牌

品牌可以是图像或文本。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import Navbar from "react-bootstrap/Navbar";export default function App() {
  return (
    <>
      <Navbar bg="light">
        <Navbar.Brand href="#home">app</Navbar.Brand>
      </Navbar>
    </>
  );
}
```

向左侧添加文本。

`bg`是背景色，可以是`light`或`dark`。

我们也可以通过书写来添加图像:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import Navbar from "react-bootstrap/Navbar";export default function App() {
  return (
    <>
      <Navbar bg="light">
        <Navbar.Brand href="#home">
          <img
            alt="logo"
            src="http://placekitten.com/200/200"
            width="30"
            height="30"
            className="d-inline-block align-top"
          />{" "}
          app
        </Navbar.Brand>
      </Navbar>
    </>
  );
}
```

为 nav 品牌添加一个徽标。

我们将`className`设置为`d-inline-block`和`align-top`，为徽标添加一些边距。

# 形式

如果表格是内嵌的，我们可以把它们添加到导航栏上。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import Navbar from "react-bootstrap/Navbar";
import Form from "react-bootstrap/Form";
import InputGroup from "react-bootstrap/InputGroup";
import FormControl from "react-bootstrap/FormControl";
import Button from "react-bootstrap/Button";export default function App() {
  return (
    <>
      <Navbar className="bg-light justify-content-between">
        <Form inline>
          <InputGroup>
            <InputGroup.Prepend>
              <InputGroup.Text>@</InputGroup.Text>
            </InputGroup.Prepend>
            <FormControl
              placeholder="name"
            />
          </InputGroup>
        </Form>
        <Form inline>
          <FormControl type="text" placeholder="search" className=" mr-sm-2" />
          <Button type="submit">Submit</Button>
        </Form>
      </Navbar>
    </>
  );
}
```

我们添加了`justifyp-content-between`道具，使两个窗体显示在左右两边。

`Form`设置为`inline`,这样它们就可以内嵌显示。

在`Form`里面，我们有通常的`InputGroup`和`FormControl` s。

# 文本和非导航链接

我们可以添加不与`Navbar.Text`组件链接的导航文本。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import Navbar from "react-bootstrap/Navbar";export default function App() {
  return (
    <>
      <Navbar className="bg-light justify-content-between">
        <Navbar.Brand href="#home">text navbar</Navbar.Brand>
        <Navbar.Toggle />
        <Navbar.Collapse className="justify-content-end">
          <Navbar.Text>
            hello: <a href="#login">james</a>
          </Navbar.Text>
        </Navbar.Collapse>
      </Navbar>
    </>
  );
}
```

我们有一个`Navbar.Text`组件，其中显示了一些文本和一个链接。

# 配色方案

我们可以用`variant`和`bg`道具来改变配色方案。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import Navbar from "react-bootstrap/Navbar";
import Nav from "react-bootstrap/Nav";export default function App() {
  return (
    <>
      <Navbar bg="primary" variant="dark">
        <Navbar.Brand href="#home">Navbar</Navbar.Brand>
        <Nav className="mr-auto">
          <Nav.Link href="#home">Home</Nav.Link>
          <Nav.Link href="#foo">foo</Nav.Link>
          <Nav.Link href="#bar">bar</Nav.Link>
        </Nav>
      </Navbar>
    </>
  );
}
```

我们有一个`bg`设置为`primary`的导航条来设置背景颜色为`blue`。

`variant`被`bg`覆盖。

![](img/8a8eebb157bf12b789102f9cef0d6dc9.png)

照片由 [Ricardo Moura](https://unsplash.com/@ricardomoura?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以在导航条上添加表单和下拉菜单。

他们有反应。

此外，我们可以改变导航条的颜色。