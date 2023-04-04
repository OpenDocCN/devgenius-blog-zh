# 反应陷阱和警报入门

> 原文：<https://blog.devgenius.io/getting-started-with-reactstrap-and-alerts-f9629808cd63?source=collection_archive---------2----------------------->

![](img/2f964c3b12377e35bc9173dea2d760ee.png)

克里斯托弗·卡森在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Reactstrap 是为 React 制作的一个版本引导程序。

这是一组具有 Boostrap 陷阱样式的 React 组件。

在本文中，我们将了解如何开始使用 Reactstrap 并显示警报。

# 入门指南

我们可以通过运行以下命令来安装 Reactstrap:

```
npm install --save reactstrap
```

然后我们可以导入我们需要的组件。

我们还需要安装 Bootstrap，因为不包括 Bootstrap CSS。

所以我们可以写:

```
npm install --save bootstrap
```

然后我们可以通过编写以下内容来导入它:

```
import 'bootstrap/dist/css/bootstrap.min.css';
```

# 警报

我们可以用`Alert`组件添加警报。

例如，我们可以写:

```
import React from "react";
import { Alert } from "reactstrap";
import "bootstrap/dist/css/bootstrap.min.css";export default function App() {
  return (
    <div className="App">
      <Alert color="primary">
        <a href="#" className="alert-link">
          link
        </a>{" "}
        message
      </Alert>
      <Alert color="secondary">
        <a href="#" className="alert-link">
          link
        </a>{" "}
        message
      </Alert>
      <Alert color="success">
        <a href="#" className="alert-link">
          link
        </a>{" "}
        message
      </Alert>
      <Alert color="danger">
        <a href="#" className="alert-link">
          link
        </a>{" "}
        message
      </Alert>
      <Alert color="warning">
        <a href="#" className="alert-link">
          link
        </a>{" "}
        message
      </Alert>
      <Alert color="info">
        <a href="#" className="alert-link">
          link
        </a>{" "}
        message
      </Alert>
      <Alert color="light">
        <a href="#" className="alert-link">
          link
        </a>{" "}
        message
      </Alert>
      <Alert color="dark">
        <a href="#" className="alert-link">
          link
        </a>{" "}
        message
      </Alert>
    </div>
  );
}
```

我们有带`color`道具的`Alert`组件来设置颜色。

`className`有链接的颜色。

# 警报的附加内容

我们可以给提醒添加额外的内容。

例如，我们可以写:

```
import React from "react";
import { Alert } from "reactstrap";
import "bootstrap/dist/css/bootstrap.min.css";export default function App() {
  return (
    <div className="App">
      <Alert color="primary">
        <h4 className="alert-heading">title</h4>
        <p>
          Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec ut arcu
          lacinia, condimentum sapien et, molestie eros. Nulla a molestie ante,
          vitae scelerisque odio. Integer ipsum quam, viverra quis ipsum
          dapibus,
        </p>
        <hr />
        <p className="mb-0">
          Fusce vitae eros varius, consequat mi at, consequat urna. Sed egestas
          nisl quis magna rutrum tincidunt ac eu tortor.
        </p>
      </Alert>
    </div>
  );
}
```

我们在警报中增加了 p 和 hr 元素。

`mb-0`删除下边距。

# 解除警报

我们可以通过使警报成为受控组件来使其不可接受。

例如，我们可以写:

```
import React from "react";
import { Alert } from "reactstrap";
import "bootstrap/dist/css/bootstrap.min.css";export default function App() {
  const [visible, setVisible] = React.useState(true); const onDismiss = () => setVisible(false); return (
    <div className="App">
      <Alert color="info" isOpen={visible} toggle={onDismiss}>
        Lorem ipsum dolor sit amet, consectetur adipiscing elit.
      </Alert>
    </div>
  );
}
```

我们有`visible`状态来打开或关闭警报。

它被传入`isOpen`道具来触发警报。

`toggle`具有`onDismiss`功能，将`visible`设置为`false`，使警报消失。

显示关闭按钮，我们可以点击它来调用`onDismiss`。

# 不受控制的警报

我们可以发出无关紧要的警告，而不会让它们失控。

为此，我们可以导入`UncontrolledAlert`组件。

例如，我们可以写:

```
import React from "react";
import { UncontrolledAlert } from "reactstrap";
import "bootstrap/dist/css/bootstrap.min.css";export default function App() {
  return (
    <div className="App">
      <UncontrolledAlert color="info">
        Lorem ipsum dolor sit amet, consectetur adipiscing elit.
      </UncontrolledAlert>
    </div>
  );
}
```

现在我们不需要以前的状态改变逻辑。

# 无渐变警报

默认情况下，提醒具有渐变过渡效果。

要禁用渐变效果，我们可以将`fade`道具设置为`false`:

```
import React from "react";
import { Alert } from "reactstrap";
import "bootstrap/dist/css/bootstrap.min.css";export default function App() {
  const [visible, setVisible] = React.useState(true); const onDismiss = () => setVisible(false); return (
    <div className="App">
      <Alert color="primary" isOpen={visible} toggle={onDismiss} fade={false}>
        Lorem ipsum dolor sit amet, consectetur adipiscing elit.
      </Alert>
    </div>
  );
}
```

我们在受控警报中添加了道具。

此外，我们还可以对不受控制的警报进行同样的处理:

```
import React from "react";
import { UncontrolledAlert } from "reactstrap";
import "bootstrap/dist/css/bootstrap.min.css";export default function App() {
  return (
    <div className="App">
      <UncontrolledAlert color="info" fade={false}>
        Lorem ipsum dolor sit amet, consectetur adipiscing elit.
      </UncontrolledAlert>
    </div>
  );
}
```

![](img/588034b7dc338dc9b61133ab1f502176.png)

肯尼斯·席佩尔·维拉在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以安装 Reactstrap 和 Bootstrap 包，将 Reactstrap 添加到我们的应用程序中。

此外，我们可以使用警报来显示通知。