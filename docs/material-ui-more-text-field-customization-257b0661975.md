# 材料用户界面-更多文本字段定制

> 原文：<https://blog.devgenius.io/material-ui-more-text-field-customization-257b0661975?source=collection_archive---------0----------------------->

![](img/0441d0ed8c286642c0c5ab0134e7a82a.png)

Mia Cambriello 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

材质 UI 是一个为 React 制作的材质设计库。

这是一组具有材质设计风格的 React 组件。

在本文中，我们将看看如何使用材质 UI 定制文本字段。

# 输入

我们可以在没有其他部分的情况下添加输入框。

例如，我们可以写:

```
import React from "react";
import Input from "[@material](http://twitter.com/material)-ui/core/Input";export default function App() {
  return (
    <>
      <div>
        <Input
          placeholder="Placeholder"
          inputProps={{ "aria-label": "description" }}
        />
      </div>
    </>
  );
}
```

添加一个`Input`组件而不添加所有其他组件。

# 颜色

要改变一个`TextField`的颜色，我们可以设置`color`道具。

例如，我们可以写:

```
import React from "react";
import TextField from "[@material](http://twitter.com/material)-ui/core/TextField";export default function App() {
  return (
    <>
      <div>
        <TextField
          id="filled-primary"
          label="Filled primary"
          variant="filled"
          color="primary"
        />
      </div>
    </>
  );
}
```

我们将`color`道具设置为`primary`来设置颜色。

# 自定义文本字段

我们可以通过传递我们自己的样式来定制一个文本字段。

`TextField`有一个`InputProps`道具，带一个有属性的物体。

例如，我们可以写:

```
import React from "react";
import TextField from "[@material](http://twitter.com/material)-ui/core/TextField";
import { fade, makeStyles } from "[@material](http://twitter.com/material)-ui/core/styles";const useStyles = makeStyles(theme => ({
  root: {
    border: "1px solid brown",
    overflow: "hidden",
    borderRadius: 3,
    backgroundColor: "orange",
    transition: theme.transitions.create(["border-color", "box-shadow"]),
    "&:hover": {
      backgroundColor: "#fff"
    },
    "&$focused": {
      backgroundColor: "#fff",
      boxShadow: `${fade(theme.palette.primary.main, 0.25)} 0 0 0 2px`,
      borderColor: theme.palette.primary.main
    }
  },
  focused: {}
}));function StyledTextField(props) {
  const classes = useStyles(); return (
    <TextField InputProps={{ classes, disableUnderline: true }} {...props} />
  );
}export default function App() {
  return (
    <>
      <div>
        <StyledTextField
          id="filled-primary"
          label="Filled primary"
          variant="filled"
          color="primary"
        />
      </div>
    </>
  );
}
```

`StyledTextField`得到所有的道具并把它们传进去。

我们用`makeStyle`函数制作我们的样式。

我们将`root`设计成我们想要的所有样式。

`root`将是文本字段，因为我们从返回的`useStyles`钩子向文本字段传递了`classes`。

我们设置了`border`、`backgroundColor`，阴影还有更多。

`transition`让我们添加过渡效果。

# 与第三方输入库集成

我们可以通过第三方库向我们的输入添加输入掩码之类的东西。

例如，我们可以使用 react-text-mask 库向我们的`Input`组件添加一个输入掩码。

我们可以写:

```
import React from "react";
import Input from "[@material](http://twitter.com/material)-ui/core/Input";
import MaskedInput from "react-text-mask";function TextMask(props) {
  const { inputRef, ...other } = props; return (
    <MaskedInput
      {...other}
      ref={ref => {
        inputRef(ref ? ref.inputElement : null);
      }}
      mask={[
        /\d/,
        /\d/,
        /\d/,
        "-",
        /\d/,
        /\d/,
        /\d/,
        "-",
        /\d/,
        /\d/,
        /\d/,
        /\d/
      ]}
      placeholderChar={"\u2000"}
      showMask
    />
  );
}export default function App() {
  const [values, setValues] = React.useState("");
  const handleChange = event => {
    setValues(event.target.value);
  }; return (
    <>
      <div>
        <Input
          value={values.textmask}
          onChange={handleChange}
          name="textmask"
          id="formatted-text-mask-input"
          inputComponent={TextMask}
        />
      </div>
    </>
  );
}
```

我们添加一个`TextMask`组件，它使用带有我们想要添加的掩码的`MaskedInput`。

掩码会将格式限制为我们想要的格式。

`showMask`显示面具

`mask`有格式。

`placeholderChar`有一个占位符。

我们还将`ref`设置为从道具传入的`inputRef`。

在我们的`App`组件中，我们将`TextMask`传递给我们的`Input`的`inputComponent`道具。

我们将看到显示的输入掩码。

它使用的其他库包括 react-number-format 库，用于将数字输入限制为固定格式。

# 易接近

输入应该链接到标签和帮助文本，以便文本字段是可访问的。

例如，我们写道:

```
import React from "react";
import FormControl from "[@material](http://twitter.com/material)-ui/core/FormControl";
import InputLabel from "[@material](http://twitter.com/material)-ui/core/InputLabel";
import Input from "[@material](http://twitter.com/material)-ui/core/Input";
import FormHelperText from "[@material](http://twitter.com/material)-ui/core/FormHelperText";export default function App() {
  return (
    <>
      <div>
        <FormControl>
          <InputLabel htmlFor="name-input">name</InputLabel>
          <Input id="name-input" aria-describedby="name-helper-text" />
          <FormHelperText id="name-helper-text">
            enter your name.
          </FormHelperText>
        </FormControl>
      </div>
    </>
  );
}
```

让它变得触手可及。

我们有与输入的`id`相匹配的`html-for`道具。

`FormHelperText`的`id`与`Input`的`aria-describedby`相匹配。

这样，它们就可以相互联系起来。

![](img/0cc8074415002260c309c40e3533f4f4.png)

照片由 [Alina Karpenko](https://unsplash.com/@alinasagirova?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

可以使用第三方库自定义材料 UI 文本字段。

此外，我们可以通过在组件中添加额外的道具来使它们变得可访问。