# 如何实现 React 表项目设置和使用表

> 原文：<https://blog.devgenius.io/how-to-implement-react-table-project-setup-and-usetable-d8e840bc3932?source=collection_archive---------6----------------------->

![](img/d3cf734db6c1fe5edd2fcee5cee26d29.png)

在本文中，我们将学习如何在我们的项目中实现 react table，并了解每一件事。

因此，让我们在系统中安装 react。

```
npx create-react-app rtdemo
cd rtdemo
npm start
```

我们将使用[这个](https://react-table.tanstack.com/)模块来显示表格。React Table 是一个钩子集合，用于构建强大的表和数据网格体验。这些挂钩是轻量级的、可组合的、超可扩展的，但是不为您呈现任何标记或样式。这实际上意味着 React Table 是一个“无头”UI 库。

React Table 是一个无头工具，这意味着开箱即用，它不呈现或提供任何实际的 UI 元素。您负责利用该库提供的钩子的状态和回调来呈现您自己的表 0markup。

现在安装 react 表模块。React Table 与 React v16.8+兼容，可与 ReactDOM 和 React Native 配合使用。

```
npm install react-table --save
```

编辑 APP.js 文件并添加以下代码。

```
import React, { useState, useEffect, useMemo } from "react";import { useTable } from "react-table";const App = (props) => {const [tutorials, setTutorials] = useState([]);const retrieveTutorials = () => {const { tutorials, totalPages } = {tutorials: [{invoice_date: "18 Oct 2021",company: "ABC Enterprise",invoice_no: "INV/ABC/21-22/109",},{invoice_date: "19 Oct 2021",company: "ABC Enterprise",invoice_no: "INV/ABC/21-22/109",},{invoice_date: "20 Oct 2021",company: "ABC Enterprise",invoice_no: "INV/ABC/21-22/109",},{invoice_date: "21 Oct 2021",company: "ABC Enterprise",invoice_no: "INV/ABC/21-22/109",},{invoice_date: "22 Oct 2021",company: "ABC Enterprise",invoice_no: "INV/ABC/21-22/109",},{invoice_date: "23 Oct 2021",company: "ABC Enterprise",invoice_no: "INV/ABC/21-22/109",},],totalPages: 1,};setTutorials(tutorials);};useEffect(retrieveTutorials, []);const columns = useMemo(() => [{Header: "Date",accessor: "invoice_date",},{Header: "Vendor",accessor: "company",},{Header: "Invoice No.",accessor: "invoice_no",},],[]);const {getTableProps,getTableBodyProps,headerGroups,rows,prepareRow,} = useTable({columns,data: tutorials,});return (<div><table {...getTableProps()}><thead>{headerGroups.map((headerGroup) => (<tr {...headerGroup.getHeaderGroupProps()}>{headerGroup.headers.map((column) => (<th {...column.getHeaderProps()}>{column.render("Header")}</th>))}</tr>))}</thead>{tutorials.length > 0 ? (<tbody {...getTableBodyProps()}>{rows.map((row, i) => {prepareRow(row);return (<tr {...row.getRowProps()}>{row.cells.map((cell) => {return (<td {...cell.getCellProps()}>{cell.render("Cell")}</td>);})}</tr>);})}</tbody>) : (<tbody><tr><td colSpan="8"><figure className="noRecord-found"></figure><span className="norecord-text">No records found</span></td></tr></tbody>)}</table></div>);};export default App;
```

这里我们使用了 useState，useEffect，useMemo 钩子。

useState:useState 挂钩是一个特殊的函数**，它将初始状态作为一个参数，并返回一个包含两个条目的数组**。语法:第一个元素是初始状态，第二个元素是用于更新状态的函数。…函数返回的值将被用作初始状态。

use effect:use effect 是做什么的？通过使用这个钩子，**你告诉 React 你的组件需要在 render** 之后做一些事情。React 将记住您传递的函数(我们称之为“效果”)，并在执行 DOM 更新后调用它。

useMemo : React 有一个名为 useMemo 的内置钩子，**允许你记忆昂贵的函数，这样你就可以避免在每次渲染时调用它们**。您只需传入一个函数和一个输入数组，useMemo 只会在其中一个输入发生变化时重新计算记忆的值。

useTable : useTable 是 React 表的根钩子**。要使用它，请传递一个 options 对象，该对象至少包含一个列和数据值，后跟您想要使用的任何 React 表兼容挂钩。**

getTableProps:这个函数用于解析你的表包装器需要的任何属性。

getTableBodyProps:这个函数用于解析表体包装器所需的任何属性。

headerGroups:规范化标题组的数组，每个标题组包含该行的最终列对象的扁平数组。

rows:从原始的`data`数组和`columns`传递到表选项中的**物化行对象**的数组

prepareRow:这个函数负责延迟准备要呈现的行。您打算在表中呈现的任何行都需要在每次呈现之前传递给这个函数**。**

*   **为什么？**由于表数据可能非常大，所以无论是否实际呈现，为要呈现的每一行计算所有必要的状态都会变得非常昂贵(例如，如果您正在对行进行分页或虚拟化，则在任何给定时刻您可能只有几行可见)。此函数只允许计算您想要显示的行，并准备好正确的状态。

所以这是关于使用 useTable 实现 React Table 的基础。我希望你已经理解了这个例子。

你也可以看看 YouTube 频道:【https://www.youtube.com/channel/UCOWT2JvRnSMVSboxvccZBFQ 

*更多内容尽在*[*blog . devgenius . io*](http://blog.devgenius.io)*。*