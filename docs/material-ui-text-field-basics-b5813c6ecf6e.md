# 材料用户界面-文本字段基础

> 原文：<https://blog.devgenius.io/material-ui-text-field-basics-b5813c6ecf6e?source=collection_archive---------0----------------------->

![](img/432479fdf75bb42c957caaadf293b828.png)

乔纳森·法伯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

材质 UI 是一个为 React 制作的材质设计库。

这是一组具有材质设计风格的 React 组件。

在这篇文章中，我们将看看如何用材质 UI 添加文本字段。

# 文本字段

文本字段是允许用户输入和编辑文本的表单控件。

为了添加它们，我们使用了`TextField`组件。

例如，我们可以写:

```
import React from "react";
import TextField from "[@material](http://twitter.com/material)-ui/core/TextField";export default function App() {
  return (
    <div className="App">
      <TextField label="Standard" />
    </div>
  );
}
```

添加基本文本字段。

`label`显示为占位符。

我们还可以添加`variant`道具来改变造型。

例如，我们可以写:

```
import React from "react";
import TextField from "[@material](http://twitter.com/material)-ui/core/TextField";export default function App() {
  return (
    <div className="App">
      <TextField label="filled" variant="filled" />
    </div>
  );
}
```

添加带有背景色的文本字段。

# 形式道具

我们可以添加`required`道具，使字段成为必填字段。

`disabled`禁用文本字段。

`type`有输入类型。

让我们向用户显示提示。

例如，我们可以写:

```
import React from "react";
import TextField from "[@material](http://twitter.com/material)-ui/core/TextField";export default function App() {
  return (
    <div className="App">
      <TextField label="required" required helperText="required field" />
    </div>
  );
}
```

向我们的字段添加帮助文本，它将显示在底部。

`required`在标签上添加一个星号，表示它是必需的。

# 确认

`error`道具让我们切换错误状态。

例如，我们可以写:

```
import React from "react";
import TextField from "[@material](http://twitter.com/material)-ui/core/TextField";export default function App() {
  return (
    <div className="App">
      <TextField label="error" error />
    </div>
  );
}
```

显示有错误的文本字段。

它会是红色的。

# 挑选

`select`属性使文本字段在内部使用`Select`组件。

例如，我们可以写:

```
import React from "react";
import TextField from "[@material](http://twitter.com/material)-ui/core/TextField";
import MenuItem from "[@material](http://twitter.com/material)-ui/core/MenuItem";const fruits = [
  { value: "apple", label: "Apple" },
  { value: "orange", label: "Orange" },
  { value: "grape", label: "Grape" }
];export default function App() {
  const [fruit, setFruit] = React.useState("apple"); const handleChange = event => {
    setFruit(event.target.value);
  }; return (
    <div className="App">
      <TextField
        select
        label="Select"
        value={fruit}
        onChange={handleChange}
        helperText="Please select your fruit"
      >
        {fruits.map(option => (
          <MenuItem key={option.value} value={option.value}>
            {option.label}
          </MenuItem>
        ))}
      </TextField>
    </div>
  );
}
```

我们用`TextField`组件创建一个下拉列表。

`label`有标签。

`value`是选择值。

`onChange`当我们改变选择时运行，我们可以用它来设置选择值状态。

在`TextField`中，我们有`MenuItem`组件来显示选项。

# 核标准情报中心

我们可以给我们的`TextFields`添加图标。

例如，我们可以写:

```
import React from "react";
import TextField from "[@material](http://twitter.com/material)-ui/core/TextField";
import Grid from "[@material](http://twitter.com/material)-ui/core/Grid";
import AccountCircle from "[@material](http://twitter.com/material)-ui/icons/AccountCircle";export default function App() {
  return (
    <div className="App">
      <Grid container spacing={1} alignItems="flex-end">
        <Grid item>
          <AccountCircle />
        </Grid>
        <Grid item>
          <TextField id="input-with-icon-grid" label="input" />
        </Grid>
      </Grid>
    </div>
  );
}
```

在图标旁边添加文本字段。

`AccountCircle`是图标。

`TextField`在它的右边。

# 输入装饰

我们可以使用`InputAdornment`组件为输入添加前缀、后缀或动作。

例如，我们可以写:

```
import React from "react";
import clsx from "clsx";
import InputAdornment from "[@material](http://twitter.com/material)-ui/core/InputAdornment";
import TextField from "[@material](http://twitter.com/material)-ui/core/TextField";
import { makeStyles } from "[@material](http://twitter.com/material)-ui/core/styles";const useStyles = makeStyles(theme => ({
  margin: {
    margin: theme.spacing(1)
  },
  textField: {
    width: "25ch"
  }
}));export default function App() {
  const classes = useStyles(); return (
    <div className="App">
      <TextField
        label="length"
        id="filled-start-adornment"
        className={clsx(classes.margin, classes.textField)}
        InputProps={{
          startAdornment: (
            <InputAdornment position="start">meters</InputAdornment>
          )
        }}
      />
    </div>
  );
}
```

我们用一个`InputAdornment`加上一个`TextField`。

`position`设置为`start`,这意味着我们为输入添加一个前缀。

它会显示在左边。

我们有一个类在前缀和输入框之间添加一些空白。

![](img/ae7e43b358268e83892efa260c0d02fd.png)

由[奥拉·米先科](https://unsplash.com/@olamishchenko?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以添加文本字段让用户输入文本。

有许多选项我们可以改变它的风格。

此外，我们可以在输入框旁边添加图标和文本。

也有错误的样式。