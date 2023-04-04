# 材质 UI —浮动动作按钮

> 原文：<https://blog.devgenius.io/material-ui-floating-action-buttons-c5c878bcceaa?source=collection_archive---------6----------------------->

![](img/9e7cdcdc53c964f87ef1efe2c54bddc4.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由[Louis Hansel @ shotsoflouis](https://unsplash.com/@louishansel?utm_source=medium&utm_medium=referral)拍摄

材质 UI 是一个为 React 制作的材质设计库。

这是一组具有材质设计风格的 React 组件。

在本文中，我们将看看如何添加带有材质 UI 的浮动按钮。

# 浮动操作按钮

我们可以用`Fab`组件添加浮动动作按钮。

例如，我们可以写:

```
import React from "react";
import Fab from "[@material](http://twitter.com/material)-ui/core/Fab";
import AddIcon from "[@material](http://twitter.com/material)-ui/icons/Add";export default function App() {
  return (
    <Fab color="primary" aria-label="add">
      <AddIcon />
    </Fab>
  );
}
```

用`Fab`组件给我们的应用程序添加一个浮动的动作按钮。

我们在里面有一个图标。

# 大小

为了改变尺寸，我们设置了`size`道具。

例如，我们可以写:

```
import React from "react";
import Fab from "[@material](http://twitter.com/material)-ui/core/Fab";
import AddIcon from "[@material](http://twitter.com/material)-ui/icons/Add";export default function App() {
  return (
    <Fab size="small">
      <AddIcon />
    </Fab>
  );
}
```

将按钮大小更改为小。

# 动画

当我们切换标签时，我们可以动画显示浮动的动作按钮。

为了添加动画，我们编写:

```
import React from "react";
import Fab from "[@material](http://twitter.com/material)-ui/core/Fab";
import SwipeableViews from "react-swipeable-views";
import AppBar from "[@material](http://twitter.com/material)-ui/core/AppBar";
import Tabs from "[@material](http://twitter.com/material)-ui/core/Tabs";
import Tab from "[@material](http://twitter.com/material)-ui/core/Tab";
import Typography from "[@material](http://twitter.com/material)-ui/core/Typography";
import Box from "[@material](http://twitter.com/material)-ui/core/Box";
import AddIcon from "[@material](http://twitter.com/material)-ui/icons/Add";
import EditIcon from "[@material](http://twitter.com/material)-ui/icons/Edit";
import UpIcon from "[@material](http://twitter.com/material)-ui/icons/KeyboardArrowUp";
import Zoom from "[@material](http://twitter.com/material)-ui/core/Zoom";function TabPanel(props) {
  const { children, value, index, ...other } = props;return (
    <Typography
      component="div"
      role="tabpanel"
      hidden={value !== index}
      id={`action-tabpanel-${index}`}
      aria-labelledby={`action-tab-${index}`}
      {...other}
    >
      {value === index && <Box p={3}>{children}</Box>}
    </Typography>
  );
}export default function App() {
  const [value, setValue] = React.useState(0); const handleChange = (event, newValue) => {
    setValue(newValue);
  }; const handleChangeIndex = index => {
    setValue(index);
  }; const fabs = [
    {
      color: "primary",
      icon: <AddIcon />,
      label: "Add"
    },
    {
      color: "secondary",
      icon: <EditIcon />,
      label: "Edit"
    },
    {
      color: "inherit",
      icon: <UpIcon />,
      label: "Expand"
    }
  ];return (
    <div>
      <AppBar position="static" color="default">
        <Tabs
          value={value}
          onChange={handleChange}
          indicatorColor="primary"
          textColor="primary"
          variant="fullWidth"
          aria-label="action tabs example"
        >
          <Tab label="Item One" />
          <Tab label="Item Two" />
          <Tab label="Item Three" />
        </Tabs>
      </AppBar>
      <SwipeableViews axis="x" index={value} onChangeIndex={handleChangeIndex}>
        <TabPanel value={value} index={0}>
          Item One
        </TabPanel>
        <TabPanel value={value} index={1}>
          Item Two
        </TabPanel>
        <TabPanel value={value} index={2}>
          Item Three
        </TabPanel>
      </SwipeableViews>
      {fabs.map((fab, index) => (
        <Zoom
          key={fab.color}
          in={value === index}
          timeout={100}
          style={{
            transitionDelay: `0ms`
          }}
          unmountOnExit
        >
          <Fab
            aria-label={fab.label}
            className={fab.className}
            color={fab.color}
          >
            {fab.icon}
          </Fab>
        </Zoom>
      ))}
    </div>
  );
}
```

我们添加了一个`AppBar`来在顶部显示一个条，下面是`Tab` s。

然后为了显示选项卡内容，我们用`TabPanel`组件来显示它们。

我们自己创建了`TabPanel`组件，根据所选选项卡的索引来显示和隐藏项目。

然后我们在它下面显示浮动的动作按钮。

`Box`上的`p`支柱让我们添加一些衬垫。

我们用`Typography`组件设置字体。

`component`设置为 div。

`hidden`让我们选择要隐藏的内容，即索引未被选中的选项卡。

我们将它放在`Zoom`组件中，以便在显示按钮时得到缩放转换。

过渡的持续时间将是`timeout`值。

它以毫秒为单位。

`unmountOnExit`切换标签页时会清除按钮。

当我们单击选项卡标题时，索引被更新，这将触发`handleChange`功能运行。

如果我们滑动标签，那么`handleChangeIndex`功能将运行并执行与`handleChange`相同的操作。

![](img/cb7df534ae6a9cec5e28341e9113d640.png)

照片由[布鲁克·拉克](https://unsplash.com/@brookelark?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以添加不同样式和大小的浮动动作按钮。

此外，我们可以在一个选项卡中的不同按钮之间转换。