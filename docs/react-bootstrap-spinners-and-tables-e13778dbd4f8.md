# React 引导程序—微调器和表格

> 原文：<https://blog.devgenius.io/react-bootstrap-spinners-and-tables-e13778dbd4f8?source=collection_archive---------7----------------------->

![](img/161b851425d901bb6c3be98e0bf84e14.png)

大卫·坎特利在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

React Bootstrap 是为 React 开发的 Bootstrap 的一个版本。

这是一组具有引导风格的 React 组件。

在本文中，我们将了解如何使用 React Bootstrap 添加微调器。

# 纺纱机

微调器用于显示数据的加载状态。

要使用它，我们可以使用`Spinner`组件:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import Spinner from "react-bootstrap/Spinner";export default function App() {
  return (
    <>
      <Spinner animation="border" role="status">
        <span className="sr-only">Loading...</span>
      </Spinner>
    </>
  );
}
```

我们添加了带有`animation`道具的`Spinner`。

`border`意味着我们有一个作为旋转器旋转的圆。

我们也可以用`sr-only`类为屏幕阅读器添加文本。

它将被屏幕阅读器读取，但用户看不到它们。

# 动画片

我们也可以将`animation`道具设置为`grow`。

然后我们会得到一个悸动的旋转器:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import Spinner from "react-bootstrap/Spinner";export default function App() {
  return (
    <>
      <Spinner animation="grow" role="status">
        <span className="sr-only">Loading...</span>
      </Spinner>
    </>
  );
}
```

# 变体

纺纱工有各种不同的造型。

所有常见的自举都有。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import Spinner from "react-bootstrap/Spinner";export default function App() {
  return (
    <>
      <Spinner animation="border" variant="primary" />
      <Spinner animation="border" variant="secondary" />
      <Spinner animation="border" variant="success" />
      <Spinner animation="border" variant="danger" />
      <Spinner animation="border" variant="warning" />
      <Spinner animation="border" variant="info" />
      <Spinner animation="border" variant="light" />
      <Spinner animation="border" variant="dark" />
      <Spinner animation="grow" variant="primary" />
      <Spinner animation="grow" variant="secondary" />
      <Spinner animation="grow" variant="success" />
      <Spinner animation="grow" variant="danger" />
      <Spinner animation="grow" variant="warning" />
      <Spinner animation="grow" variant="info" />
      <Spinner animation="grow" variant="light" />
      <Spinner animation="grow" variant="dark" />
    </>
  );
}
```

然后我们得到不同颜色和动画风格的旋转器。

# 大小

我们也可以改变旋转器的大小，

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import Spinner from "react-bootstrap/Spinner";export default function App() {
  return (
    <>
      <Spinner animation="border" size="sm" />
      <Spinner animation="border" />
      <Spinner animation="grow" size="sm" />
      <Spinner animation="grow" />
    </>
  );
}
```

用`size`支柱设定尺寸。

`sm`表示比正常尺寸小。

我们也可以写`lg`让它们比正常的大。

# 小跟班

我们可以把旋转器放在纽扣里。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import Spinner from "react-bootstrap/Spinner";
import Button from "react-bootstrap/Button";export default function App() {
  return (
    <>
      <Button variant="success" disabled>
        <Spinner as="span" animation="border" size="sm" />
        <span className="sr-only">Loading...</span>
      </Button>{" "}
      <Button variant="success" disabled>
        <Spinner as="span" animation="grow" size="sm" />
        Loading...
      </Button>
    </>
  );
}
```

创建内部带有`Spinner`的按钮。

我们把它们变小，这样它们的大小就和按钮文本一样了。

# 桌子

我们可以用 React Bootstrap 添加表。

它附带了`Table`组件，让我们可以创建简单的表格:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import Table from "react-bootstrap/Table";export default function App() {
  return (
    <>
      <Table striped bordered hover>
        <thead>
          <tr>
            <th>ID</th>
            <th>First Name</th>
            <th>Last Name</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>1</td>
            <td>james</td>
            <td>smith</td>
          </tr>
          <tr>
            <td>2</td>
            <td>mary</td>
            <td>jones</td>
          </tr>
          <tr>
            <td>3</td>
            <td colSpan="2">larry</td>
          </tr>
        </tbody>
      </Table>
    </>
  );
}
```

我们只是将`Table`放在常规的表格元素周围来创建一个表格。

`striped`使各行颜色交替。

`bordered`给行添加边框。

`hover`高亮显示该行时改变其颜色。

# 小桌子

我们可以设置`size`道具来改变桌子的大小。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import Table from "react-bootstrap/Table";export default function App() {
  return (
    <>
      <Table striped bordered hover size="sm">
        <thead>
          <tr>
            <th>ID</th>
            <th>First Name</th>
            <th>Last Name</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>1</td>
            <td>james</td>
            <td>smith</td>
          </tr>
          <tr>
            <td>2</td>
            <td>mary</td>
            <td>jones</td>
          </tr>
          <tr>
            <td>3</td>
            <td colSpan="2">larry</td>
          </tr>
        </tbody>
      </Table>
    </>
  );
}
```

那么行就比平时小。

# 黑暗的桌子

我们可以将`variant`道具设置为`dark`使其变暗:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import Table from "react-bootstrap/Table";export default function App() {
  return (
    <>
      <Table striped bordered hover variant="dark">
        <thead>
          <tr>
            <th>ID</th>
            <th>First Name</th>
            <th>Last Name</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>1</td>
            <td>james</td>
            <td>smith</td>
          </tr>
          <tr>
            <td>2</td>
            <td>mary</td>
            <td>jones</td>
          </tr>
          <tr>
            <td>3</td>
            <td colSpan="2">larry</td>
          </tr>
        </tbody>
      </Table>
    </>
  );
}
```

现在所有的行都是暗的。

![](img/9d9e5e882c3abcb222b146511f756d65.png)

照片由[思想目录](https://unsplash.com/@thoughtcatalog?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以添加微调器来显示正在加载的内容。

可以用`Table`组件添加表格。