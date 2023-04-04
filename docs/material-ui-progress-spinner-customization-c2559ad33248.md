# 材料用户界面-进度微调器定制

> 原文：<https://blog.devgenius.io/material-ui-progress-spinner-customization-c2559ad33248?source=collection_archive---------4----------------------->

![](img/4402da861cd19cbd37c497da43bc733a.png)

安娜·佩尔泽在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

材质 UI 是一个为 React 制作的材质设计库。

这是一组具有材质设计风格的 React 组件。

在本文中，我们将看看如何添加带有材质 UI 的进度微调器。

# 交互式集成

我们可以添加微调按钮和浮动动作按钮。

例如，我们可以写:

```
import React from "react";
import { makeStyles } from "[@material](http://twitter.com/material)-ui/core/styles";
import CircularProgress from "[@material](http://twitter.com/material)-ui/core/CircularProgress";
import { pink } from "[@material](http://twitter.com/material)-ui/core/colors";
import Fab from "[@material](http://twitter.com/material)-ui/core/Fab";
import CheckIcon from "[@material](http://twitter.com/material)-ui/icons/Check";
import SaveIcon from "[@material](http://twitter.com/material)-ui/icons/Save";
import clsx from "clsx";const useStyles = makeStyles(theme => ({
  root: {
    display: "flex",
    alignItems: "center"
  },
  wrapper: {
    margin: theme.spacing(1),
    position: "relative"
  },
  fabProgress: {
    color: pink[500],
    position: "absolute",
    top: -6,
    left: -6,
    zIndex: 1
  }
}));export default function App() {
  const classes = useStyles();
  const [loading, setLoading] = React.useState(false);
  const [success, setSuccess] = React.useState(false);
  const timer = React.useRef();const buttonClassname = clsx({
    [classes.buttonSuccess]: success
  });React.useEffect(() => {
    return () => {
      clearTimeout(timer.current);
    };
  }, []);const handleButtonClick = () => {
    if (!loading) {
      setSuccess(false);
      setLoading(true);
      timer.current = setTimeout(() => {
        setSuccess(true);
        setLoading(false);
      }, 1000);
    }
  }; return (
    <div className={classes.root}>
      <div className={classes.wrapper}>
        <Fab
          aria-label="save"
          color="primary"
          className={buttonClassname}
          onClick={handleButtonClick}
        >
          {success ? <CheckIcon /> : <SaveIcon />}
        </Fab>
        {loading && (
          <CircularProgress size={68} className={classes.fabProgress} />
        )}
      </div>
    </div>
  );
}
```

在浮动操作按钮周围添加加载微调器。

我们必须设置`fabProgress`类来移动微调器，使其环绕浮动动作按钮。

此外，我们必须用`wrapper`类改变间距，使按钮与微调按钮对齐。

当我们点击按钮时，运行`handleButtonClick`功能。

这将`loading`状态设置为`true`。

这将触发微调器显示。

然后 1 秒钟后，我们在`useEffect`回调中将`loading`状态设置为`false`。

这里的`success`状态将被设置为`true`，我们会看到复选标记图标。

同样，我们可以对按钮做同样的事情。

例如，我们可以写:

```
import React from "react";
import { makeStyles } from "[@material](http://twitter.com/material)-ui/core/styles";
import CircularProgress from "[@material](http://twitter.com/material)-ui/core/CircularProgress";
import { pink } from "[@material](http://twitter.com/material)-ui/core/colors";
import Button from "[@material](http://twitter.com/material)-ui/core/Button";
import clsx from "clsx";const useStyles = makeStyles(theme => ({
  root: {
    display: "flex",
    alignItems: "center"
  },
  wrapper: {
    margin: theme.spacing(1),
    position: "relative"
  },
  buttonProgress: {
    color: pink[500],
    position: "absolute",
    top: "50%",
    left: "50%",
    marginTop: -12,
    marginLeft: -12
  }
}));export default function App() {
  const classes = useStyles();
  const [loading, setLoading] = React.useState(false);
  const [success, setSuccess] = React.useState(false);
  const timer = React.useRef(); const buttonClassname = clsx({
    [classes.buttonSuccess]: success
  }); React.useEffect(() => {
    return () => {
      clearTimeout(timer.current);
    };
  }, []); const handleButtonClick = () => {
    if (!loading) {
      setSuccess(false);
      setLoading(true);
      timer.current = setTimeout(() => {
        setSuccess(true);
        setLoading(false);
      }, 1000);
    }
  }; return (
    <div className={classes.root}>
      <div className={classes.wrapper}>
        <Button
          variant="contained"
          color="primary"
          className={buttonClassname}
          disabled={loading}
          onClick={handleButtonClick}
        >
          save
        </Button>
        {loading && (
          <CircularProgress size={24} className={classes.buttonProgress} />
        )}
      </div>
    </div>
  );
}
```

添加一个中间带有微调器的保存按钮。

逻辑与浮动操作按钮相同。

但是我们有不同的样式让微调器在按钮中定位它。

# 带标签的圆形旋转器

我们可以在圆形微调器中显示进度。

例如，我们可以写:

```
import React from "react";
import CircularProgress from "[@material](http://twitter.com/material)-ui/core/CircularProgress";
import Typography from "[@material](http://twitter.com/material)-ui/core/Typography";
import Box from "[@material](http://twitter.com/material)-ui/core/Box";export default function App() {
  const [progress, setProgress] = React.useState(10); React.useEffect(() => {
    const timer = setInterval(() => {
      setProgress(prevProgress =>
        prevProgress >= 100 ? 10 : prevProgress + 10
      );
    }, 800);
    return () => {
      clearInterval(timer);
    };
  }, []); return (
    <div>
      <Box position="relative" display="inline-flex">
        <CircularProgress variant="static" value={progress} />
        <Box
          top={0}
          left={0}
          bottom={0}
          right={0}
          position="absolute"
          display="flex"
          alignItems="center"
          justifyContent="center"
        >
          <Typography
            variant="caption"
            component="div"
            color="textSecondary"
          >{`${Math.round(progress)}%`}</Typography>
        </Box>
      </Box>
    </div>
  );
}
```

添加一个进度微调器，其中包含一些动画来显示进度。

我们只是放了一个`Box`来显示文本。

`Box`有一个绝对位置，我们将`alignItems`和`justifyContent`设置为`center`以使文本在微调器中居中。

![](img/a6365574808ea47b80414e13f9952f22.png)

照片由 [Anh Nguyen](https://unsplash.com/@pwign?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以在按钮内或浮动动作按钮周围添加微调器。

此外，我们可以在循环进度微调器中显示进度。