# React 引导—模式定制

> 原文：<https://blog.devgenius.io/react-bootstrap-modal-customizations-7f0f5ed128f8?source=collection_archive---------7----------------------->

![](img/ec529be4c8fb7db5f567cebbd3e81b84.png)

Lukasz Szmigiel 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

React Bootstrap 是为 React 开发的 Bootstrap 的一个版本。

这是一组具有引导风格的 React 组件。

在本文中，我们将了解如何使用 React Bootstrap 向 React 应用程序添加模态。

# 没有动画的模态

如果我们将`animation`属性设置为`false`，我们可以禁用模态上的动画。

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
        animation={false}
        show={show}
        onHide={onClose}
        backdrop="static"
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
          arcu a metus. escape key.
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

我们将`animation`属性设置为`false`，这样当我们打开或关闭模态时就不会看到任何动画。

我们有打开模态按钮来设置`show`状态为`true`来打开模态。

我们将`onHide`属性设置为`onClose`按钮，当我们单击关闭按钮时，它会关闭。

`keyboard`设置为`false`禁用 Esc 键关闭。

# 垂直居中

我们可以让模态垂直居中。

为此，我们只需添加`centered`道具。

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
      </Button> <Modal centered show={show} onHide={onClose} keyboard={false}>
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
          arcu a metus. escape key.
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

当`centered`支柱在`Modal`上时，模态将垂直居中。

# 使用网格

我们可以在模态中使用布局网格。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import Modal from "react-bootstrap/Modal";
import Button from "react-bootstrap/Button";
import Row from "react-bootstrap/Row";
import Col from "react-bootstrap/Col";
import Container from "react-bootstrap/Container";export default function App() {
  const [show, setShow] = React.useState(false);const onClose = () => setShow(false);
  const onClick = () => setShow(true); return (
    <>
      <Button variant="primary" onClick={onClick}>
        open modal
      </Button> <Modal centered show={show} onHide={onClose} keyboard={false}>
        <Modal.Header closeButton>
          <Modal.Title>Modal title</Modal.Title>
        </Modal.Header>
        <Modal.Body>
          <Container>
            <Row>
              <Col xs={12} md={6}>
                foo
              </Col>
              <Col xs={12} md={6}>
                bar
              </Col>
            </Row> <Row>
              <Col xs={6} md={8}>
                foo
              </Col>
              <Col xs={6} md={4}>
                bar
              </Col>
            </Row>
          </Container>
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

我们有一个带有`Row`和`Col`的`Container`组件，以我们的方式显示布局。

与任何其他内容一样，我们可以像在`Col` s 中那样根据断点设置宽度。

我们将列大小设置为从 1 到 12，分别是最窄到最宽。

![](img/40f84c2b212c62373efa71b617396569.png)

照片由[阿丽亚娜·卡明斯基](https://unsplash.com/@arianka?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以在模态中禁用动画。

此外，我们可以使它们垂直居中。

布局网格也可以在模态内部。