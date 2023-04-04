# 材料用户界面-自定义工具提示

> 原文：<https://blog.devgenius.io/material-ui-customize-tooltips-4f82c7857cb2?source=collection_archive---------0----------------------->

![](img/795316527a4202ef693ce2bd36bcdea8.png)

照片由[凯特·雷默](https://unsplash.com/@studioktr?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

材质 UI 是一个为 React 制作的材质设计库。

这是一组具有材质设计风格的 React 组件。

在本文中，我们将看看如何用材质 UI 定制工具提示。

# 扳机

我们可以改变工具提示的触发方式。

要禁止在悬停时显示工具提示，我们可以使用`disableHoverListener`道具。

当我们聚焦于一个元素时，`disableFocusListener` prop 允许我们禁用显示工具提示。

`disableTouchListener`让我们在触摸一个元素时禁用工具提示显示。

例如，我们可以写:

```
import React from "react";
import DeleteIcon from "[@material](http://twitter.com/material)-ui/icons/Delete";
import IconButton from "[@material](http://twitter.com/material)-ui/core/IconButton";
import Tooltip from "[@material](http://twitter.com/material)-ui/core/Tooltip";export default function App() {
  return (
    <div>
      <Tooltip title="Delete File" disableFocusListener disableTouchListener>
        <IconButton>
          <DeleteIcon />
        </IconButton>
      </Tooltip>
    </div>
  );
}
```

禁用焦点和触摸监听器。

这样，我们将看到工具提示，如果我们悬停或点击它。

我们也可以按照我们想要的方式控制工具提示的打开。

我们可以让它在点击时显示，当我们点击离开时消失。

为此，我们写道:

```
import React from "react";
import DeleteIcon from "[@material](http://twitter.com/material)-ui/icons/Delete";
import IconButton from "[@material](http://twitter.com/material)-ui/core/IconButton";
import Tooltip from "[@material](http://twitter.com/material)-ui/core/Tooltip";
import ClickAwayListener from "[@material](http://twitter.com/material)-ui/core/ClickAwayListener";export default function App() {
  const [open, setOpen] = React.useState(false); const handleTooltipClose = () => {
    setOpen(false);
  }; const handleTooltipOpen = () => {
    setOpen(true);
  }; return (
    <div>
      <ClickAwayListener onClickAway={handleTooltipClose}>
        <Tooltip
          open={open}
          title="Delete File"
          disableFocusListener
          disableHoverListener
          disableTouchListener
        >
          <IconButton onClick={handleTooltipOpen}>
            <DeleteIcon />
          </IconButton>
        </Tooltip>
      </ClickAwayListener>
    </div>
  );
}
```

我们添加了一个`ClickAwayListener`，当我们点击离开时，它会关闭带有`handleTooltipClose`功能的工具提示。

而当我们点击`IconButton`时，`handleTooltipOpen`会打开工具提示。

这些都是通过改变`open`状态来完成的。

我们将它直接传入`Tooltip`的`open`支柱。

同样，我们有`disableFocusListener`、`disableHoverListener` 和`disableTouchListener`来禁用所有的监听器。

# 受控工具提示

要使工具提示成为受控工具提示，我们可以使用`open`、`onClose`和`onOpen`道具。

例如，我们可以写:

```
import React from "react";
import DeleteIcon from "[@material](http://twitter.com/material)-ui/icons/Delete";
import IconButton from "[@material](http://twitter.com/material)-ui/core/IconButton";
import Tooltip from "[@material](http://twitter.com/material)-ui/core/Tooltip";export default function App() {
  const [open, setOpen] = React.useState(false); const handleTooltipClose = () => {
    setOpen(false);
  }; const handleTooltipOpen = () => {
    setOpen(true);
  }; return (
    <div>
      <Tooltip
        open={open}
        onClose={handleTooltipClose}
        onOpen={handleTooltipOpen}
        title="Delete File"
      >
        <IconButton>
          <DeleteIcon />
        </IconButton>
      </Tooltip>
    </div>
  );
}
```

增加一个由`open`支柱控制开合的`Tooltip`。

道具设置为`open`状态。

然后我们有`onClose`道具来设置工具提示关闭，当我们做一些像悬停离开关闭它。

它将`open`状态设置为`false`。

我们还有`onOpen`道具，通过将`open`状态设置为`true`来打开工具提示。

# 可变宽度

我们可以通过设置自定义宽度的`tooltip`类来改变工具提示的宽度。

例如，我们可以写:

```
import React from "react";
import Tooltip from "[@material](http://twitter.com/material)-ui/core/Tooltip";
import Button from "[@material](http://twitter.com/material)-ui/core/Button";
import { makeStyles } from "[@material](http://twitter.com/material)-ui/core/styles";const useStyles = makeStyles(theme => ({
  customWidth: {
    maxWidth: 500
  }
}));const longText = `
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nullam sagittis ullamcorper vehicula. Integer viverra est sed purus vulputate tempus. Vestibulum facilisis, metus et sollicitudin cursus, mi sapien suscipit nunc, vitae tristique diam tortor a urna. Proin auctor, ante ac viverra posuere, urna tortor semper mauris, 
`;export default function App() {
  const classes = useStyles(); return (
    <div>
      <Tooltip title={longText} classes={{ tooltip: classes.customWidth }}>
        <Button className={classes.button}>hover me</Button>
      </Tooltip>
    </div>
  );
}
```

我们添加了一个`longText`字符串，并将其传递给`title`属性。

这将被显示。

`classes`有工具提示的类。

我们设置了`tooltip`属性，将工具提示类设置为由`makeStyles`返回的`customWidth`类。

# 相互作用的

我们可以制作一个与`interactive`道具互动的工具提示。

这样，当用户悬停在工具提示上时，在关闭它的延迟到期之前，它不会关闭。

例如，我们可以写:

```
import React from "react";
import Button from "[@material](http://twitter.com/material)-ui/core/Button";
import Tooltip from "[@material](http://twitter.com/material)-ui/core/Tooltip";export default function App() {
  return (
    <Tooltip title="tooltip" interactive>
      <Button>Interactive</Button>
    </Tooltip>
  );
}
```

这样，当我们离开按钮时，我们会看到它关闭前的延迟。

![](img/5737f2a27d2b20126e6e753c01f58e1d.png)

Todd Quackenbush 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

自定义工具提示有很多种方法。

我们可以改变它的触发方式。

此外，我们可以设置离开延迟，宽度，并使其成为一个受控组件。