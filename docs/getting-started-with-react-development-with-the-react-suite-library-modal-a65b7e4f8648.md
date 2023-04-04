# 使用 React 套件库(模态)开始 React 开发

> 原文：<https://blog.devgenius.io/getting-started-with-react-development-with-the-react-suite-library-modal-a65b7e4f8648?source=collection_archive---------3----------------------->

![](img/48af7bceab6d464c163f3e202f5d9c57.png)

[伊泰·维尔奇克](https://unsplash.com/@itayverchik?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

React Suite 是一个有用的 UI 库，让我们可以轻松地将许多组件添加到 React 应用程序中。

在本文中，我们将了解如何使用它向 React 应用程序添加组件。

# 情态的

我们可以用`Modal`组件在 React 应用程序中添加一个模态。

例如，我们可以写:

```
import React, { useState } from "react";
import { Modal, ButtonToolbar, Button, Paragraph } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  const [show, setShow] = useState();
  const open = () => {
    setShow(true);
  }; const close = () => {
    setShow(false);
  };
  return (
    <div>
      <ButtonToolbar>
        <Button onClick={open}> Open</Button>
      </ButtonToolbar>
      <Modal show={show} onHide={close}>
        <Modal.Header>
          <Modal.Title>Modal Title</Modal.Title>
        </Modal.Header>
        <Modal.Body>hello </Modal.Body>
        <Modal.Footer>
          <Button onClick={close} appearance="primary">
            Ok
          </Button>
          <Button onClick={close} appearance="subtle">
            Cancel
          </Button>
        </Modal.Footer>
      </Modal>
    </div>
  );
}
```

我们创建了`show`状态，用`open`函数将其设置为`true`，用`close`函数将其设置为`false`。

在`Modal`组件中，我们将`show`属性设置为`show`状态，以控制是否显示模态。

我们将`close`函数传递给所有按钮的`onClick`属性，以便在我们单击它们时关闭模态。

`Modal.Header`有表头。

`Modal.Title`有标题。

`Modal.Body`有身体。

`Modal.Footer`有模态页脚。

我们可以将`backdrop`道具设置为`false`来禁用背景:

```
import React, { useState } from "react";
import { Modal, ButtonToolbar, Button, Paragraph } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  const [show, setShow] = useState();
  const open = () => {
    setShow(true);
  }; const close = () => {
    setShow(false);
  };
  return (
    <div>
      <ButtonToolbar>
        <Button onClick={open}> Open</Button>
      </ButtonToolbar>
      <Modal show={show} onHide={close} backdrop={false}>
        <Modal.Header>
          <Modal.Title>Modal Title</Modal.Title>
        </Modal.Header>
        <Modal.Body>hello </Modal.Body>
        <Modal.Footer>
          <Button onClick={close} appearance="primary">
            Ok
          </Button>
          <Button onClick={close} appearance="subtle">
            Cancel
          </Button>
        </Modal.Footer>
      </Modal>
    </div>
  );
}
```

模态的大小可以用`size`支柱改变:

```
import React, { useState } from "react";
import { Modal, ButtonToolbar, Button, Paragraph } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  const [show, setShow] = useState();
  const open = () => {
    setShow(true);
  }; const close = () => {
    setShow(false);
  };
  return (
    <div>
      <ButtonToolbar>
        <Button onClick={open}> Open</Button>
      </ButtonToolbar>
      <Modal show={show} onHide={close} size="xs">
        <Modal.Header>
          <Modal.Title>Modal Title</Modal.Title>
        </Modal.Header>
        <Modal.Body>hello </Modal.Body>
        <Modal.Footer>
          <Button onClick={close} appearance="primary">
            Ok
          </Button>
          <Button onClick={close} appearance="subtle">
            Cancel
          </Button>
        </Modal.Footer>
      </Modal>
    </div>
  );
}
```

我们将其设置为`xs`以将其设置为最小尺寸。

其他选项包括`sm`用于小型，`md`用于中型，`lg`用于大型。

`full`道具会让它变大。

# 结论

我们可以使用 React Suite 在 React 应用程序中添加一个模型。