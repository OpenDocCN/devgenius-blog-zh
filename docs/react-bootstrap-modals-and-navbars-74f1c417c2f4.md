# 反应引导—模态和 nav

> 原文：<https://blog.devgenius.io/react-bootstrap-modals-and-navbars-74f1c417c2f4?source=collection_archive---------9----------------------->

![](img/18a321361eeb8d88cd226411de7fa210.png)

莱斯莉·德克森在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

React Bootstrap 是为 React 开发的 Bootstrap 的一个版本。

这是一组具有引导风格的 React 组件。

在本文中，我们将了解如何使用 React Bootstrap 向 React 应用程序添加模态和导航。

# 模态尺寸

我们可以用`size`道具改变模型的大小。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import Modal from "react-bootstrap/Modal";
import Button from "react-bootstrap/Button";export default function App() {
  const [show, setShow] = React.useState(false);const onClose = () => setShow(false);
  const onClick = () => setShow(true);return (
    <>
      <Button variant="primary" onClick={onClick}>
        open modal
      </Button> <Modal size="sm" show={show} onHide={onClose} keyboard={false}>
        <Modal.Header closeButton>
          <Modal.Title>Modal title</Modal.Title>
        </Modal.Header>
        <Modal.Body>
          Lorem ipsum dolor sit amet, consectetur adipiscing elit. Morbi
          efficitur massa tellus, non ultrices augue sagittis ornare. Sed
          ultrices ligula in est tempus tincidunt. Nam id consequat lacus, et
          mattis turpis. Sed velit nibh, tincidunt in ante a, accumsan fringilla
          odio. Etiam fermentum euismod enim id faucibus. Etiam facilisis nulla
          id leo imperdiet aliquet. In dignissim nulla non magna commodo
          ullamcorper. Fusce non ligula id tellus dapibus tempor sit amet in
          libero. Proin malesuada vulputate augue in bibendum. In mollis felis
          eu ante pharetra, ut vehicula sapien malesuada. Nulla imperdiet, urna
          a laoreet ullamcorper, massa nunc posuere neque, iaculis egestas urna
          arcu a metus.
        </Modal.Body>
        <Modal.Footer>
          <Button variant="secondary" onClick={onClose}>
            Close
          </Button>
          <Button variant="primary">Understood</Button>
        </Modal.Footer>
      </Modal>
    </>
  );
}
```

使模态变小。

为了让它们变大，我们可以将`size`道具设置为`lg`。

# 使用自定义 CSS 调整模型大小

我们可以使用自定义 CSS 来设置模态的大小。

为此，我们可以将`dialogClassName`道具添加到模态中。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import Modal from "react-bootstrap/Modal";
import Button from "react-bootstrap/Button";export default function App() {
  const [show, setShow] = React.useState(false);const onClose = () => setShow(false);
  const onClick = () => setShow(true); return (
    <>
      <Button variant="primary" onClick={onClick}>
        open modal
      </Button> <Modal
        dialogClassName="modal-90w"
        show={show}
        onHide={onClose}
        keyboard={false}
      >
        <Modal.Header closeButton>
          <Modal.Title>Modal title</Modal.Title>
        </Modal.Header>
        <Modal.Body>
          Lorem ipsum dolor sit amet, consectetur adipiscing elit. Morbi
          efficitur massa tellus, non ultrices augue sagittis ornare. Sed
          ultrices ligula in est tempus tincidunt. Nam id consequat lacus, et
          mattis turpis. Sed velit nibh, tincidunt in ante a, accumsan fringilla
          odio. Etiam fermentum euismod enim id faucibus. Etiam facilisis nulla
          id leo imperdiet aliquet. In dignissim nulla non magna commodo
          ullamcorper. Fusce non ligula id tellus dapibus tempor sit amet in
          libero. Proin malesuada vulputate augue in bibendum. In mollis felis
          eu ante pharetra, ut vehicula sapien malesuada. Nulla imperdiet, urna
          a laoreet ullamcorper, massa nunc posuere neque, iaculis egestas urna
          arcu a metus.
        </Modal.Body>
        <Modal.Footer>
          <Button variant="secondary" onClick={onClose}>
            Close
          </Button>
          <Button variant="primary">Understood</Button>
        </Modal.Footer>
      </Modal>
    </>
  );
}
```

我们将`dialogClassName`设置为一个类名，然后我们可以为这个类名添加我们自己的 CSS 规则。

# 航行

为了给我们的 React 应用程序添加导航，我们可以使用`Nav`组件。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import Nav from "react-bootstrap/Nav";export default function App() {
  return (
    <>
      <Nav
        activeKey="/home"
        onSelect={selectedKey => alert(`selected ${selectedKey}`)}
      >
        <Nav.Item>
          <Nav.Link href="/home">active</Nav.Link>
        </Nav.Item>
        <Nav.Item>
          <Nav.Link eventKey="foo">foo</Nav.Link>
        </Nav.Item>
        <Nav.Item>
          <Nav.Link eventKey="bar">bar</Nav.Link>
        </Nav.Item>
        <Nav.Item>
          <Nav.Link eventKey="disabled" disabled>
            disabled
          </Nav.Link>
        </Nav.Item>
      </Nav>
    </>
  );
}
```

我们有带`activeKey`支柱的`Nav`组件。

这将设置活动链接的`href` 。

`eventKey`是每个导航项目的唯一键。

在它里面，我们有添加导航项目的`Nav.Item`组件。

`href`有链接到的路径。

我们还可以将 nav 组件呈现为 ul 元素。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import Nav from "react-bootstrap/Nav";export default function App() {
  return (
    <>
      <Nav defaultActiveKey="/home" as="ul">
        <Nav.Item as="li">
          <Nav.Link href="/home">home</Nav.Link>
        </Nav.Item>
        <Nav.Item as="li">
          <Nav.Link eventKey="foo">foo</Nav.Link>
        </Nav.Item>
        <Nav.Item as="li">
          <Nav.Link eventKey="bar">bar</Nav.Link>
        </Nav.Item>
      </Nav>
    </>
  );
}
```

将我们的导航显示为 ul。

我们用`as`道具做到了。

`Nav`将`as`设置为`ul`，而`Nav.Item`将`as`设置为`li`。

![](img/97b1c4e76af1031402e82b1b4f66631c.png)

照片由[桑迪·米勒](https://unsplash.com/@sandym10?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以用类或`size`道具来确定模型的大小。

此外，我们可以添加一个 nav 与反应助推器。