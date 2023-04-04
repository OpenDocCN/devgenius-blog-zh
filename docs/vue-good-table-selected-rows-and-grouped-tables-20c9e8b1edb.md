# vue-good-table —选定的行和分组的表格

> 原文：<https://blog.devgenius.io/vue-good-table-selected-rows-and-grouped-tables-20c9e8b1edb?source=collection_archive---------0----------------------->

![](img/e3f51ad7eabff1aa20fbdea726f665b4.png)

图为 [Duy Pham](https://unsplash.com/@miinyuii?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

从头开始创建表是一件痛苦的事情。

这也是为什么有很多表格插件让我们轻松添加表格的原因。

在本文中，我们将了解如何使用 vue-good-table 插件将表格添加到 Vue 应用程序中。

# 选定行操作槽

我们可以填充`selected-row-actions`槽，这样我们可以添加自己的内容，以便在选择表格行时显示。

为此，我们可以写:

```
<template>
  <div>
    <vue-good-table :columns="columns" :rows="rows" :select-options="{ 
    enabled: true,
  }">
      <div slot="selected-row-actions">
        <button>Action 1</button>
      </div>
    </vue-good-table>
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
        },
        {
          label: "Before",
          field: "before"
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

向容器中添加显示选定行数的按钮。

# 分组表

我们可以用`group-options`属性添加一个分组表。

例如，我们可以写:

```
<template>
  <div>
    <vue-good-table :columns="columns" :rows="rows" :group-options="{
    enabled: true
  }"></vue-good-table>
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
        },
        {
          label: "Before",
          field: "before"
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

通过将`rows`数组更改为具有各种属性的对象来添加组。

`mode`被设置为`'span'`，这意味着标题将跨越所有列。

`labels`是显示在标题中的标签。

`html`设置为`true`意味着标签将被呈现为 HTML。

标题将显示在组的上方。

我们可以用`table-header-row`槽定制航向。

例如，我们可以写:

```
<template>
  <div>
    <vue-good-table
      :columns="columns"
      :rows="rows"
      :group-options="{
        enabled: true,   
        headerPosition: 'top'
      }"
    >
      <template slot="table-header-row" slot-scope="props">
        <span v-if="props.column && props.column.field === 'name'">
          {{props.row.label}}
          <button class="fancy-btn">Action</button>
        </span>
        <span v-else>{{ props.formattedRow[props.column.field] }}</span>
      </template>
    </vue-good-table>
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

添加组标题。

我们填充`table-header-row`槽来填充标题。

此外，我们将`headerPosition`设置为`'top'`以将组标题置于行的上方。

删除了`mode`属性，这样我们就可以在 Name 列中显示自定义标题。

# 结论

我们可以用 vue-good-table 分组显示表格行。

此外，我们可以为选择行显示添加内容。