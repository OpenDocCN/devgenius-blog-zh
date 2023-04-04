# 使用 React 套件库(表单和复选框)开始 React 开发

> 原文：<https://blog.devgenius.io/getting-started-with-react-development-with-the-react-suite-library-forms-and-checkboxes-104c653b336d?source=collection_archive---------1----------------------->

![](img/eee21874a74b40aa5181eb50d2fdebbf.png)

照片由[希瑟·罗](https://unsplash.com/@llhheather?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

React Suite 是一个有用的 UI 库，让我们可以轻松地将许多组件添加到 React 应用程序中。

在本文中，我们将了解如何使用它向 React 应用程序添加组件。

# 形式

我们可以使用 React Suite 提供的组件将表单添加到 React 应用程序中。

例如，我们可以写:

```
import React from "react";
import {
  Form,
  FormGroup,
  FormControl,
  ControlLabel,
  HelpBlock,
  ButtonToolbar,
  Button
} from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div className="App">
      <Form>
        <FormGroup>
          <ControlLabel>Username</ControlLabel>
          <FormControl name="name" />
          <HelpBlock>Required</HelpBlock>
        </FormGroup>
        <FormGroup>
          <ControlLabel>Textarea</ControlLabel>
          <FormControl rows={5} name="textarea" componentClass="textarea" />
        </FormGroup>
        <FormGroup>
          <ButtonToolbar>
            <Button appearance="primary">Submit</Button>
            <Button appearance="default">Cancel</Button>
          </ButtonToolbar>
        </FormGroup>
      </Form>
    </div>
  );
}
```

`Form`组件呈现为一个`form`元素。

`FormGroup`让我们分组形成控制元素。

`ControlLabel`有标签。

`FormControl`拥有表单控件。

我们可以将`FormControl`渲染成各种控件。

`HelpBlock`有输入提示。

`ButtonToolbar`让我们添加一个按钮工具栏。

`ButtonGroup`让我们添加一个按钮组。

`componentClass`让我们设置元素来呈现表单控件。

我们可以将`layout`设置为`'horizontal'`来并排制作标签和表单控件:

```
import React from "react";
import {
  Form,
  FormGroup,
  FormControl,
  ControlLabel,
  HelpBlock,
  ButtonToolbar,
  Button
} from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div className="App">
      <Form layout="horizontal">
        <FormGroup>
          <ControlLabel>Username</ControlLabel>
          <FormControl name="name" />
          <HelpBlock>Required</HelpBlock>
        </FormGroup>
        <FormGroup>
          <ControlLabel>Textarea</ControlLabel>
          <FormControl rows={5} name="textarea" componentClass="textarea" />
        </FormGroup>
        <FormGroup>
          <ButtonToolbar>
            <Button appearance="primary">Submit</Button>
            <Button appearance="default">Cancel</Button>
          </ButtonToolbar>
        </FormGroup>
      </Form>
    </div>
  );
}
```

我们还可以添加内嵌布局，将`layout`设置为`'inline'`:

```
import React from "react";
import {
  Form,
  FormGroup,
  FormControl,
  ControlLabel,
  ButtonToolbar,
  Button
} from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div className="App">
      <Form layout="inline">
        <FormGroup>
          <ControlLabel>Username</ControlLabel>
          <FormControl name="name" />
        </FormGroup>
        <FormGroup>
          <ControlLabel>Textarea</ControlLabel>
          <FormControl rows={5} name="textarea" componentClass="textarea" />
        </FormGroup>
        <FormGroup>
          <ButtonToolbar>
            <Button appearance="primary">Submit</Button>
            <Button appearance="default">Cancel</Button>
          </ButtonToolbar>
        </FormGroup>
      </Form>
    </div>
  );
}
```

# 检验盒

我们可以添加一个带有`Checkbox`和`CheckboxGroup`组件的复选框。

例如，我们可以写:

```
import React from "react";
import { Checkbox } from "rsuite";import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div className="App">
      <Checkbox defaultChecked> Checked</Checkbox>
    </div>
  );
}
```

默认情况下，`defaultChecked`属性会使其被选中。

我们也可以用`disabled`道具禁用它。

我们可以用`indeterminate`道具让它接受一个不确定的状态。

我们可以从`onChange`处理程序中获得检查过的值。

例如，我们可以写:

```
import React, { useState } from "react";
import { Checkbox, CheckboxGroup } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  const [value, setValue] = useState([]);
  const handleChange = (value) => {
    console.log(value);
    setValue(value);
  }; return (
    <div className="App">
      <CheckboxGroup
        inline
        name="checkboxList"
        value={value}
        onChange={handleChange}
      >
        <Checkbox value="A">Item A</Checkbox>
        <Checkbox value="B">Item B</Checkbox>
        <Checkbox value="C">Item C</Checkbox>
        <Checkbox value="D">Item D</Checkbox>
      </CheckboxGroup>
    </div>
  );
}
```

从`value`数组参数中获取校验值。

我们调用`setValue`将检查值设置为`value`状态的值。

# 结论

使用 React Suite，我们可以轻松地将表单和复选框添加到 React 应用程序中。