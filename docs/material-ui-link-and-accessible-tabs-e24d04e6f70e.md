# 材料用户界面-链接和可访问的选项卡

> 原文：<https://blog.devgenius.io/material-ui-link-and-accessible-tabs-e24d04e6f70e?source=collection_archive---------2----------------------->

![](img/8efdf16e442c5ab55ac6b28f955013ed.png)

赫尔墨斯·里维拉在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

材质 UI 是一个为 React 制作的材质设计库。

这是一组具有材质设计风格的 React 组件。

在这篇文章中，我们将看看如何添加垂直标签与材料用户界面。

# 垂直制表符

我们可以添加将`orientation`属性设置为`vertical`的垂直标签。

例如，我们可以写:

```
import React from "react";
import Tabs from "[@material](http://twitter.com/material)-ui/core/Tabs";
import Tab from "[@material](http://twitter.com/material)-ui/core/Tab";
import Box from "[@material](http://twitter.com/material)-ui/core/Box";function TabPanel(props) {
  const { children, value, index, ...other } = props; return <div {...other}>{value === index && <Box p={3}>{children}</Box>}</div>;
}export default function App() {
  const [value, setValue] = React.useState(0); const handleChange = (event, newValue) => {
    setValue(newValue);
  }; return (
    <div>
      <Tabs
        orientation="vertical"
        variant="scrollable"
        value={value}
        onChange={handleChange}
      >
        <Tab label="Item One" />
        <Tab label="Item Two" />
        <Tab label="Item Three" />
      </Tabs>
      <TabPanel value={value} index={0}>
        Item One
      </TabPanel>
      <TabPanel value={value} index={1}>
        Item Two
      </TabPanel>
      <TabPanel value={value} index={2}>
        Item Three
      </TabPanel>
    </div>
  );
}
```

添加下面有内容的选项卡。

因为我们将`orientation`设置为`vertical`，所以标签将是垂直的。

# 导航标签

我们也可以为标签页使用导航链接。

例如，我们可以写:

```
import React from "react";
import Tabs from "[@material](http://twitter.com/material)-ui/core/Tabs";
import Tab from "[@material](http://twitter.com/material)-ui/core/Tab";
import Box from "[@material](http://twitter.com/material)-ui/core/Box";
import AppBar from "[@material](http://twitter.com/material)-ui/core/AppBar";function TabPanel(props) {
  const { children, value, index, ...other } = props;return <div {...other}>{value === index && <Box p={3}>{children}</Box>}</div>;
}function LinkTab(props) {
  return (
    <Tab
      component="a"
      onClick={event => {
        event.preventDefault();
      }}
      {...props}
    />
  );
}export default function App() {
  const [value, setValue] = React.useState(0); const handleChange = (event, newValue) => {
    setValue(newValue);
  }; return (
    <div>
      <AppBar position="static">
        <Tabs variant="fullWidth" value={value} onChange={handleChange}>
          <LinkTab label="Page One" href="/foo" />
          <LinkTab label="Page Two" href="/bar" />
          <LinkTab label="Page Three" href="/baz" />
        </Tabs>
      </AppBar>
      <TabPanel value={value} index={0}>
        Page One
      </TabPanel>
      <TabPanel value={value} index={1}>
        Page Two
      </TabPanel>
      <TabPanel value={value} index={2}>
        Page Three
      </TabPanel>
    </div>
  );
}
```

添加链接选项卡。

我们有一个`LinkTab`组件，它将带有`onClick`属性的`Tab`呈现给一个调用`preventDefault`的函数，这样我们就可以导航到`href`属性中的 URL。

# 图标标签

此外，我们可以添加图标到标签。

我们将图标传入`Tab`的`icon`道具中添加图标。

例如，我们可以写:

```
import React from "react";
import Tabs from "[@material](http://twitter.com/material)-ui/core/Tabs";
import Tab from "[@material](http://twitter.com/material)-ui/core/Tab";
import Paper from "[@material](http://twitter.com/material)-ui/core/Paper";
import PhoneIcon from "[@material](http://twitter.com/material)-ui/icons/Phone";
import FavoriteIcon from "[@material](http://twitter.com/material)-ui/icons/Favorite";
import AirplanemodeActiveIcon from "[@material](http://twitter.com/material)-ui/icons/AirplanemodeActive";export default function App() {
  const [value, setValue] = React.useState(0); const handleChange = (event, newValue) => {
    setValue(newValue);
  }; return (
    <div>
      <Paper square>
        <Tabs
          value={value}
          onChange={handleChange}
          variant="fullWidth"
          indicatorColor="primary"
          textColor="primary"
        >
          <Tab icon={<PhoneIcon />} />
          <Tab icon={<FavoriteIcon />} />
          <Tab icon={<AirplanemodeActiveIcon />} />
        </Tabs>
      </Paper>
    </div>
  );
}
```

给我们的标签页添加图标。

我们将图标组件作为`Tab`的`icon`属性的值进行传递。

# 键盘导航

我们可以添加`selectionFollowsFocus`道具来启用键盘导航。/

例如，我们可以写:

```
import React from "react";
import Tabs from "[@material](http://twitter.com/material)-ui/core/Tabs";
import Tab from "[@material](http://twitter.com/material)-ui/core/Tab";
import Paper from "[@material](http://twitter.com/material)-ui/core/Paper";
import PhoneIcon from "[@material](http://twitter.com/material)-ui/icons/Phone";
import FavoriteIcon from "[@material](http://twitter.com/material)-ui/icons/Favorite";
import AirplanemodeActiveIcon from "[@material](http://twitter.com/material)-ui/icons/AirplanemodeActive";export default function App() {
  const [value, setValue] = React.useState(0); const handleChange = (event, newValue) => {
    setValue(newValue);
  }; return (
    <div>
      <Paper square>
        <Tabs
          selectionFollowsFocus
          value={value}
          onChange={handleChange}
          variant="fullWidth"
          indicatorColor="primary"
          textColor="primary"
        >
          <Tab icon={<PhoneIcon />} />
          <Tab icon={<FavoriteIcon />} />
          <Tab icon={<AirplanemodeActiveIcon />} />
        </Tabs>
      </Paper>
    </div>
  );
}
```

启用键盘导航。

如果我们把注意力放在标签上，那么我们就可以用键盘来导航。

![](img/ce9040fea1880ddfbacf233cc4c70de5.png)

泰·范·哈伦在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

# 结论

我们可以通过使标签垂直来定制标签。

此外，我们可以添加选项卡作为链接。

我们可以启用键盘导航。