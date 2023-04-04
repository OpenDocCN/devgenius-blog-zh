# vue-好表-事件

> 原文：<https://blog.devgenius.io/vue-good-table-events-477689e08f04?source=collection_archive---------4----------------------->

![](img/6f712c543e879016b0e7813fa3720367.png)

[Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的[天线](https://unsplash.com/@antenna?utm_source=medium&utm_medium=referral)拍照

从头开始创建表是一件痛苦的事情。

这也是为什么有很多表格插件让我们轻松添加表格的原因。

在本文中，我们将了解如何使用 vue-good-table 插件将表格添加到 Vue 应用程序中。

# 表事件

用`vue-good-table`组件创建的表会发出各种各样的事件。

当它们被发射时，我们可以听它们运行我们想要的。

其中之一就是`on-row-click`事件。

要听它，我们可以写:

```
<template>
  <div>
    <vue-good-table :columns="columns" :rows="rows" [@on](http://twitter.com/on)-row-click="onRowClick"/>
  </div>
</template><script>
export default {
  data() {
    return {
      columns: [
        {
          label: "Name",
          field: "name"
        },
        {
          label: "Age",
          field: "age",
          type: "number"
        },
        {
          label: "Birthday",
          field: "birthDay",
          type: "date",
          dateInputFormat: "yyyy-MM-dd",
          dateOutputFormat: "MMM do yy"
        }
      ],
      rows: [
        { id: 1, name: "John", age: 20, birthDay: "2000-01-20" },
        { id: 2, name: "Jane", age: 24, birthDay: "1996-10-31" },
        { id: 3, name: "Susan", age: 16, birthDay: "2004-10-30" }
      ]
    };
  },
  methods: {
    onRowClick(params) {
      console.log(params);
    }
  }
};
</script>
```

我们用`onRowClick`方法听`on-row-click`事件。

`params`参数是一个具有各种属性的对象。

它具有`row`、`pageIndex`、`selected`和`event`属性。

`row`是带有行的对象。

`pageIndex`有索引的行。

`selected`是一个布尔值，表示表格行是否被选中。

`event`具有通过单击行创建的`MouseEvent`对象。

我们可以用`on-row-dblclick`事件检测一行上的双击。

例如，我们可以这样来听:

```
<template>
  <div>
    <vue-good-table :columns="columns" :rows="rows" [@on](http://twitter.com/on)-row-dblclick="onRowDoubleClick"/>
  </div>
</template><script>
export default {
  data() {
    return {
      columns: [
        {
          label: "Name",
          field: "name"
        },
        {
          label: "Age",
          field: "age",
          type: "number"
        },
        {
          label: "Birthday",
          field: "birthDay",
          type: "date",
          dateInputFormat: "yyyy-MM-dd",
          dateOutputFormat: "MMM do yy"
        }
      ],
      rows: [
        { id: 1, name: "John", age: 20, birthDay: "2000-01-20" },
        { id: 2, name: "Jane", age: 24, birthDay: "1996-10-31" },
        { id: 3, name: "Susan", age: 16, birthDay: "2004-10-30" }
      ]
    };
  },
  methods: {
    onRowDoubleClick(params) {
      console.log(params);
    }
  }
};
</script>
```

`params`与`onRowClick`方法中的属性相同。

我们可以通过监听`on-cell-click`事件来检测细胞点击。

例如，我们可以通过写下来听它:

```
<template>
  <div>
    <vue-good-table :columns="columns" :rows="rows" [@on](http://twitter.com/on)-cell-click="onCellClick"/>
  </div>
</template><script>
export default {
  data() {
    return {
      columns: [
        {
          label: "Name",
          field: "name"
        },
        {
          label: "Age",
          field: "age",
          type: "number"
        },
        {
          label: "Birthday",
          field: "birthDay",
          type: "date",
          dateInputFormat: "yyyy-MM-dd",
          dateOutputFormat: "MMM do yy"
        }
      ],
      rows: [
        { id: 1, name: "John", age: 20, birthDay: "2000-01-20" },
        { id: 2, name: "Jane", age: 24, birthDay: "1996-10-31" },
        { id: 3, name: "Susan", age: 16, birthDay: "2004-10-30" }
      ]
    };
  },
  methods: {
    onCellClick(params) {
      console.log(params);
    }
  }
};
</script>
```

`params`对象与其他处理程序方法中的对象略有不同。

它具有`row`、`column`、`rowIndex`和`event`属性。

`row`和`event`与其他`params`对象相同。

`column`是被点击的列。

`rowIndex`有被点击行的索引。

# 结论

我们可以用 vue-good-table 插件监听桌子上触发的各种事件。