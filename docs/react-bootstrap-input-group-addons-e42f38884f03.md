# 反应引导—输入组插件

> 原文：<https://blog.devgenius.io/react-bootstrap-input-group-addons-e42f38884f03?source=collection_archive---------3----------------------->

![](img/b0f5ddaada9250c19ba24c51d43f1679.png)

马丁·莫雷诺在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

React Bootstrap 是为 React 开发的 Bootstrap 的一个版本。

这是一组具有引导风格的 React 组件。

在本文中，我们将了解如何使用 React Bootstrap 向 React 应用程序添加输入组。

# 输入组复选框和单选按钮

我们可以在输入组中添加复选框和单选按钮。

为此，我们可以分别使用`InputGrou.Checkbox`和`InputGroup.Radio`组件。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import InputGroup from "react-bootstrap/InputGroup";
import FormControl from "react-bootstrap/FormControl";export default function App() {
  return (
    <div>
      <InputGroup className="mb-3">
        <InputGroup.Prepend>
          <InputGroup.Checkbox/>
        </InputGroup.Prepend>
        <FormControl />
      </InputGroup>
      <InputGroup>
        <InputGroup.Prepend>
          <InputGroup.Radio />
        </InputGroup.Prepend>
        <FormControl />
      </InputGroup>
    </div>
  );
}
```

我们只是将它们添加到`InputGroup.Prepend`组件中，将它们添加到输入框的左侧。

# 多端输入

我们可以在一个输入组中有多个控件。

我们只是把它们都加到`InputGroup`里。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import InputGroup from "react-bootstrap/InputGroup";
import FormControl from "react-bootstrap/FormControl";export default function App() {
  return (
    <div>
      <InputGroup className="mb-3">
        <InputGroup.Prepend>
          <InputGroup.Text>name and age</InputGroup.Text>
        </InputGroup.Prepend>
        <FormControl />
        <FormControl />
      </InputGroup>
    </div>
  );
}
```

# 多个插件

此外，我们可以在输入框的任何一边有多个输入组插件。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import InputGroup from "react-bootstrap/InputGroup";
import FormControl from "react-bootstrap/FormControl";export default function App() {
  return (
    <div>
      <InputGroup className="mb-3">
        <InputGroup.Prepend>
          <InputGroup.Text>$</InputGroup.Text>
          <InputGroup.Text>US</InputGroup.Text>
          <InputGroup.Text>0.00</InputGroup.Text>
        </InputGroup.Prepend>
        <FormControl placeholder="amount" />
      </InputGroup>
      <InputGroup className="mb-3">
        <FormControl placeholder="amount" />
        <InputGroup.Append>
          <InputGroup.Text>$</InputGroup.Text>
          <InputGroup.Text>US</InputGroup.Text>
          <InputGroup.Text>0.00</InputGroup.Text>
        </InputGroup.Append>
      </InputGroup>
    </div>
  );
}
```

在第一个输入组的左边添加 3 个插件。

我们在第二个输入组的右边添加了 3 个插件。

# 按钮插件

我们可以将按钮作为附件。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import InputGroup from "react-bootstrap/InputGroup";
import FormControl from "react-bootstrap/FormControl";
import Button from "react-bootstrap/Button";export default function App() {
  return (
    <div>
      <InputGroup className="mb-3">
        <InputGroup.Prepend>
          <Button variant="outline-primary">click me</Button>
        </InputGroup.Prepend>
        <FormControl />
      </InputGroup> <InputGroup className="mb-3">
        <FormControl placeholder="username" />
        <InputGroup.Append>
          <Button variant="outline-primary">click me</Button>
        </InputGroup.Append>
      </InputGroup> <InputGroup className="mb-3">
        <InputGroup.Prepend>
          <Button variant="outline-primary">click me</Button>
          <Button variant="outline-primary">click me</Button>
        </InputGroup.Prepend>
        <FormControl />
      </InputGroup> <InputGroup>
        <FormControl placeholder="username" />
        <InputGroup.Append>
          <Button variant="outline-primary">click me</Button>
          <Button variant="outline-primary">click me</Button>
        </InputGroup.Append>
      </InputGroup>
    </div>
  );
}
```

我们在`InputGroup.Prepend`和`InputGroup.Append`组件里有按钮，分别把它们添加到输入框的左边和右边。

# 带有下拉菜单的按钮

我们也可以添加下拉按钮作为输入插件。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import InputGroup from "react-bootstrap/InputGroup";
import FormControl from "react-bootstrap/FormControl";
import Dropdown from "react-bootstrap/Dropdown";
import DropdownButton from "react-bootstrap/DropdownButton";export default function App() {
  return (
    <div>
      <InputGroup className="mb-3">
        <DropdownButton
          as={InputGroup.Prepend}
          variant="outline-primary"
          title="Dropdown"
        >
          <Dropdown.Item href="#">foo</Dropdown.Item>
          <Dropdown.Item href="#">bar</Dropdown.Item>
          <Dropdown.Divider />
          <Dropdown.Item href="#">baz</Dropdown.Item>
        </DropdownButton>
        <FormControl />
      </InputGroup> <InputGroup>
        <FormControl placeholder="username" /> <DropdownButton
          as={InputGroup.Prepend}
          variant="outline-primary"
          title="Dropdown"
        >
          <Dropdown.Item href="#">foo</Dropdown.Item>
          <Dropdown.Item href="#">bar</Dropdown.Item>
          <Dropdown.Divider />
          <Dropdown.Item href="#">baz</Dropdown.Item>
        </DropdownButton>
      </InputGroup>
    </div>
  );
}
```

我们使用`DropdownButton`和设置为`InmputGeoup.Prepend`的`as`属性，这样它就适合输入组。

同样，我们设置`variant`来改变按钮的样式。

![](img/4a974fbedc03cb82bf6ec8b4554f812f.png)

由 [Behzad Ghaffarian](https://unsplash.com/@behz?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

# 结论

我们可以在输入组中添加下拉菜单和按钮。

此外，我们可以在一个组中有多个输入组插件。