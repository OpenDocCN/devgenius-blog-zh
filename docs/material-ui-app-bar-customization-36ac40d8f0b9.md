# 材料界面—应用程序栏定制

> 原文：<https://blog.devgenius.io/material-ui-app-bar-customization-36ac40d8f0b9?source=collection_archive---------2----------------------->

![](img/f5cb7543a10d389b88b750c30a7c82ac.png)

安娜·佩尔泽在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

材质 UI 是一个为 React 制作的材质设计库。

这是一组具有材质设计风格的 React 组件。

在这篇文章中，我们将看看如何用 Material UI 定制应用程序栏。

# 密集的应用程序栏

我们可以为桌面应用添加密集的应用栏。

为此，我们将`variant`属性设置为`dense`。

例如，我们可以写:

```
import React from "react";
import AppBar from "[@material](http://twitter.com/material)-ui/core/AppBar";
import Toolbar from "[@material](http://twitter.com/material)-ui/core/Toolbar";
import IconButton from "[@material](http://twitter.com/material)-ui/core/IconButton";
import Typography from "[@material](http://twitter.com/material)-ui/core/Typography";
import MenuIcon from "[@material](http://twitter.com/material)-ui/icons/Menu";export default function App() {
  return (
    <div>
      <AppBar position="static">
        <Toolbar variant="dense">
          <IconButton edge="start">
            <MenuIcon />
          </IconButton>
          <Typography variant="h6" color="inherit">
            app
          </Typography>
        </Toolbar>
      </AppBar>
    </div>
  );
}
```

# 底部应用程序栏

为了将应用程序栏添加到页面底部，我们将其位置设为静态。

例如，我们可以写:

```
import React from "react";
import AppBar from "[@material](http://twitter.com/material)-ui/core/AppBar";
import Toolbar from "[@material](http://twitter.com/material)-ui/core/Toolbar";
import IconButton from "[@material](http://twitter.com/material)-ui/core/IconButton";
import Typography from "[@material](http://twitter.com/material)-ui/core/Typography";
import MenuIcon from "[@material](http://twitter.com/material)-ui/icons/Menu";
import Paper from "[@material](http://twitter.com/material)-ui/core/Paper";
import { makeStyles } from "[@material](http://twitter.com/material)-ui/core/styles";const useStyles = makeStyles(theme => ({
  paper: {
    height: "calc(100vh - 100px)"
  }
}));export default function App() {
  const classes = useStyles(); return (
    <div>
      <Paper square className={classes.paper}>
        foo
      </Paper>
      <AppBar position="static">
        <Toolbar variant="dense">
          <IconButton edge="start">
            <MenuIcon />
          </IconButton>
          <Typography variant="h6" color="inherit">
            app
          </Typography>
        </Toolbar>
      </AppBar>
    </div>
  );
}
```

我们用`position`道具使`AppBar`的位置静止。

然后我们在`Paper`组件中有我们的内容。

我们让它占据页面的大部分高度。

# 固定位置

我们可以用`position`支柱固定`AppBar`的位置。

例如，我们可以写:

```
import React from "react";
import AppBar from "[@material](http://twitter.com/material)-ui/core/AppBar";
import Toolbar from "[@material](http://twitter.com/material)-ui/core/Toolbar";
import IconButton from "[@material](http://twitter.com/material)-ui/core/IconButton";
import Typography from "[@material](http://twitter.com/material)-ui/core/Typography";
import MenuIcon from "[@material](http://twitter.com/material)-ui/icons/Menu";
import Paper from "[@material](http://twitter.com/material)-ui/core/Paper";
import { makeStyles } from "[@material](http://twitter.com/material)-ui/core/styles";const useStyles = makeStyles(theme => ({
  paper: {
    paddingTop: 50
  }
}));export default function App() {
  const classes = useStyles(); return (
    <div>
      <AppBar position="fixed">
        <Toolbar variant="dense">
          <IconButton edge="start">
            <MenuIcon />
          </IconButton>
          <Typography variant="h6" color="inherit">
            app
          </Typography>
        </Toolbar>
      </AppBar>
      <Paper square className={classes.paper}>
        foo
      </Paper>
    </div>
  );
}
```

使`AppBar`与`position`支柱固定在`fixed`上。

我们可以用`sticky`代替`fixed`得到同样的效果。

# 卷动

我们可以让应用程序栏在向下滚动时消失，

例如，我们可以写:

```
import React from "react";
import AppBar from "[@material](http://twitter.com/material)-ui/core/AppBar";
import Toolbar from "[@material](http://twitter.com/material)-ui/core/Toolbar";
import Typography from "[@material](http://twitter.com/material)-ui/core/Typography";
import CssBaseline from "[@material](http://twitter.com/material)-ui/core/CssBaseline";
import useScrollTrigger from "[@material](http://twitter.com/material)-ui/core/useScrollTrigger";
import Box from "[@material](http://twitter.com/material)-ui/core/Box";
import Container from "[@material](http://twitter.com/material)-ui/core/Container";
import Slide from "[@material](http://twitter.com/material)-ui/core/Slide";function HideOnScroll(props) {
  const { children, window } = props;
  const trigger = useScrollTrigger({ target: window ? window() : undefined }); return (
    <Slide appear={false} direction="down" in={!trigger}>
      {children}
    </Slide>
  );
}export default function App() {
  return (
    <>
      <CssBaseline />
      <HideOnScroll>
        <AppBar>
          <Toolbar>
            <Typography variant="h6"> App Bar</Typography>
          </Toolbar>
        </AppBar>
      </HideOnScroll>
      <Toolbar />
      <Container>
        <Box my={2}>
          {[...new Array(200)]
            .map(() => `Lorem ipsum dolor sit amet`)
            .join("\n")}
        </Box>
      </Container>
    </>
  );
}
```

让应用程序栏在我们向下滚动时消失。

我们用`HideOnScroll`组件做到了这一点。

`Slide`组件让我们在滚动时让滚动条消失。

这是通过`useScrollTrigger`挂钩完成的。

我们将`Slide`的`appear`道具设置为`false`，使滚动时内容消失。

`direction`属性被设置为`down`，这样当我们向下滚动时内容就会消失。

# 按钮返回到顶部

我们可以添加一个按钮，让我们滚动到页面的顶部。

例如，我们可以写:

```
import React from "react";
import AppBar from "[@material](http://twitter.com/material)-ui/core/AppBar";
import Toolbar from "[@material](http://twitter.com/material)-ui/core/Toolbar";
import Typography from "[@material](http://twitter.com/material)-ui/core/Typography";
import CssBaseline from "[@material](http://twitter.com/material)-ui/core/CssBaseline";
import useScrollTrigger from "[@material](http://twitter.com/material)-ui/core/useScrollTrigger";
import Box from "[@material](http://twitter.com/material)-ui/core/Box";
import Container from "[@material](http://twitter.com/material)-ui/core/Container";
import Fab from "[@material](http://twitter.com/material)-ui/core/Fab";
import KeyboardArrowUpIcon from "[@material](http://twitter.com/material)-ui/icons/KeyboardArrowUp";
import Zoom from "[@material](http://twitter.com/material)-ui/core/Zoom";
import { makeStyles } from "[@material](http://twitter.com/material)-ui/core/styles";const useStyles = makeStyles(theme => ({
  root: {
    position: "fixed",
    bottom: theme.spacing(2),
    right: theme.spacing(2)
  }
}));function ScrollTop(props) {
  const { children, window } = props;
  const classes = useStyles();
  const trigger = useScrollTrigger({
    target: window ? window() : undefined,
    disableHysteresis: true,
    threshold: 100
  }); const handleClick = event => {
    const anchor = (event.target.ownerDocument || document).querySelector(
      "#top"
    ); if (anchor) {
      anchor.scrollIntoView({ behavior: "smooth", block: "center" });
    }
  }; return (
    <Zoom in={trigger}>
      <div onClick={handleClick} className={classes.root}>
        {children}
      </div>
    </Zoom>
  );
}export default function App() {
  return (
    <React.Fragment>
      <CssBaseline />
      <AppBar>
        <Toolbar>
          <Typography variant="h6"> App Bar</Typography>
        </Toolbar>
      </AppBar>
      <Toolbar id="top" />
      <Container>
        <Box my={2}>
          {[...new Array(200)]
            .map(() => `Lorem ipsum dolor sit amet`)
            .join("\n")}
        </Box>
      </Container>
      <ScrollTop>
        <Fab color="secondary" size="small">
          <KeyboardArrowUpIcon />
        </Fab>
      </ScrollTop>
    </React.Fragment>
  );
}
```

添加一个浮动操作按钮，我们可以单击它移动到页面顶部。

`ScrollToTop`是一个使用`useScrollTrigger`标签移动到页面顶部的组件。

它有我们传递给`in` prop 的`trigger`。

`handleClick`函数让我们用`scrollIntoView`方法滚动到顶部。

![](img/7b1d0b83e99be30945f887a505c05fea.png)

照片由 [Anh Nguyen](https://unsplash.com/@pwign?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以添加各种定制的应用程序栏。

我们可以通过添加自己的代码来修复它并滚动到顶部。