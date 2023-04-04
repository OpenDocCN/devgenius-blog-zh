# vue-good-table —选项

> 原文：<https://blog.devgenius.io/vue-good-table-options-2a4c2424b5b2?source=collection_archive---------7----------------------->

![](img/6c91f993713662478770cca8ed8e4301.png)

由[àlex Rodriguez](https://unsplash.com/@alexabad?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

从头开始创建表是一件痛苦的事情。

这也是为什么有很多表格插件让我们轻松添加表格的原因。

在本文中，我们将了解如何使用 vue-good-table 插件将表格添加到 Vue 应用程序中。

# 更改选项

vue-good-table 插件为我们提供了许多定制表格的选项。

其中之一是行号选项。这让我们可以在每一行显示行号。

为了添加这个，我们添加了`line-numbers`道具:

```
<template>
  <div>
    <vue-good-table :columns="columns" :rows="rows" line-numbers/>
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
  }
};
</script>
```

可以通过添加一个`row-style-class`属性为每行添加一个 CSS 类来定制行样式。

例如，我们可以写:

```
<template>
  <div>
    <vue-good-table :columns="columns" :rows="rows" :row-style-class="rowStyleClassFn"/>
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
        { id: 2, name: "Jane", age: 24, birthDay: "2011-10-31" },
        { id: 3, name: "Susan", age: 16, birthDay: "2011-10-30" }
      ]
    };
  },
  methods: {
    rowStyleClassFn(row) {
      return row.age > 18 ? "green" : "red";
    }
  }
};
</script><style>
.green {
  background-color: green;
}.red {
  background-color: red;
}td {
  color: white !important;
}
</style>
```

我们将`rowStyleClassFn`功能设置为`row-style-class`道具。

然后我们可以根据类别应用我们想要的样式。

`rtl`道具让我们为表格启用从右到左的布局。

例如，我们可以写:

```
<template>
  <div>
    <vue-good-table :columns="columns" :rows="rows" rtl/>
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
  }
};
</script>
```

一旦我们添加了`rtl`道具，我们就翻转柱子。

# 结论

使用 vue-good-table，我们可以用各种道具更换桌子。