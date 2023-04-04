# vue-good-table —可折叠组和表格样式

> 原文：<https://blog.devgenius.io/vue-good-table-collapsible-groups-and-table-styling-bbed737867a5?source=collection_archive---------3----------------------->

![](img/41d739a1d9bb6a12efc489a1a691c1f5.png)

安妮·斯普拉特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

从头开始创建表是一件痛苦的事情。

这也是为什么有很多表格插件让我们轻松添加表格的原因。

在本文中，我们将了解如何使用 vue-good-table 插件将表格添加到 Vue 应用程序中。

# 可折叠组

我们可以通过改变`group-options`属性使表格组可折叠。

为此，我们可以写:

```
<template>
  <div>
    <vue-good-table
      :columns="columns"
      :rows="rows"
      :group-options="{
        enabled: true,   
        collapsable: true
      }"
    ></vue-good-table>
  </div>
</template>
<script>
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
        {
          mode: "span",
          label: "Humans",
          html: false,
          children: [
            { id: 1, name: "John", age: 20, birthDay: "2000-01-20" },
            { id: 2, name: "Jane", age: 24, birthDay: "1996-10-31" },
            { id: 3, name: "Mary", age: 16, birthDay: "2004-10-30" }
          ]
        }
      ]
    };
  }
};
</script>
```

我们将`collapsible`属性设置为`true`,使该组可折叠。

# 主题

我们可以改变桌子的主题来改变配色方案。

为此，我们可以添加`theme`道具:

```
<template>
  <div>
    <vue-good-table :columns="columns" :rows="rows" theme="black-rhino"></vue-good-table>
  </div>
</template>
<script>
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
  }
};
</script>
```

我们将`theme`属性设置为`'black-rhino'`，以黑色显示标题行，灰色显示单元格。

# 样式类别

我们还可以通过使用表格的选择器来设计表格的样式。

为此，我们可以添加`styleClass`属性来设置表的类名。

例如，我们可以写:

```
<template>
  <div>
    <vue-good-table :columns="columns" :rows="rows" styleClass="vgt-table"></vue-good-table>
  </div>
</template>
<script>
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
  }
};
</script><style>
.vgt-table {
  border: 1px solid red !important;
}
</style>
```

我们将`styleClass`设置为我们想要的表的类名。

然后我们可以添加`style`标签来设置表格的边框。

# 结论

我们可以为分组表格添加可折叠的行。

我们可以为我们的表添加样式类，使样式更容易。