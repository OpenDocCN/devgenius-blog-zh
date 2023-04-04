# 材料用户界面-工具提示

> 原文：<https://blog.devgenius.io/material-ui-tooltips-c2c9c1314a43?source=collection_archive---------1----------------------->

![](img/1432b6475ff2438ed40b0c6265826a4c.png)

照片由[安格尔·坎普](https://unsplash.com/@angelekamp?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

材质 UI 是一个为 React 制作的材质设计库。

这是一组具有材质设计风格的 React 组件。

在本文中，我们将看看如何用材质 UI 添加工具提示。

# 简单工具提示

我们可以添加简单的工具提示，当我们将鼠标悬停在另一个组件上时就会显示出来。

例如，我们可以添加一个带有`Fab`组件的浮动动作按钮。

然后我们用一个`Tooltip`包围它，显示一个工具提示。

例如，我们可以写:

```
import React from "react";
import AddIcon from "[@material](http://twitter.com/material)-ui/icons/Add";
import Fab from "[@material](http://twitter.com/material)-ui/core/Fab";
import DeleteIcon from "[@material](http://twitter.com/material)-ui/icons/Delete";
import IconButton from "[@material](http://twitter.com/material)-ui/core/IconButton";
import Tooltip from "[@material](http://twitter.com/material)-ui/core/Tooltip";export default function App() {
  return (
    <div>
      <Tooltip title="Delete File">
        <IconButton>
          <DeleteIcon />
        </IconButton>
      </Tooltip>
      <Tooltip title="Add Task">
        <Fab color="primary">
          <AddIcon />
        </Fab>
      </Tooltip>
    </div>
  );
}
```

用`Tooltip`组件添加工具提示。

我们可以把它围在一个`IconButton`或`Fab`周围。

`title`有将在工具提示中显示的内容。

# 定位工具提示

我们可以把工具提示放在我们想要的位置。

例如，我们可以写:

```
import React from "react";
import AddIcon from "[@material](http://twitter.com/material)-ui/icons/Add";
import Fab from "[@material](http://twitter.com/material)-ui/core/Fab";
import DeleteIcon from "[@material](http://twitter.com/material)-ui/icons/Delete";
import IconButton from "[@material](http://twitter.com/material)-ui/core/IconButton";
import Tooltip from "[@material](http://twitter.com/material)-ui/core/Tooltip";export default function App() {
  return (
    <div>
      <Tooltip title="Delete File" placement="left-end">
        <IconButton>
          <DeleteIcon />
        </IconButton>
      </Tooltip>
      <Tooltip title="Add Task" placement="left-start">
        <Fab color="primary">
          <AddIcon />
        </Fab>
      </Tooltip>
    </div>
  );
}
```

给每个工具提示添加一个`placement`道具来改变位置。

# 自定义工具提示

我们可以用`withStyles`函数来设计工具提示的样式。

例如，我们可以写:

```
import React from "react";
import { withStyles } from "[@material](http://twitter.com/material)-ui/core/styles";
import DeleteIcon from "[@material](http://twitter.com/material)-ui/icons/Delete";
import IconButton from "[@material](http://twitter.com/material)-ui/core/IconButton";
import Tooltip from "[@material](http://twitter.com/material)-ui/core/Tooltip";const LightTooltip = withStyles(theme => ({
  tooltip: {
    backgroundColor: theme.palette.common.white,
    color: "green",
    boxShadow: theme.shadows[1],
    fontSize: 11
  }
}))(Tooltip);export default function App() {
  return (
    <div>
      <LightTooltip title="Delete File" placement="left-end">
        <IconButton>
          <DeleteIcon />
        </IconButton>
      </LightTooltip>
    </div>
  );
}
```

我们将`backgroundColor`设为白色，内容设为绿色。

还有，我们把`fontSize`改成 11。

我们还可以使用`makeStyles`函数来创建可以传递到工具提示中的类。

例如，我们可以写:

```
import React from "react";
import { makeStyles } from "[@material](http://twitter.com/material)-ui/core/styles";
import DeleteIcon from "[@material](http://twitter.com/material)-ui/icons/Delete";
import IconButton from "[@material](http://twitter.com/material)-ui/core/IconButton";
import Tooltip from "[@material](http://twitter.com/material)-ui/core/Tooltip";const useStyles = makeStyles(theme => ({
  arrow: {
    color: theme.palette.common.black
  },
  tooltip: {
    backgroundColor: theme.palette.common.black
  }
}));function BlackTooltip(props) {
  const classes = useStyles(); return <Tooltip arrow classes={classes} {...props} />;
}export default function App() {
  return (
    <div>
      <BlackTooltip title="Delete File">
        <IconButton>
          <DeleteIcon />
        </IconButton>
      </BlackTooltip>
    </div>
  );
}
```

我们创建了一个`BlackTooltip`组件，它向`Tooltip`添加了一些类。

现在我们有了一个带有黑色箭头颜色的黑色工具提示，如`arrow`和`tooltip`属性所示。

# 箭头工具提示

为了给工具提示添加一个箭头，我们可以添加`arrow`道具。

比如，我们可以写；

```
import React from "react";
import DeleteIcon from "[@material](http://twitter.com/material)-ui/icons/Delete";
import IconButton from "[@material](http://twitter.com/material)-ui/core/IconButton";
import Tooltip from "[@material](http://twitter.com/material)-ui/core/Tooltip";export default function App() {
  return (
    <div>
      <Tooltip title="Delete File" arrow>
        <IconButton>
          <DeleteIcon />
        </IconButton>
      </Tooltip>
    </div>
  );
}
```

我们使用`Tooltip`上的`arrow`支柱来添加它。

# 自定义子元素

要添加自定义子元素，我们必须使用`forwardRef`并在回调中返回我们的组件。

这样，就应用了元素的 DOM 事件侦听器。

例如，我们可以写:

```
import React from "react";
import DeleteIcon from "[@material](http://twitter.com/material)-ui/icons/Delete";
import IconButton from "[@material](http://twitter.com/material)-ui/core/IconButton";
import Tooltip from "[@material](http://twitter.com/material)-ui/core/Tooltip";const Content = React.forwardRef((props, ref) => {
  return (
    <div {...props} ref={ref}>
      <IconButton>
        <DeleteIcon />
      </IconButton>
    </div>
  );
});export default function App() {
  return (
    <div>
      <Tooltip title="Delete File" arrow>
        <Content />
      </Tooltip>
    </div>
  );
}
```

创建一个可以放在`Tooltip`标签之间的`Content`组件。

我们将内容放在`forwardRef`回调函数中，这样工具提示就会正确显示。

![](img/556924d82e9807665699bf6ea4a00c02.png)

[K8](https://unsplash.com/@k8_iv?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

# 结论

我们可以创建不同风格和位置的工具提示。

内容也可以改变，以显示我们想要的。

样式可以以各种方式改变。