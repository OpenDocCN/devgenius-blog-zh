# 材料用户界面—表格

> 原文：<https://blog.devgenius.io/material-ui-tables-3fbd4ca890b1?source=collection_archive---------2----------------------->

![](img/233f0e131327ab1653ed022c1edcf401.png)

照片由 [Manja Vitolic](https://unsplash.com/@madhatterzone?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

材质 UI 是一个为 React 制作的材质设计库。

这是一组具有材质设计风格的 React 组件。

在本文中，我们将看看如何添加带有材质 UI 的表格。

# 密集表

我们可以把桌子的`size`道具改成`small`让桌子更密。

例如，我们可以写:

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
      <Table size="small">
        <TableHead>
          <TableRow>
            <TableCell>first name</TableCell>
            <TableCell>last name</TableCell>
            <TableCell align="right">age</TableCell>
          </TableRow>
        </TableHead>
        <TableBody>
          {rows.map(row => (
            <TableRow key={row.firstName}>
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

然后，我们以比默认方式更密集的方式显示表格行。

# 分类和选择

我们可以用`TableSortLabel`组件添加排序。

例如，我们可以写:

```
import React from "react";
import PropTypes from "prop-types";
import clsx from "clsx";
import { lighten, makeStyles } from "[@material](http://twitter.com/material)-ui/core/styles";
import Table from "[@material](http://twitter.com/material)-ui/core/Table";
import TableBody from "[@material](http://twitter.com/material)-ui/core/TableBody";
import TableCell from "[@material](http://twitter.com/material)-ui/core/TableCell";
import TableContainer from "[@material](http://twitter.com/material)-ui/core/TableContainer";
import TableHead from "[@material](http://twitter.com/material)-ui/core/TableHead";
import TablePagination from "[@material](http://twitter.com/material)-ui/core/TablePagination";
import TableRow from "[@material](http://twitter.com/material)-ui/core/TableRow";
import TableSortLabel from "[@material](http://twitter.com/material)-ui/core/TableSortLabel";
import Toolbar from "[@material](http://twitter.com/material)-ui/core/Toolbar";
import Typography from "[@material](http://twitter.com/material)-ui/core/Typography";
import Paper from "[@material](http://twitter.com/material)-ui/core/Paper";
import Checkbox from "[@material](http://twitter.com/material)-ui/core/Checkbox";
import IconButton from "[@material](http://twitter.com/material)-ui/core/IconButton";
import Tooltip from "[@material](http://twitter.com/material)-ui/core/Tooltip";import DeleteIcon from "[@material](http://twitter.com/material)-ui/icons/Delete";
import FilterListIcon from "[@material](http://twitter.com/material)-ui/icons/FilterList";function createData(name, age) {
  return { name, age };
}const rows = [
  createData("james", 30),
  createData("mary", 45),
  createData("alex", 26)
];function descendingComparator(a, b, orderBy) {
  if (b[orderBy] < a[orderBy]) {
    return -1;
  }
  if (b[orderBy] > a[orderBy]) {
    return 1;
  }
  return 0;
}function getComparator(order, orderBy) {
  return order === "desc"
    ? (a, b) => descendingComparator(a, b, orderBy)
    : (a, b) => -descendingComparator(a, b, orderBy);
}function stableSort(array, comparator) {
  const stabilizedThis = array.map((el, index) => [el, index]);
  stabilizedThis.sort((a, b) => {
    const order = comparator(a[0], b[0]);
    if (order !== 0) return order;
    return a[1] - b[1];
  });
  return stabilizedThis.map(el => el[0]);
}const headCells = [
  {
    id: "name",
    label: "name"
  },
  { id: "age", numeric: true, label: "age" }
];function EnhancedTableHead(props) {
  const {
    classes,
    onSelectAllClick,
    order,
    orderBy,
    numSelected,
    rowCount,
    onRequestSort
  } = props;
  const createSortHandler = property => event => {
    onRequestSort(event, property);
  }; return (
    <TableHead>
      <TableRow>
        <TableCell padding="checkbox">
          <Checkbox
            indeterminate={numSelected > 0 && numSelected < rowCount}
            checked={rowCount > 0 && numSelected === rowCount}
            onChange={onSelectAllClick}
            inputProps={{ "aria-label": "select all desserts" }}
          />
        </TableCell>
        {headCells.map(headCell => (
          <TableCell
            key={headCell.id}
            align={headCell.numeric ? "right" : "left"}
            padding={headCell.disablePadding ? "none" : "default"}
            sortDirection={orderBy === headCell.id ? order : false}
          >
            <TableSortLabel
              active={orderBy === headCell.id}
              direction={orderBy === headCell.id ? order : "asc"}
              onClick={createSortHandler(headCell.id)}
            >
              {headCell.label}
              {orderBy === headCell.id ? (
                <span className={classes.visuallyHidden}>
                  {order === "desc" ? "sorted descending" : "sorted ascending"}
                </span>
              ) : null}
            </TableSortLabel>
          </TableCell>
        ))}
      </TableRow>
    </TableHead>
  );
}EnhancedTableHead.propTypes = {
  classes: PropTypes.object.isRequired,
  numSelected: PropTypes.number.isRequired,
  onRequestSort: PropTypes.func.isRequired,
  onSelectAllClick: PropTypes.func.isRequired,
  order: PropTypes.oneOf(["asc", "desc"]).isRequired,
  orderBy: PropTypes.string.isRequired,
  rowCount: PropTypes.number.isRequired
};const useToolbarStyles = makeStyles(theme => ({
  root: {
    paddingLeft: theme.spacing(2),
    paddingRight: theme.spacing(1)
  },
  highlight:
    theme.palette.type === "light"
      ? {
          color: theme.palette.secondary.main,
          backgroundColor: lighten(theme.palette.secondary.light, 0.85)
        }
      : {
          color: theme.palette.text.primary,
          backgroundColor: theme.palette.secondary.dark
        },
  title: {
    flex: "1 1 100%"
  }
}));const EnhancedTableToolbar = props => {
  const classes = useToolbarStyles();
  const { numSelected } = props;return (
    <Toolbar
      className={clsx(classes.root, {
        [classes.highlight]: numSelected > 0
      })}
    >
      {numSelected > 0 ? (
        <Typography
          className={classes.title}
          color="inherit"
          variant="subtitle1"
          component="div"
        >
          {numSelected} selected
        </Typography>
      ) : (
        <Typography
          className={classes.title}
          variant="h6"
          id="tableTitle"
          component="div"
        >
          Names
        </Typography>
      )}{numSelected > 0 ? (
        <Tooltip title="Delete">
          <IconButton aria-label="delete">
            <DeleteIcon />
          </IconButton>
        </Tooltip>
      ) : (
        <Tooltip title="Filter list">
          <IconButton aria-label="filter list">
            <FilterListIcon />
          </IconButton>
        </Tooltip>
      )}
    </Toolbar>
  );
};EnhancedTableToolbar.propTypes = {
  numSelected: PropTypes.number.isRequired
};const useStyles = makeStyles(theme => ({
  root: {
    width: "100%"
  },
  paper: {
    width: "100%",
    marginBottom: theme.spacing(2)
  },
  table: {
    minWidth: 750
  },
  visuallyHidden: {
    border: 0,
    clip: "rect(0 0 0 0)",
    height: 1,
    margin: -1,
    overflow: "hidden",
    padding: 0,
    position: "absolute",
    top: 20,
    width: 1
  }
}));export default function App() {
  const classes = useStyles();
  const [order, setOrder] = React.useState("asc");
  const [orderBy, setOrderBy] = React.useState("calories");
  const [selected, setSelected] = React.useState([]);
  const [page, setPage] = React.useState(0);
  const [rowsPerPage, setRowsPerPage] = React.useState(5); const handleRequestSort = (event, property) => {
    const isAsc = orderBy === property && order === "asc";
    setOrder(isAsc ? "desc" : "asc");
    setOrderBy(property);
  }; const handleSelectAllClick = event => {
    if (event.target.checked) {
      const newSelecteds = rows.map(n => n.name);
      setSelected(newSelecteds);
      return;
    }
    setSelected([]);
  }; const handleClick = (event, name) => {
    const selectedIndex = selected.indexOf(name);
    let newSelected = []; if (selectedIndex === -1) {
      newSelected = newSelected.concat(selected, name);
    } else if (selectedIndex === 0) {
      newSelected = newSelected.concat(selected.slice(1));
    } else if (selectedIndex === selected.length - 1) {
      newSelected = newSelected.concat(selected.slice(0, -1));
    } else if (selectedIndex > 0) {
      newSelected = newSelected.concat(
        selected.slice(0, selectedIndex),
        selected.slice(selectedIndex + 1)
      );
    } setSelected(newSelected);
  }; const handleChangePage = (event, newPage) => {
    setPage(newPage);
  }; const handleChangeRowsPerPage = event => {
    setRowsPerPage(parseInt(event.target.value, 10));
    setPage(0);
  }; const isSelected = name => selected.indexOf(name) !== -1; const emptyRows =
    rowsPerPage - Math.min(rowsPerPage, rows.length - page * rowsPerPage); return (
    <div className={classes.root}>
      <Paper className={classes.paper}>
        <EnhancedTableToolbar numSelected={selected.length} />
        <TableContainer>
          <Table className={classes.table}>
            <EnhancedTableHead
              classes={classes}
              numSelected={selected.length}
              order={order}
              orderBy={orderBy}
              onSelectAllClick={handleSelectAllClick}
              onRequestSort={handleRequestSort}
              rowCount={rows.length}
            />
            <TableBody>
              {stableSort(rows, getComparator(order, orderBy))
                .slice(page * rowsPerPage, page * rowsPerPage + rowsPerPage)
                .map((row, index) => {
                  const isItemSelected = isSelected(row.name);return (
                    <TableRow
                      hover
                      onClick={event => handleClick(event, row.name)}
                      role="checkbox"
                      aria-checked={isItemSelected}
                      tabIndex={-1}
                      key={row.name}
                      selected={isItemSelected}
                    >
                      <TableCell padding="checkbox">
                        <Checkbox checked={isItemSelected} />
                      </TableCell>
                      <TableCell component="th" scope="row" padding="none">
                        {row.name}
                      </TableCell>
                      <TableCell align="right">{row.age}</TableCell>
                    </TableRow>
                  );
                })}
              {emptyRows > 0 && (
                <TableRow>
                  <TableCell />
                </TableRow>
              )}
            </TableBody>
          </Table>
        </TableContainer>
        <TablePagination
          rowsPerPageOptions={[5, 10, 25]}
          component="div"
          count={rows.length}
          rowsPerPage={rowsPerPage}
          page={page}
          onChangePage={handleChangePage}
          onChangeRowsPerPage={handleChangeRowsPerPage}
        />
      </Paper>
    </div>
  );
}
```

添加带有排序和选择的表格。

我们从[https://material-ui . com/components/tables/# sorting-amp-selecting](https://material-ui.com/components/tables/#sorting-amp-selecting)中的例子开始。

我们有`stableSort`函数来进行排序。

我们在行上有一个复选框，显示让我们选择项目。

我们还在底部添加了一个工具栏，让我们选择用`TablePagination`组件显示的行数。

`EnhancedTableHead`有带排序的表头。

我们使用了`TableSortLabel`来进行排序。

![](img/f51fd2be439c5a1767f4a1d286f4eace.png)

[乔·凯恩](https://unsplash.com/@joeyc?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

我们可以添加密集表和带有内置组件排序的表。