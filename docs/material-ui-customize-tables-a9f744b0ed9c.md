# 材料用户界面—自定义表格

> 原文：<https://blog.devgenius.io/material-ui-customize-tables-a9f744b0ed9c?source=collection_archive---------0----------------------->

![](img/b9cd9a3e7abdd076204ee6dc5e1b75aa.png)

丽贝卡·马修斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

材质 UI 是一个为 React 制作的材质设计库。

这是一组具有材质设计风格的 React 组件。

在本文中，我们将看看如何添加带有材质 UI 的表格。

# 定制表格

我们可以用`withStyles`函数定制表格组件的样式。

例如，我们可以写:

```
import React from "react";
import { withStyles } from "[@material](http://twitter.com/material)-ui/core/styles";
import Table from "[@material](http://twitter.com/material)-ui/core/Table";
import TableBody from "[@material](http://twitter.com/material)-ui/core/TableBody";
import TableCell from "[@material](http://twitter.com/material)-ui/core/TableCell";
import TableContainer from "[@material](http://twitter.com/material)-ui/core/TableContainer";
import TableHead from "[@material](http://twitter.com/material)-ui/core/TableHead";
import TableRow from "[@material](http://twitter.com/material)-ui/core/TableRow";
import Paper from "[@material](http://twitter.com/material)-ui/core/Paper";const StyledTableCell = withStyles(theme => ({
  head: {
    backgroundColor: theme.palette.common.black,
    color: theme.palette.common.white
  },
  body: {
    fontSize: 12
  }
}))(TableCell);const StyledTableRow = withStyles(theme => ({
  root: {
    "&:nth-of-type(odd)": {
      backgroundColor: theme.palette.action.hover
    }
  }
}))(TableRow);function createData(name, age) {
  return { name, age };
}const rows = [createData("james", 15), createData("mary", 23)];export default function App() {
  return (
    <TableContainer component={Paper}>
      <Table>
        <TableHead>
          <TableRow>
            <StyledTableCell>name</StyledTableCell>
            <StyledTableCell align="right">age</StyledTableCell>
          </TableRow>
        </TableHead>
        <TableBody>
          {rows.map(row => (
            <StyledTableRow key={row.name}>
              <StyledTableCell component="th" scope="row">
                {row.name}
              </StyledTableCell>
              <StyledTableCell align="right">{row.age}</StyledTableCell>
            </StyledTableRow>
          ))}
        </TableBody>
      </Table>
    </TableContainer>
  );
}
```

我们创建了自己的表格单元格和表格行组件。

我们所做的就是将所有的样式传递给`withStyles`函数。

然后我们把它们都放在`App`组件中。

# 固定标题

我们可以用`stickyHeader`支柱固定割台。

例如，我们可以写:

```
import React from "react";
import Table from "[@material](http://twitter.com/material)-ui/core/Table";
import TableBody from "[@material](http://twitter.com/material)-ui/core/TableBody";
import TableCell from "[@material](http://twitter.com/material)-ui/core/TableCell";
import TableContainer from "[@material](http://twitter.com/material)-ui/core/TableContainer";
import TableHead from "[@material](http://twitter.com/material)-ui/core/TableHead";
import TableRow from "[@material](http://twitter.com/material)-ui/core/TableRow";
import Paper from "[@material](http://twitter.com/material)-ui/core/Paper";
import { makeStyles } from "[@material](http://twitter.com/material)-ui/core/styles";const useStyles = makeStyles({
  container: {
    maxHeight: 200
  }
});function createData(name, age) {
  return { name, age };
}const rows = [
  createData("james", 15),
  createData("mary", 15),
  createData("alex", 15),
  createData("may", 15),
  createData("sam", 15),
  createData("john", 15),
  createData("mary", 23)
];export default function App() {
  const classes = useStyles(); return (
    <TableContainer component={Paper} className={classes.container}>
      <Table stickyHeader>
        <TableHead>
          <TableRow>
            <TableCell>name</TableCell>
            <TableCell align="right">age</TableCell>
          </TableRow>
        </TableHead>
        <TableBody>
          {rows.map(row => (
            <TableRow key={row.name}>
              <TableCell component="th" scope="row">
                {row.name}
              </TableCell>
              <TableCell align="right">{row.age}</TableCell>
            </TableRow>
          ))}
        </TableBody>
      </Table>
    </TableContainer>
  );
}
```

我们用`makeStyles`函数来设计表格的高度。

这样，我们得到一个粘性头。

然后我们为表创建行。

然后为了让桌子有粘性，我们用`stickyHeader`支撑`Table`。

现在，当我们滚动内容时，我们会看到标题停留在顶部。

# 可折叠桌子

我们可以用带有`TableRow`组件的可折叠列表制作一个表格。

在它里面，我们添加了带有`Collapse`组件的`TableCell`。

为此，我们可以写:

```
import React from "react";
import PropTypes from "prop-types";
import { makeStyles } from "[@material](http://twitter.com/material)-ui/core/styles";
import Box from "[@material](http://twitter.com/material)-ui/core/Box";
import Collapse from "[@material](http://twitter.com/material)-ui/core/Collapse";
import IconButton from "[@material](http://twitter.com/material)-ui/core/IconButton";
import Table from "[@material](http://twitter.com/material)-ui/core/Table";
import TableBody from "[@material](http://twitter.com/material)-ui/core/TableBody";
import TableCell from "[@material](http://twitter.com/material)-ui/core/TableCell";
import TableContainer from "[@material](http://twitter.com/material)-ui/core/TableContainer";
import TableHead from "[@material](http://twitter.com/material)-ui/core/TableHead";
import TableRow from "[@material](http://twitter.com/material)-ui/core/TableRow";
import Typography from "[@material](http://twitter.com/material)-ui/core/Typography";
import Paper from "[@material](http://twitter.com/material)-ui/core/Paper";
import KeyboardArrowDownIcon from "[@material](http://twitter.com/material)-ui/icons/KeyboardArrowDown";
import KeyboardArrowUpIcon from "[@material](http://twitter.com/material)-ui/icons/KeyboardArrowUp";function createData(name, age, history) {
  return { name, age, history };
}const rows = [createData("james", 15, [{ date: "2020-01-01" }])];function Row(props) {
  const { row } = props;
  const [open, setOpen] = React.useState(false); return (
    <React.Fragment>
      <TableRow>
        <TableCell>
          <IconButton
            aria-label="expand row"
            size="small"
            onClick={() => setOpen(!open)}
          >
            {open ? <KeyboardArrowUpIcon /> : <KeyboardArrowDownIcon />}
          </IconButton>
        </TableCell>
        <TableCell component="th" scope="row">
          {row.name}
        </TableCell>
        <TableCell>{row.age}</TableCell>
      </TableRow>
      <TableRow>
        <TableCell style={{ paddingBottom: 0, paddingTop: 0 }} colSpan={6}>
          <Collapse in={open} timeout="auto" unmountOnExit>
            <Box margin={1}>
              <Typography variant="h6" gutterBottom component="div">
                History
              </Typography>
              <Table size="small">
                <TableHead>
                  <TableRow>
                    <TableCell>Date</TableCell>
                  </TableRow>
                </TableHead>
                <TableBody>
                  {row.history.map(historyRow => (
                    <TableRow key={historyRow.date}>
                      <TableCell component="th" scope="row">
                        {historyRow.date}
                      </TableCell>
                    </TableRow>
                  ))}
                </TableBody>
              </Table>
            </Box>
          </Collapse>
        </TableCell>
      </TableRow>
    </React.Fragment>
  );
}export default function App() {
  return (
    <TableContainer component={Paper}>
      <Table>
        <TableHead>
          <TableRow>
            <TableCell />
            <TableCell>name</TableCell>
            <TableCell>age</TableCell>
          </TableRow>
        </TableHead>
        <TableBody>
          {rows.map(row => (
            <Row key={row.name} row={row} />
          ))}
        </TableBody>
      </Table>
    </TableContainer>
  );
}
```

我们创建了一个`Row`组件，然后是`TableCell`。

在它里面，我们有`Collapse`组件，我们可以单击它来打开或关闭一个表。

在那里面，我们有一个`Table`，里面有更多的东西。

![](img/70e2f2c93f7d14daca97cbcb67e8957f.png)

照片由 [Ales Krivec](https://unsplash.com/@aleskrivec?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 结论

我们可以添加定制样式的表格组件。

此外，我们可以创建一个嵌套了另一个表格的表格，我们可以展开或折叠该表格。