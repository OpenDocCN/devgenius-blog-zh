# 反应引导—表单控件

> 原文：<https://blog.devgenius.io/react-bootstrap-form-controls-f073ed4e8b75?source=collection_archive---------0----------------------->

![](img/e2cda627ec971955559e0f8a83dbcfa4.png)

杰克·卡塔拉诺在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

React Bootstrap 是为 React 开发的 Bootstrap 的一个版本。

这是一组具有引导风格的 React 组件。

在本文中，我们将了解如何使用 React Bootstrap 向 React 应用程序添加表单。

# 形式

使用 React Bootstrap 构建表单有许多组件。

例如，我们可以写:

```
import React from "react";
import Form from "react-bootstrap/Form";
import Button from "react-bootstrap/Button";
import "bootstrap/dist/css/bootstrap.min.css";export default function App() {
  return (
    <>
      <Form>
        <Form.Group>
          <Form.Label>Email</Form.Label>
          <Form.Control type="email" placeholder="Enter email" />
          <Form.Text className="text-muted">Please type email</Form.Text>
        </Form.Group> <Form.Group>
          <Form.Label>Name</Form.Label>
          <Form.Control type="text" placeholder="Name" />
        </Form.Group>
        <Form.Group>
          <Form.Check type="checkbox" label="Check me" />
        </Form.Group>
        <Button variant="primary" type="submit">
          Submit
        </Button>
      </Form>
    </>
  );
}
```

我们添加一个内部带有`Form.Group`的`Form`组件来添加表单组。

在它里面，我们有`Form.Label`来添加一个表单控件标签。

我们有`Form.Control`来添加一个表单控件。

`Form.Text`让我们在表单控件下面添加更多的文本。

还有添加复选框的`Form.Check`复选框。

# 表单控件

React Bootstrap 附带了我们需要的所有常见的表单控件。

我们可以添加文本框、选择元素等等。

例如，我们可以写:

```
import React from "react";
import Form from "react-bootstrap/Form";
import "bootstrap/dist/css/bootstrap.min.css";export default function App() {
  return (
    <>
      <Form>
        <Form.Group controlId="email">
          <Form.Label>Email address</Form.Label>
          <Form.Control type="email" placeholder="name@example.com" />
        </Form.Group>
      </Form>
    </>
  );
}
```

添加文本输入框。

我们有一个表单组来将表单分组。

`Form.Label`有标签。

`Form.Control`有文本输入框。`type`被设置为`email`以将该值限制为电子邮件。

要添加一个选择下拉列表，我们可以写:

```
import React from "react";
import Form from "react-bootstrap/Form";
import "bootstrap/dist/css/bootstrap.min.css";export default function App() {
  return (
    <>
      <Form>
        <Form.Group controlId="select">
          <Form.Label>Example select</Form.Label>
          <Form.Control as="select">
            <option>foo</option>
            <option>bar</option>
            <option>baz</option>
          </Form.Control>
        </Form.Group>
      </Form>
    </>
  );
}
```

我们将`as`属性设置为`select`以将其呈现为下拉列表。

然后我们可以把我们的`option`元素放进去。

我们还可以添加`multiple`属性来启用 select 元素上的多重选择。

例如，我们可以写:

```
import React from "react";
import Form from "react-bootstrap/Form";
import "bootstrap/dist/css/bootstrap.min.css";export default function App() {
  return (
    <>
      <Form>
        <Form.Group controlId="select">
          <Form.Label>Example select</Form.Label>
          <Form.Control as="select" multiple>
            <option>foo</option>
            <option>bar</option>
            <option>baz</option>
          </Form.Control>
        </Form.Group>
      </Form>
    </>
  );
}
```

现在，我们可以从框中选择多个选项。

我们可以使用`Form.File`组件来添加一个文件输入。

例如，我们可以写:

```
import React from "react";
import Form from "react-bootstrap/Form";
import "bootstrap/dist/css/bootstrap.min.css";export default function App() {
  return (
    <>
      <Form>
        <Form.Group>
          <Form.File id="file-upload" label="upload a file" />
        </Form.Group>
      </Form>
    </>
  );
}
```

我们添加了`Form.File`上传来让我们上传一个文件。

# 胶料

我们可以改变表单控件的大小。

例如，我们可以写:

```
import React from "react";
import Form from "react-bootstrap/Form";
import "bootstrap/dist/css/bootstrap.min.css";export default function App() {
  return (
    <>
      <Form>
        <Form.Group>
          <Form.Control size="lg" type="text" placeholder="Large text" />
          <br />
          <Form.Control type="text" placeholder="Normal text" />
          <br />
          <Form.Control size="sm" type="text" placeholder="Small text" />
        </Form.Group>
      </Form>
    </>
  );
}
```

我们设置`size`道具来改变大小。

`lg`大。`sm`小。如果我们没有传入一个值，那么我们得到一个正常大小的控件。

我们可以对选择的元素做同样的事情。

例如，我们可以写:

```
import React from "react";
import Form from "react-bootstrap/Form";
import "bootstrap/dist/css/bootstrap.min.css";export default function App() {
  return (
    <>
      <Form>
        <Form.Group>
          <Form.Control as="select" size="lg">
            <option>Large select</option>
          </Form.Control>
          <br />
          <Form.Control as="select">
            <option>Default select</option>
          </Form.Control>
          <br />
          <Form.Control size="sm" as="select">
            <option>Small select</option>
          </Form.Control>
        </Form.Group>
      </Form>
    </>
  );
}
```

我们可以像处理文本输入一样改变它们的大小。

# 只读控件

我们可以添加`readOnly`属性使表单控件只读。

例如，我们可以写:

```
import React from "react";
import Form from "react-bootstrap/Form";
import "bootstrap/dist/css/bootstrap.min.css";export default function App() {
  return (
    <>
      <Form>
        <Form.Group>
          <Form.Control type="text" placeholder="Readonly input..." readOnly />
        </Form.Group>
      </Form>
    </>
  );
}
```

加上`readOnly`道具，我们就不能往箱子里进东西了。

![](img/a1ef359a6d7d199ba9ed6237c84fec25.png)

由[比约恩·安东尼森](https://unsplash.com/@bjornftw?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以用`Form.Control`组件添加输入框、下拉框和文件输入。