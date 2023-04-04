# 材料用户界面—虚拟化列表和表格

> 原文：<https://blog.devgenius.io/material-ui-virtualized-lists-and-tables-2f5887547ccf?source=collection_archive---------2----------------------->

![](img/5bfcbaab057161ebecf8a8239cfe249d.png)

照片由[塞巴斯蒂安·桑塔克鲁斯](https://unsplash.com/@sebassanta?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

材质 UI 是一个为 React 制作的材质设计库。

这是一组具有材质设计风格的 React 组件。

在本文中，我们将看看如何添加带有固定标题的列表、虚拟化列表和带有材料 UI 的表格。

# 固定副标题列表

我们可以使用 CSS 将副标题固定在屏幕顶部，直到它被下一个副标题推开。

例如，我们可以写:

```
import React from "react";
import { makeStyles } from "[@material](http://twitter.com/material)-ui/core/styles";
import List from "[@material](http://twitter.com/material)-ui/core/List";
import ListItem from "[@material](http://twitter.com/material)-ui/core/ListItem";
import ListItemText from "[@material](http://twitter.com/material)-ui/core/ListItemText";
import ListSubheader from "[@material](http://twitter.com/material)-ui/core/ListSubheader";const useStyles = makeStyles(theme => ({
  root: {
    width: "100%",
    maxWidth: 360,
    backgroundColor: theme.palette.background.paper,
    position: "relative",
    overflow: "auto",
    maxHeight: 300
  },
  listSection: {
    backgroundColor: "inherit"
  },
  ul: {
    backgroundColor: "inherit",
    padding: 0
  }
}));export default function App() {
  const classes = useStyles(); return (
    <List className={classes.root} subheader={<li />}>
      {Array(10)
        .fill()
        .map((s, sectionId) => (
          <li key={`section-${sectionId}`} className={classes.listSection}>
            <ul className={classes.ul}>
              <ListSubheader>{`I'm sticky ${sectionId}`}</ListSubheader>
              {[0, 1, 2].map(item => (
                <ListItem key={`item-${sectionId}-${item}`}>
                  <ListItemText primary={`Item ${item}`} />
                </ListItem>
              ))}
            </ul>
          </li>
        ))}
    </List>
  );
}
```

我们用`makeStyles`函数将根的位置设置为`relative`。

此外，我们设置了一个`maxHeight`,这样当内容溢出列表时它就会滚动。

这样，我们设置副标题保持固定。

# 插图列表

我们可以让没有图标的列表项与有图标的列表项对齐。

例如，我们可以写:

```
import React from "react";
import List from "[@material](http://twitter.com/material)-ui/core/List";
import ListItem from "[@material](http://twitter.com/material)-ui/core/ListItem";
import ListItemIcon from "[@material](http://twitter.com/material)-ui/core/ListItemIcon";
import ListItemText from "[@material](http://twitter.com/material)-ui/core/ListItemText";
import StarIcon from "[@material](http://twitter.com/material)-ui/icons/Star";export default function App() {
  return (
    <List aria-label="contacts">
      <ListItem button>
        <ListItemIcon>
          <StarIcon />
        </ListItemIcon>
        <ListItemText primary="james smith" />
      </ListItem>
      <ListItem button>
        <ListItemText inset primary="mary jane" />
      </ListItem>
    </List>
  );
}
```

我们添加了`inset`属性，使第二个列表项中的文本与第一个列表项中的文本对齐。

# 虚拟化列表

如果我们有一个包含很多条目的列表，那么我们必须创建一个虚拟化的列表。

为此，我们可以写:

```
import React from "react";
import ListItem from "[@material](http://twitter.com/material)-ui/core/ListItem";
import ListItemText from "[@material](http://twitter.com/material)-ui/core/ListItemText";
import { FixedSizeList } from "react-window";function renderRow(props) {
  const { index, style } = props; return (
    <ListItem button style={style} key={index}>
      <ListItemText primary={`Item ${index + 1}`} />
    </ListItem>
  );
}export default function App() {
  return (
    <FixedSizeList height={300} width={300} itemSize={40} itemCount={300}>
      {renderRow}
    </FixedSizeList>
  );
}
```

用 react-window 创建虚拟化列表。

我们用`renderRow`来呈现一个表格行。

我们使用了`FixedSizedList`而不是普通的`List`。

`itemCount`设置项目计数。

`itemSize`有一件物品的大小。

`height`和`width`是列表的高度和宽度。

# 桌子

Material UI 附带了一个表格组件，让我们可以在表格中显示数据。

要创建一个简单的，我们可以写如下:

```
import React from "react";
import Table from "[@material](http://twitter.com/material)-ui/core/Table";
import TableBody from "[@material](http://twitter.com/material)-ui/core/TableBody";
import TableCell from "[@material](http://twitter.com/material)-ui/core/TableCell";
import TableContainer from "[@material](http://twitter.com/material)-ui/core/TableContainer";
import TableHead from "[@material](http://twitter.com/material)-ui/core/TableHead";
import TableRow from "[@material](http://twitter.com/material)-ui/core/TableRow";
import Paper from "[@material](http://twitter.com/material)-ui/core/Paper";function createData(firstName, lastName, age) {
  return { firstName, lastName, age };
}const rows = [
  createData("james", "smith", 20),
  createData("mary", "jones", 30),
  createData("may", "wong", 30)
];export default function App() {
  return (
    <TableContainer component={Paper}>
      <Table>
        <TableHead>
          <TableRow>
            <TableCell>first name</TableCell>
            <TableCell>last name</TableCell>
            <TableCell align="right">age</TableCell>
          </TableRow>
        </TableHead>
        <TableBody>
          {rows.map(row => (
            <TableRow key={row.name}>
              <TableCell component="th" scope="row">
                {row.firstName}
              </TableCell>
              <TableCell>{row.lastName}</TableCell>
              <TableCell align="right">{row.age}</TableCell>
            </TableRow>
          ))}
        </TableBody>
      </Table>
    </TableContainer>
  );
}
```

我们创建了一个包含`TableContainer`、`Table`、`TableHead`、`TableRow`和`TableCell`组件的表格。

`TableContainer`有容器。

`Tabke`有表本身。

`TableHead`保存标题。

`TableRow`有行。

`TableCell`有细胞。

我们可以将单元格的`align`属性设置为`right`来使内容右对齐。

此外，如果我们愿意，我们可以将`component`道具设置为显示为`th`。

将`scope`设置为行，表示该单元格是用于行的，我们得到的单元格格式与标题不同。

![](img/12d1ddba90c7a6de9ba0963d142553d3.png)

照片由[亚历山德罗·阿里蒙蒂](https://unsplash.com/@alessandro_alimonti?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 结论

如果我们的列表有很多条目，我们可以创建虚拟列表。

列表标题可以被钉住。

表格组件是核心材料 UI 包的一部分。