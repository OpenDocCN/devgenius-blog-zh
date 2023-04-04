# 材料用户界面-芯片和分频器

> 原文：<https://blog.devgenius.io/material-ui-chips-and-dividers-88649471deca?source=collection_archive---------2----------------------->

![](img/7b6a3b7d0dae8a6d8dbdb985cf64e1de.png)

照片由[卡祖恩德](https://unsplash.com/@kazuend?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

材质 UI 是一个为 React 制作的材质设计库。

这是一组具有材质设计风格的 React 组件。

在这篇文章中，我们将看看如何自定义芯片和添加材料用户界面分频器。

# 小芯片

我们可以添加`size`道具来改变芯片的大小。

例如，我们可以写:

```
import React from "react";
import Chip from "[@material](http://twitter.com/material)-ui/core/Chip";export default function App() {
  return (
    <div>
      <Chip size="small" label="Basic" />
    </div>
  );
}
```

做一个小的基本芯片。

# 概述变体

我们可以通过将`variant`道具设置为`outlined`来制作芯片轮廓。

例如，我们可以写:

```
import React from "react";
import Chip from "[@material](http://twitter.com/material)-ui/core/Chip";export default function App() {
  return (
    <div>
      <Chip variant="outlined" label="Basic" />
    </div>
  );
}
```

# 神使

我们可以在芯片上添加头像。

例如，我们可以写:

```
import React from "react";
import Chip from "[@material](http://twitter.com/material)-ui/core/Chip";
import Avatar from "[@material](http://twitter.com/material)-ui/core/Avatar";export default function App() {
  return (
    <div>
      <Chip
        label="cat"
        avatar={<Avatar alt="cat" src="[http://placekitten.com/200/200](http://placekitten.com/200/200)" />}
      />
    </div>
  );
}
```

我们将标签设置为与`label`道具一起显示。

`avatar`左边有我们要展示的头像。

# 核标准情报中心

我们可以在芯片标签旁边显示一个图标。

为此，我们将一个图标组件设置为`icon`属性的值。

例如，我们可以写:

```
import React from "react";
import Chip from "[@material](http://twitter.com/material)-ui/core/Chip";
import FaceIcon from "[@material](http://twitter.com/material)-ui/icons/Face";export default function App() {
  return (
    <div>
      <Chip label="person" icon={<FaceIcon />} />
    </div>
  );
}
```

我们将`icon`设置为`FaceIcon`以将其显示在标签的左侧。

删除图标可以用`deleteIcon`标签更改。

例如，我们可以写:

```
import React from "react";
import Chip from "[@material](http://twitter.com/material)-ui/core/Chip";
import DoneIcon from "[@material](http://twitter.com/material)-ui/icons/Done";export default function App() {
  const handleDelete = () => {
    console.info("delete icon clicked");
  }; return (
    <div>
      <Chip label="task" onDelete={handleDelete} deleteIcon={<DoneIcon />} />
    </div>
  );
}
```

添加我们选择的删除图标。

我们有`onDelete` prop 来运行一个函数，当它被点击时做一些事情。

此外，我们有`deleteIcon`来显示我们设置的删除图标。

我们还可以通过传入一个`onClick`道具来使芯片可点击。

例如，我们可以写:

```
import React from "react";
import Chip from "[@material](http://twitter.com/material)-ui/core/Chip";export default function App() {
  const handleClick = () => {
    console.info("d clicked");
  }; return (
    <div>
      <Chip label="task" onClick={handleClick} />
    </div>
  );
}
```

我们添加了`onClick` prop 并向其传递一个函数来处理点击。

# 圆规

分隔线让我们添加一条细线来分组项目。

我们可以在列表中添加分隔线。

例如，我们可以写:

```
import React from "react";
import List from "[@material](http://twitter.com/material)-ui/core/List";
import ListItem from "[@material](http://twitter.com/material)-ui/core/ListItem";
import ListItemText from "[@material](http://twitter.com/material)-ui/core/ListItemText";
import Divider from "[@material](http://twitter.com/material)-ui/core/Divider";const fruits = [
  { key: 1, name: "apple" },
  { key: 2, name: "orange" },
  { key: 3, name: "grape" }
];export default function App() {
  return (
    <div>
      <List component="nav">
        {fruits.map(f => [
          <ListItem button>
            <ListItemText primary={f.name} key={f.key} />
          </ListItem>,
          <Divider />
        ])}
      </List>
    </div>
  );
}
```

在每个`ListItem`下方添加一个分隔线。

# 嵌入式分隔器

我们可以添加不占据容器整个宽度的隔板。

例如，我们可以写:

```
import React from "react";
import List from "[@material](http://twitter.com/material)-ui/core/List";
import ListItem from "[@material](http://twitter.com/material)-ui/core/ListItem";
import ListItemText from "[@material](http://twitter.com/material)-ui/core/ListItemText";
import Divider from "[@material](http://twitter.com/material)-ui/core/Divider";
import ListItemAvatar from "[@material](http://twitter.com/material)-ui/core/ListItemAvatar";
import Avatar from "[@material](http://twitter.com/material)-ui/core/Avatar";
import ImageIcon from "[@material](http://twitter.com/material)-ui/icons/Image";const fruits = [
  { key: 1, name: "apple" },
  { key: 2, name: "orange" },
  { key: 3, name: "grape" }
];export default function App() {
  return (
    <div>
      <List component="nav">
        {fruits.map(f => [
          <ListItem>
            <ListItemAvatar>
              <Avatar>
                <ImageIcon />
              </Avatar>
            </ListItemAvatar>
            <ListItemText primary={f.name} key={f.key} secondary={f.name} />
          </ListItem>,
          <Divider variant="inset" component="li" />
        ])}
      </List>
    </div>
  );
}
```

添加带有头像的列表项。

在它的右边，我们有列表项文本。

之后，我们有了`Divider`组件。

将`variant`设置为`inset`，使其仅显示在文本下方。

`component`设置为`li`显示为`li`元素。

![](img/18503b380ac691546f6b9fd537360356.png)

艾拉·奥尔森在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以添加芯片来显示小块的文本、图像和图标。

此外，我们还可以展示隔断来分隔物品。