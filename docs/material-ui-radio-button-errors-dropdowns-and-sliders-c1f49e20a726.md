# 材料用户界面—单选按钮错误、下拉菜单和滑块

> 原文：<https://blog.devgenius.io/material-ui-radio-button-errors-dropdowns-and-sliders-c1f49e20a726?source=collection_archive---------0----------------------->

![](img/b1675a68272e2a941564e88f812cb4b4.png)

由 [Hermes Rivera](https://unsplash.com/@hermez777?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

材质 UI 是一个为 React 制作的材质设计库。

这是一组具有材质设计风格的 React 组件。

在这篇文章中，我们将看看如何添加单选按钮，下拉菜单，以及带有材质 UI 的滑块。

# 显示错误

为了显示单选按钮的错误，我们可以向它传递一个`error` prop。

例如，我们可以写:

```
import React from "react";
import Radio from "@material-ui/core/Radio";
import FormControlLabel from "@material-ui/core/FormControlLabel";
import FormLabel from "@material-ui/core/FormLabel";
import FormControl from "@material-ui/core/FormControl";
import FormHelperText from "@material-ui/core/FormHelperText";
import Button from "@material-ui/core/Button";
import RadioGroup from "@material-ui/core/RadioGroup";export default function App() {
  const [value, setValue] = React.useState("");
  const [error, setError] = React.useState(false);
  const [helperText, setHelperText] = React.useState(""); const handleRadioChange = event => {
    setValue(event.target.value);
    setHelperText(" ");
    setError(false);
  }; const handleSubmit = event => {
    event.preventDefault(); if (value === "2") {
      setHelperText("right");
      setError(false);
    } else if (value === "3") {
      setHelperText("wrong");
      setError(true);
    } else {
      setHelperText("Please select an option.");
      setError(true);
    }
  }; return (
    <form onSubmit={handleSubmit}>
      <FormControl component="fieldset" error={error}>
        <FormLabel component="legend">What's 1 + 1?</FormLabel>
        <RadioGroup name="quiz" value={value} onChange={handleRadioChange}>
          <FormControlLabel value="2" control={<Radio />} label="2" />
          <FormControlLabel value="3" control={<Radio />} label="3" />
        </RadioGroup>
        <FormHelperText>{helperText}</FormHelperText>
        <Button type="submit" variant="outlined" color="primary">
          Check Answer
        </Button>
      </FormControl>
    </form>
  );
}
```

我们用`value`状态来设置用户选择的值。

在`App`中，我们返回一个带有`FormComtrol`组件的表单来包装单选按钮。

`RadioGroup`有单选按钮组。

`FormControlLabel`给了里面有单选按钮的标签。

当我们点击按钮时，`handeSubmit`功能运行。

在函数中，我们调用`preventDefault`来停止默认的提交行为。

然后我们检查用户是否选择了有效的选项。

我们设置辅助文本和`error`状态，以便我们可以将其传递给`error`属性。

帮助文本显示在单选按钮和检查答案按钮之间。

# 挑选

我们可以添加`Select`组件来添加一个选择下拉列表。

例如，我们可以写:

```
import React from "react";
import InputLabel from "@material-ui/core/InputLabel";
import MenuItem from "@material-ui/core/MenuItem";
import FormControl from "@material-ui/core/FormControl";
import Select from "@material-ui/core/Select";export default function App() {
  const [fruit, setFruit] = React.useState(""); const handleChange = event => {
    setFruit(event.target.value);
  }; return (
    <form>
      <FormControl>
        <InputLabel id="fruit-label">fruit</InputLabel>
        <Select
          labelId="fruit-label"
          id="fruit"
          value={fruit}
          onChange={handleChange}
        >
          <MenuItem value="apple">Apple</MenuItem>
          <MenuItem value="orange">Orange</MenuItem>
          <MenuItem value="grape">Grape</MenuItem>
        </Select>
      </FormControl>
    </form>
  );
}
```

添加带有标签的下拉列表。

`InputLabel`让我们为下拉列表添加一个标签。

`Select`让我们添加一个下拉菜单。

`Select`的`labelId`应与`InputLabel`的`id`相匹配。

# 本地选择

为了使选择下拉菜单显示为本地下拉菜单，我们可以使用`Select`上的`native`属性。

例如，我们可以写:

```
import React from "react";
import InputLabel from "[@material](http://twitter.com/material)-ui/core/InputLabel";
import FormControl from "[@material](http://twitter.com/material)-ui/core/FormControl";
import Select from "[@material](http://twitter.com/material)-ui/core/Select";export default function App() {
  const [fruit, setFruit] = React.useState(""); const handleChange = event => {
    setFruit(event.target.value);
  }; return (
    <form>
      <FormControl>
        <Select
          native
          id="fruit"
          value={fruit}
          onChange={handleChange}
        >
          <option value="">Select one</option>
          <option value="apple">Apple</option>
          <option value="orange">Orange</option>
          <option value="grape">Grape</option>
        </Select>
      </FormControl>
    </form>
  );
}
```

我们将`native`道具添加到`Select`中。

我们用`option`元素替换了`MenuItem`。

我们还删除了标签，这样它就不会与下拉菜单重叠。

# 滑块

我们可以添加一个滑块，通过拖动滑块来添加数字输入。

要添加一个，我们可以使用`Slider`组件。

例如，我们可以写:

```
import React from "react";
import Slider from "[@material](http://twitter.com/material)-ui/core/Slider";export default function App() {
  const [value, setValue] = React.useState(10); const handleChange = event => {
    setValue(event.target.value);
  }; return (
    <form>
      <Slider
        value={value}
        onChange={handleChange}
      />
    </form>
  );
}
```

添加数字滑块。

我们将`value`状态设置为`value`道具。

此外，我们还将`onChange`道具设置为`handleChange`功能。

我们用滑块的选定值调用`setValue`来更新`value`状态。

现在可以拖动滑块的手柄来改变其选定值。

![](img/4d7ca121a61b1297d415ec26000229fc.png)

[un plash](https://unsplash.com?utm_source=medium&utm_medium=referral)上[di lyara gariflina](https://unsplash.com/@dilja96?utm_source=medium&utm_medium=referral)拍摄的照片

# 结论

我们可以使用`Slider`组件添加一个滑块。

此外，我们还可以用单选按钮显示错误。

下拉菜单可以添加`Select`组件。