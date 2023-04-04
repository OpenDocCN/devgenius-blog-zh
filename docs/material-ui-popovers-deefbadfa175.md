# 材质用户界面—弹出框

> 原文：<https://blog.devgenius.io/material-ui-popovers-deefbadfa175?source=collection_archive---------1----------------------->

![](img/3d7fe0c334afd67608157b4c2733763f.png)

[布莱恩·陈](https://unsplash.com/@tigerrulezzz?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

材质 UI 是一个为 React 制作的材质设计库。

这是一组具有材质设计风格的 React 组件。

在本文中，我们将看看如何添加带有材质 UI 的 popovers。

# 简单的爆米花

Popovers 让我们在其他东西上显示一些内容。

要添加它，我们可以使用`Popover`组件。

例如，我们可以写:

```
import React from "react";
import { makeStyles } from "[@material](http://twitter.com/material)-ui/core/styles";
import Popover from "[@material](http://twitter.com/material)-ui/core/Popover";
import Typography from "[@material](http://twitter.com/material)-ui/core/Typography";
import Button from "[@material](http://twitter.com/material)-ui/core/Button";const useStyles = makeStyles(theme => ({
  typography: {
    padding: theme.spacing(3)
  }
}));export default function App() {
  const classes = useStyles();
  const [anchorEl, setAnchorEl] = React.useState(null); const handleClick = event => {
    setAnchorEl(event.currentTarget);
  }; const handleClose = () => {
    setAnchorEl(null);
  }; const open = Boolean(anchorEl); return (
    <div>
      <Button variant="contained" color="primary" onClick={handleClick}>
        Open Popover
      </Button>
      <Popover
        open={open}
        anchorEl={anchorEl}
        onClose={handleClose}
        anchorOrigin={{
          vertical: "bottom",
          horizontal: "center"
        }}
        transformOrigin={{
          vertical: "top",
          horizontal: "center"
        }}
      >
        <Typography className={classes.typography}>Popover content.</Typography>
      </Popover>
    </div>
  );
}
```

添加一个按钮来打开弹出窗口和弹出窗口本身。

`anchorEl`道具有 popover 的锚元素，就是按钮本身。

`anchorOrigin`指定弹出窗口相对于按钮的显示位置。

`transformOrigin`让我们改变 popover 本身的位置。

`open`设置 popover ios 是否打开。

如果`anchorEl`被置位，则它打开。

不然就不开了。

`onClose`是弹出窗口关闭时运行的功能。

# 鼠标悬停在交互上

当我们将鼠标移到一个元素上时，我们可以打开一个弹出窗口。

例如，我们可以写:

```
import React from "react";
import Popover from "[@material](http://twitter.com/material)-ui/core/Popover";
import Typography from "[@material](http://twitter.com/material)-ui/core/Typography";
import { makeStyles } from "[@material](http://twitter.com/material)-ui/core/styles";const useStyles = makeStyles(theme => ({
  popover: {
    pointerEvents: "none"
  },
  paper: {
    padding: theme.spacing(3)
  }
}));export default function App() {
  const classes = useStyles();
  const [anchorEl, setAnchorEl] = React.useState(null); const handlePopoverOpen = event => {
    setAnchorEl(event.currentTarget);
  }; const handlePopoverClose = () => {
    setAnchorEl(null);
  }; const open = Boolean(anchorEl); return (
    <div>
      <Typography
        onMouseEnter={handlePopoverOpen}
        onMouseLeave={handlePopoverClose}
      >
        Hover me
      </Typography>
      <Popover
        className={classes.popover}
        classes={{
          paper: classes.paper
        }}
        open={open}
        anchorEl={anchorEl}
        anchorOrigin={{
          vertical: "bottom",
          horizontal: "left"
        }}
        transformOrigin={{
          vertical: "top",
          horizontal: "left"
        }}
        onClose={handlePopoverClose}
        disableRestoreFocus
      >
        <Typography>some popover.</Typography>
      </Popover>
    </div>
  );
}
```

我们将函数传递给`onMouseEnter` prop，让我们在鼠标悬停在文本上时打开弹出窗口。

类似地，当我们的鼠标分别离开元素时，我们向`onMouseLeave`属性传递一个函数来关闭模态。

其他部分与上例相同。

# PopupState 助手

为了使 popover 状态的管理更容易，我们可以使用 material-ui-popup-state 库来管理状态。

要安装它，我们运行:

```
npm install --save material-ui-popup-state
```

然后我们可以写:

```
import React from "react";
import Typography from "[@material](http://twitter.com/material)-ui/core/Typography";
import Box from "[@material](http://twitter.com/material)-ui/core/Box";
import Button from "[@material](http://twitter.com/material)-ui/core/Button";
import Popover from "[@material](http://twitter.com/material)-ui/core/Popover";
import PopupState, { bindTrigger, bindPopover } from "material-ui-popup-state";export default function App() {
  return (
    <PopupState variant="popover">
      {popupState => (
        <div>
          <Button
            variant="contained"
            color="primary"
            {...bindTrigger(popupState)}
          >
            Open Popover
          </Button>
          <Popover
            {...bindPopover(popupState)}
            anchorOrigin={{
              vertical: "bottom",
              horizontal: "center"
            }}
            transformOrigin={{
              vertical: "top",
              horizontal: "center"
            }}
          >
            <Box p={3}>
              <Typography>some popover.</Typography>
            </Box>
          </Popover>
        </div>
      )}
    </PopupState>
  );
}
```

我们将`PopupState`组件的`variant`设置为`popover`来显示弹出窗口。

然后在它里面，我们有一个接受`popupState`参数的函数，它有弹出窗口的打开状态。

我们使用`bindTrigger`返回一个对象来切换按钮中的弹出窗口。

在弹出窗口中，我们调用`bindPopover`将`open`道具绑定到`popupState`上。

代码的其余部分与前面的示例相同。

![](img/1f3852413a126153ea837360e423d405.png)

照片由[阿丽娜·卡尔彭科](https://unsplash.com/@alinasagirova?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 结论

我们可以用`Popover`组件添加 popovers。

材质界面弹出状态库可以使管理弹出状态更加容易。