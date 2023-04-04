# 材料界面—应用程序栏

> 原文：<https://blog.devgenius.io/material-ui-app-bar-7f426d551714?source=collection_archive---------0----------------------->

![](img/617ed0fa59d33f4aa34ea4a666e0fa19.png)

照片由[防溅板](https://unsplash.com?utm_source=medium&utm_medium=referral)上的[刻痕](https://unsplash.com/@jannerboy62?utm_source=medium&utm_medium=referral)拍摄

材质 UI 是一个为 React 制作的材质设计库。

这是一组具有材质设计风格的 React 组件。

在这篇文章中，我们将看看如何添加带有材质 UI 的应用程序栏。

# 应用程序栏

应用程序栏让我们显示与当前屏幕相关的信息和操作。

要添加一个，我们使用`AppBar`组件。

例如，我们可以写:

```
import React from "react";
import AppBar from "[@material](http://twitter.com/material)-ui/core/AppBar";
import Toolbar from "[@material](http://twitter.com/material)-ui/core/Toolbar";
import Button from "[@material](http://twitter.com/material)-ui/core/Button";
import IconButton from "[@material](http://twitter.com/material)-ui/core/IconButton";
import MenuIcon from "[@material](http://twitter.com/material)-ui/icons/Menu";export default function App() {
  return (
    <div>
      <AppBar position="static">
        <Toolbar>
          <IconButton edge="start" color="inherit">
            <MenuIcon />
          </IconButton>
          <Button color="inherit">home</Button>
          <Button color="inherit">Login</Button>
        </Toolbar>
      </AppBar>
    </div>
  );
}
```

我们添加了一个内部带有`Toolbar`的`AppBar`组件来为其添加内容。

它里面可以有文本和图标。

# 带有搜索栏的应用程序栏

我们可以添加一个带有`InputBase`组件的搜索框。

例如，我们可以这样写:

```
import React from "react";
import AppBar from "[@material](http://twitter.com/material)-ui/core/AppBar";
import Toolbar from "[@material](http://twitter.com/material)-ui/core/Toolbar";
import Button from "[@material](http://twitter.com/material)-ui/core/Button";
import IconButton from "[@material](http://twitter.com/material)-ui/core/IconButton";
import MenuIcon from "[@material](http://twitter.com/material)-ui/icons/Menu";
import InputBase from "[@material](http://twitter.com/material)-ui/core/InputBase";
import { makeStyles } from "[@material](http://twitter.com/material)-ui/core/styles";const useStyles = makeStyles(theme => ({
  inputRoot: {
    color: "inherit"
  },
  inputInput: {
    color: "white"
  }
}));export default function App() {
  const classes = useStyles(); return (
    <div>
      <AppBar position="static">
        <Toolbar>
          <IconButton edge="start" color="inherit" aria-label="menu">
            <MenuIcon />
          </IconButton>
          <Button color="inherit">home</Button>
          <Button color="inherit">Login</Button>
          <InputBase
            placeholder="Search"
            classes={{
              root: classes.inputRoot,
              input: classes.inputInput
            }}
          />
        </Toolbar>
      </AppBar>
    </div>
  );
}
```

用搜索框添加工具栏按钮。

搜索框被添加到`InputBase`组件中。

我们可以用`classes` prop 将类传递给它以对其进行样式化。

# 带菜单的应用程序栏

我们可以通过使用`AppBar`中`Toolbar`内部的`Menu`组件来添加一个带有菜单的 app bar。

例如，我们可以写:

```
import React from "react";
import AppBar from "[@material](http://twitter.com/material)-ui/core/AppBar";
import Toolbar from "[@material](http://twitter.com/material)-ui/core/Toolbar";
import Typography from "[@material](http://twitter.com/material)-ui/core/Typography";
import IconButton from "[@material](http://twitter.com/material)-ui/core/IconButton";
import MenuIcon from "[@material](http://twitter.com/material)-ui/icons/Menu";
import AccountCircle from "[@material](http://twitter.com/material)-ui/icons/AccountCircle";
import MenuItem from "[@material](http://twitter.com/material)-ui/core/MenuItem";
import Menu from "[@material](http://twitter.com/material)-ui/core/Menu";export default function App() {
  const [anchorEl, setAnchorEl] = React.useState(null);
  const open = Boolean(anchorEl); const handleMenu = event => {
    setAnchorEl(event.currentTarget);
  }; const handleClose = () => {
    setAnchorEl(null);
  }; return (
    <div>
      <AppBar position="static">
        <Toolbar>
          <IconButton edge="start" color="inherit" aria-label="menu">
            <MenuIcon />
          </IconButton>
          <Typography variant="h6">App</Typography> <div>
            <IconButton onClick={handleMenu} color="inherit">
              <AccountCircle />
            </IconButton>
            <Menu
              anchorEl={anchorEl}
              anchorOrigin={{
                vertical: "top",
                horizontal: "right"
              }}
              keepMounted
              transformOrigin={{
                vertical: "top",
                horizontal: "right"
              }}
              open={open}
              onClose={handleClose}
            >
              <MenuItem onClick={handleClose}>Logout</MenuItem>
              <MenuItem onClick={handleClose}>My account</MenuItem>
            </Menu>
          </div>
        </Toolbar>
      </AppBar>
    </div>
  );
}
```

添加我们的菜单。

我们将`Menu`添加到带有`Menu`组件的文本的右侧。

菜单的打开由`open`状态控制。

`onClose`将`open`状态设置为`false`关闭菜单。

菜单项也做同样的事情，当它们被点击时关闭菜单。

# 带有搜索栏的应用程序栏

如果我们对`InputBase`组件应用一些样式，我们可以添加一个搜索框。

例如，我们可以写:

```
import React from "react";
import AppBar from "[@material](http://twitter.com/material)-ui/core/AppBar";
import Toolbar from "[@material](http://twitter.com/material)-ui/core/Toolbar";
import IconButton from "[@material](http://twitter.com/material)-ui/core/IconButton";
import Typography from "[@material](http://twitter.com/material)-ui/core/Typography";
import InputBase from "[@material](http://twitter.com/material)-ui/core/InputBase";
import { makeStyles } from "[@material](http://twitter.com/material)-ui/core/styles";
import MenuIcon from "[@material](http://twitter.com/material)-ui/icons/Menu";
import SearchIcon from "[@material](http://twitter.com/material)-ui/icons/Search";const useStyles = makeStyles(theme => ({
  search: {
    position: "relative",
    borderRadius: theme.shape.borderRadius,
    marginLeft: 0,
    width: "100%"
  },
  searchIcon: {
    padding: theme.spacing(0, 2),
    height: "100%",
    position: "absolute",
    display: "flex",
    alignItems: "center",
    justifyContent: "center"
  },
  inputRoot: {
    color: "inherit"
  },
  inputInput: {
    padding: theme.spacing(1, 1, 1, 0),
    paddingLeft: `calc(1em + ${theme.spacing(4)}px)`,
    width: "100%"
  }
}));export default function App() {
  const classes = useStyles(); return (
    <div>
      <AppBar position="static">
        <Toolbar>
          <IconButton edge="start" color="inherit" aria-label="menu">
            <MenuIcon />
          </IconButton>
          <Typography variant="h6">App</Typography> <div className={classes.search}>
            <div className={classes.searchIcon}>
              <SearchIcon />
            </div>
            <InputBase
              placeholder="Search…"
              classes={{
                root: classes.inputRoot,
                input: classes.inputInput
              }}
            />
          </div>
        </Toolbar>
      </AppBar>
    </div>
  );
}
```

我们必须对搜索框应用一些样式，使它在工具栏中正确显示。

我们调整了图标`absolute` 的位置，这样我们可以将它移动到工具栏的中心。

这是通过使用`searchIcon`类将`alignItems`和`justifyContent`设置为`center`来完成的。

此外，我们必须用`search`类将搜索框垂直居中。

我们还用`borderRadius`把它弄圆了。

![](img/25e149a11bf2c82e330bcd3da8030c65.png)

照片由[大卫·直](https://unsplash.com/@davidstraight?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以添加一个带有文本、图标和输入框的应用程序栏。

要添加它们，我们必须设计它们的样式，使它们在盒子里看起来合适。