# 材料界面—文本字段定制

> 原文：<https://blog.devgenius.io/material-ui-text-field-customization-28d53819c549?source=collection_archive---------0----------------------->

![](img/a243b8cd43b2e1be372debbba95a2c0a.png)

安娜·佩尔泽在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

材质 UI 是一个为 React 制作的材质设计库。

这是一组具有材质设计风格的 React 组件。

在这篇文章中，我们将看看如何用材质 UI 添加文本字段。

# 大小

我们可以用`size`属性改变文本字段的大小。

例如，我们可以写:

```
import React from "react";
import TextField from "[@material](http://twitter.com/material)-ui/core/TextField";export default function App() {
  return (
    <div className="App">
      <TextField label="Size" id="small" defaultValue="Small" size="small" />
    </div>
  );
}
```

添加大小为`small`的文本字段。

# 布局

我们可以按照自己的方式来布局文本字段。

为此，我们可以写:

```
import React from "react";
import TextField from "[@material](http://twitter.com/material)-ui/core/TextField";
import { makeStyles } from "[@material](http://twitter.com/material)-ui/core/styles";const useStyles = makeStyles(theme => ({
  root: {
    display: "flex",
    flexWrap: "wrap"
  },
  textField: {
    marginLeft: theme.spacing(1),
    marginRight: theme.spacing(1),
    width: "25ch"
  }
}));export default function App() {
  const classes = useStyles();return (
    <>
      <div>
        <TextField
          id="filled-full-width"
          label="Label"
          style={{ margin: 8 }}
          placeholder="Placeholder"
          helperText="Full width!"
          fullWidth
          margin="normal"
          InputLabelProps={{
            shrink: true
          }}
          variant="filled"
        />
        <TextField
          label="None"
          id="filled-margin-none"
          defaultValue="foo"
          className={classes.textField}
          helperText="foo"
        />
        <TextField
          label="Dense"
          style={{ margin: 3 }}
          id="filled-margin-dense"
          defaultValue="bar"
          className={classes.textField}
          helperText="bar"
          margin="dense"
        />
      </div>
    </>
  );
}
```

添加文本字段。

我们在顶部有一个全角文本字段，在底部有两个较短的文本字段。

他们有通常的道具，其中有`helperText`的提示。

`margin`有余量。

我们也有一些改变页边空白的样式。

`className`有阶级名称。

`placeholder`为文本字段准备占位符。

`variant`可填可不填。

`margin`可以比默认的`dense`小。

# 非受控与受控

文本字段可以不受控制，这意味着它没有附加任何状态。

或者它可以被控制，这意味着它具有附加的状态。

要制作一个受控的文本字段，我们可以将`value`和`onChange`道具添加到`TextField`中。

例如，我们可以写:

```
import React from "react";
import TextField from "[@material](http://twitter.com/material)-ui/core/TextField";export default function App() {
  const [name, setName] = React.useState("james");
  const handleChange = event => {
    setName(event.target.value);
  };
  return (
    <>
      <div>
        <TextField
          id="standard-name"
          label="Name"
          value={name}
          onChange={handleChange}
        />
      </div>
    </>
  );
}
```

我们有传递给`value` prop 的`name`状态。

以及传递给`onChange` prop 的`handleChange`函数。

然后当我们输入一些东西时，`handleChange`函数将运行并调用`setName`来更新`name`状态。

# 成分

一个`TextField`由更小的部件组成。

它们包括`FormControl`、`Input`、`FilledInput`、`InputLabel`、`OutlinedInput`和`FormHelperText`。

例如，我们可以将`TextField`替换为:

```
import React from "react";
import FormControl from "@material-ui/core/FormControl";
import Input from "@material-ui/core/Input";
import InputLabel from "@material-ui/core/InputLabel";
export default function App() {
  const [name, setName] = React.useState("james");
  const handleChange = event => {
    setName(event.target.value);
  };
  return (
    <>
      <div>
        <FormControl>
          <InputLabel htmlFor="input">Name</InputLabel>
          <Input id="input" value={name} onChange={handleChange} />
        </FormControl>
      </div>
    </>
  );
}
```

我们有一个用于输入框的`Input`和一个用于输入框标签的`InputLabel`。

要添加提示文本，我们可以使用`FormHelperText`组件:

```
import React from "react";
import FormControl from "[@material](http://twitter.com/material)-ui/core/FormControl";
import Input from "[@material](http://twitter.com/material)-ui/core/Input";
import InputLabel from "[@material](http://twitter.com/material)-ui/core/InputLabel";
import FormHelperText from "[@material](http://twitter.com/material)-ui/core/FormHelperText";export default function App() {
  const [name, setName] = React.useState("james");
  const handleChange = event => {
    setName(event.target.value);
  };
  return (
    <>
      <div>
        <FormControl>
          <InputLabel htmlFor="input">Name</InputLabel>
          <Input id="input" value={name} onChange={handleChange} />
          <FormHelperText>enter your name</FormHelperText>
        </FormControl>
      </div>
    </>
  );
}
```

我们也可以使用`OutlinedInput`和`FilledInput`组件来添加这些种类的输入框:

```
import React from "react";
import FormControl from "[@material](http://twitter.com/material)-ui/core/FormControl";
import OutlinedInput from "[@material](http://twitter.com/material)-ui/core/OutlinedInput";
import InputLabel from "[@material](http://twitter.com/material)-ui/core/InputLabel";
import FormHelperText from "[@material](http://twitter.com/material)-ui/core/FormHelperText";export default function App() {
  const [name, setName] = React.useState("james");
  const handleChange = event => {
    setName(event.target.value);
  };
  return (
    <>
      <div>
        <FormControl>
          <InputLabel htmlFor="input">Name</InputLabel>
          <OutlinedInput
            id="input"
            value={name}
            onChange={handleChange}
            label="Name"
          />
          <FormHelperText>enter your name</FormHelperText>
        </FormControl>
      </div>
    </>
  );
}
```

向框中添加带有轮廓的输入框。

我们可以使用`FilledInput`来添加一个带有背景的输入框:

```
import React from "react";
import FormControl from "[@material](http://twitter.com/material)-ui/core/FormControl";
import FilledInput from "[@material](http://twitter.com/material)-ui/core/FilledInput";
import InputLabel from "[@material](http://twitter.com/material)-ui/core/InputLabel";
import FormHelperText from "[@material](http://twitter.com/material)-ui/core/FormHelperText";export default function App() {
  const [name, setName] = React.useState("james");
  const handleChange = event => {
    setName(event.target.value);
  };
  return (
    <>
      <div>
        <FormControl>
          <InputLabel htmlFor="input">Name</InputLabel>
          <FilledInput id="input" value={name} onChange={handleChange} />
          <FormHelperText>enter your name</FormHelperText>
        </FormControl>
      </div>
    </>
  );
}
```

![](img/7db51986f3ac2f277dff71f4de0fff6d.png)

照片由[丹金](https://unsplash.com/@danielcgold?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

一个`TextField`由许多表单组件组成。

因此，我们可以添加那些而不是一个`TextField`。

我们还可以创建一个文本字段和控制器输入。