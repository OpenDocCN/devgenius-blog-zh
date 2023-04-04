# vue-good-table —分页和排序事件

> 原文：<https://blog.devgenius.io/vue-good-table-pagination-and-sorting-events-915c6d6a321?source=collection_archive---------4----------------------->

![](img/1e49dfc5fef44a3081a7d9d3433f54ff.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

从头开始创建表是一件痛苦的事情。

这也是为什么有很多表格插件让我们轻松添加表格的原因。

在本文中，我们将了解如何使用 vue-good-table 插件将表格添加到 Vue 应用程序中。

# 表事件

我们可以监听由`vue-good-table`组件触发的各种事件。

其中之一是行鼠标离开事件。

要听它，我们可以听一下`on-row-mouseleave`事件:

```
<template>
  <div>
    <vue-good-table :columns="columns" :rows="rows" [@on](http://twitter.com/on)-row-mouseleave="onRowMouseleave"/>
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
    onRowMouseleave(row, pageIndex) {
      console.log(row, pageIndex);
    }
  }
};
</script>
```

`row`是一个具有行属性的对象。

`pageIndex`是鼠标离开的行的索引。

`on-page-change`事件让我们监听页面改变事件。

例如，我们可以写:

```
<template>
  <div>
    <vue-good-table
      :columns="columns"
      :rows="rows"
      [@on](http://twitter.com/on)-page-change="onPageChange"
      :pagination-options="{
        enabled: true,        
        perPage: 5,
      }"
    />
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
        { id: 3, name: "Mary", age: 16, birthDay: "2004-10-30" },
        { id: 4, name: "Sue", age: 16, birthDay: "2004-1-30" },
        { id: 5, name: "Alex", age: 16, birthDay: "2004-3-30" },
        { id: 6, name: "Susan", age: 16, birthDay: "2004-5-30" }
      ]
    };
  },
  methods: {
    onPageChange(params) {
      console.log(params);
    }
  }
};
</script>
```

我们添加一个带有`pagination-options`属性的表，为我们的表添加分页。

`enabled`让我们启用分页。

`perPage`让我们设置每页显示多少行。

使用`params`对象将`on-page-change`指令设置为`onPageChange`方法。

该对象具有`currentPage`、`currentPerPage`、`total`和`prevPage`属性。

`currentPage`有当前页码。

`currentPerPage`有要显示的行数。

`total`表中已有项目的总数。

`prevPage`有上一页的页码。

同样，我们可以通过监听`on-sort-change`事件来监听表排序的变化。

例如，我们可以写:

```
<template>
  <div>
    <vue-good-table :columns="columns" :rows="rows" [@on](http://twitter.com/on)-sort-change="onSortChange"/>
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
        { id: 3, name: "Mary", age: 16, birthDay: "2004-10-30" }
      ]
    };
  },
  methods: {
    onSortChange(params) {
      console.log(params);
    }
  }
};
</script>
```

去听这个事件。

`params`具有`field`和`type`属性。

`field`具有我们正在排序的属性名。

`type`有我们正在排序的方向。

# 结论

我们可以监听从`vue-good-table`组件发出的分页和排序事件。