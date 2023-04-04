# 材料用户界面—自定义选项卡

> 原文：<https://blog.devgenius.io/material-ui-customize-tabs-79a6a901e1ae?source=collection_archive---------0----------------------->

![](img/929afa5b73f3846e8c20151bf0cac428.png)

照片由[蒙蒂·艾伦](https://unsplash.com/@monty_a?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

材质 UI 是一个为 React 制作的材质设计库。

这是一组具有材质设计风格的 React 组件。

在本文中，我们将了解如何使用材质 UI 自定义选项卡。

# 集中

我们可以用`centered`道具制作居中的标签。

例如，我们可以写:

```
import React from "react";
import AppBar from "[@material](http://twitter.com/material)-ui/core/AppBar";
import Tabs from "[@material](http://twitter.com/material)-ui/core/Tabs";
import Tab from "[@material](http://twitter.com/material)-ui/core/Tab";
import Box from "[@material](http://twitter.com/material)-ui/core/Box";function TabPanel(props) {
  const { children, value, index, ...other } = props; return <div {...other}>{value === index && <Box p={3}>{children}</Box>}</div>;
}export default function App() {
  const [value, setValue] = React.useState(0); const handleChange = (event, newValue) => {
    setValue(newValue);
  }; return (
    <>
      <AppBar position="static">
        <Tabs value={value} onChange={handleChange} centered>
          <Tab label="Item One" />
          <Tab label="Item Two" />
        </Tabs>
      </AppBar>
      <TabPanel value={value} index={0}>
        Item One
      </TabPanel>
      <TabPanel value={value} index={1}>
        Item Two
      </TabPanel>
    </>
  );
}
```

那么选项卡将居中。

# 可滚动标签

我们可以通过将`scrollButtons`设置为`auto`并将`variant`设置为`scrollable`来制作可滚动的标签。

这样，滚动按钮将显示在手机上，隐藏在桌面上。

例如，我们可以写:

```
import React from "react";
import AppBar from "[@material](http://twitter.com/material)-ui/core/AppBar";
import Tabs from "[@material](http://twitter.com/material)-ui/core/Tabs";
import Tab from "[@material](http://twitter.com/material)-ui/core/Tab";
import Box from "[@material](http://twitter.com/material)-ui/core/Box";function TabPanel(props) {
  const { children, value, index, ...other } = props; return <div {...other}>{value === index && <Box p={3}>{children}</Box>}</div>;
}export default function App() {
  const [value, setValue] = React.useState(0); const handleChange = (event, newValue) => {
    setValue(newValue);
  }; return (
    <>
      <AppBar position="static">
        <Tabs
          value={value}
          onChange={handleChange}
          variant="scrollable"
          scrollButtons="auto"
        >
          {Array(10)
            .fill()
            .map((_, i) => (
              <Tab label={`Item ${i + 1}`} />
            ))}
        </Tabs>
      </AppBar>
      {Array(10)
        .fill()
        .map((_, i) => (
          <TabPanel value={value} index={i}>
            Item {i + 1}
          </TabPanel>
        ))}
    </>
  );
}
```

我们用`Array`构造函数创建了 10 个选项卡，并将它们映射到选项卡。

类似的，我们对`TabPanel` s 也做了同样的事情。

现在，当选项卡溢出页面时，我们应该会看到一个箭头按钮。

在手机上，我们可以通过滑动来滚动。

# 防止滚动按钮

我们可以在`scrollButtons`道具设置为`off`的情况下关闭滚动按钮。

例如，我们可以写:

```
import React from "react";
import AppBar from "[@material](http://twitter.com/material)-ui/core/AppBar";
import Tabs from "[@material](http://twitter.com/material)-ui/core/Tabs";
import Tab from "[@material](http://twitter.com/material)-ui/core/Tab";
import Box from "[@material](http://twitter.com/material)-ui/core/Box";function TabPanel(props) {
  const { children, value, index, ...other } = props; return <div {...other}>{value === index && <Box p={3}>{children}</Box>}</div>;
}export default function App() {
  const [value, setValue] = React.useState(0); const handleChange = (event, newValue) => {
    setValue(newValue);
  }; return (
    <>
      <AppBar position="static">
        <Tabs
          value={value}
          onChange={handleChange}
          variant="scrollable"
          scrollButtons="off"
        >
          {Array(10)
            .fill()
            .map((_, i) => (
              <Tab label={`Item ${i + 1}`} />
            ))}
        </Tabs>
      </AppBar>
      {Array(10)
        .fill()
        .map((_, i) => (
          <TabPanel value={value} index={i}>
            Item {i + 1}
          </TabPanel>
        ))}
    </>
  );
}
```

现在我们再也看不到标签滚动按钮了。

# 自定义选项卡样式

我们可以使用`withStyles`高阶组件自定义选项卡样式。

例如，我们可以写:

```
import React from "react";
import AppBar from "[@material](http://twitter.com/material)-ui/core/AppBar";
import Tabs from "[@material](http://twitter.com/material)-ui/core/Tabs";
import Tab from "[@material](http://twitter.com/material)-ui/core/Tab";
import Box from "[@material](http://twitter.com/material)-ui/core/Box";
import { withStyles } from "[@material](http://twitter.com/material)-ui/core/styles";const StyledTab = withStyles(theme => ({
  root: {
    textTransform: "none",
    color: "yellow",
    fontWeight: theme.typography.fontWeightRegular,
    fontSize: theme.typography.pxToRem(15),
    marginRight: theme.spacing(1)
  }
}))(props => <Tab disableRipple {...props} />);function TabPanel(props) {
  const { children, value, index, ...other } = props; return <div {...other}>{value === index && <Box p={3}>{children}</Box>}</div>;
}export default function App() {
  const [value, setValue] = React.useState(0); const handleChange = (event, newValue) => {
    setValue(newValue);
  }; return (
    <>
      <AppBar position="static">
        <Tabs
          value={value}
          onChange={handleChange}
          variant="scrollable"
          scrollButtons="off"
        >
          {Array(5)
            .fill()
            .map((_, i) => (
              <StyledTab label={`Item ${i + 1}`} />
            ))}
        </Tabs>
      </AppBar>
      {Array(5)
        .fill()
        .map((_, i) => (
          <TabPanel value={value} index={i}>
            Item {i + 1}
          </TabPanel>
        ))}
    </>
  );
}
```

我们用带有一些样式的对象调用`withStyles`函数。

我们传入一个函数，该函数返回一个传入了道具的`Tab`。

这样，我们将对象中的样式应用于选项卡。

然后我们可以在我们的`App`组件中使用返回的`StyledTab`。

![](img/c36a0cde6e4a9a329b1e7a93e9812434.png)

照片由 [Joyce Romero](https://unsplash.com/@joyceromero?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 结论

我们可以用各种方式自定义选项卡。

样式、滚动能力等等都可以改变。