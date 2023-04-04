# vue-good-table —自定义行和列，以及行选择

> 原文：<https://blog.devgenius.io/vue-good-table-custom-rows-and-columns-and-row-selections-12e1a866003b?source=collection_archive---------5----------------------->

![](img/7a6e8a0ce4f6c69d21ca0be501b08643.png)

[澳门图片社](https://unsplash.com/@macauphotoagency?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

从头开始创建表是一件痛苦的事情。

这就是为什么有很多表格插件让我们轻松添加表格的原因。

在本文中，我们将了解如何使用 vue-good-table 插件将表格添加到 Vue 应用程序中。

# 添加自定义列

我们可以添加不是根据行对象的属性创建的列。

例如，我们可以写:

```
<template>
  <div>
    <vue-good-table :columns="columns" :rows="rows">
      <template slot="table-row" slot-scope="props">
        <span v-if="props.column.field === 'before'">before</span>
        <span v-else>{{props.formattedRow[props.column.field]}}</span>
      </template>
    </vue-good-table>
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

我们用我们希望在列的单元格中显示的内容填充`table-row`槽。

`props.formattedRow[props.column.field]`显示来自行对象属性的数据。

`props.column.field`有字段名。

`columns`数组被更新为具有不是从 row 属性创建的`'before'`列。

# 自定义列标题

我们还可以创建自定义的列标题。

为此，我们用自己的内容填充了`table-column`槽

然后我们可以检查`props.column.label`来检查我们想要更改的列标题。

例如，我们可以写:

```
<template>
  <div>
    <vue-good-table :columns="columns" :rows="rows">
      <template slot="table-column" slot-scope="props">
        <span v-if="props.column.label === 'Name'">
          <em>{{props.column.label}}</em>
        </span>
        <span v-else>{{props.column.label}}</span>
      </template>
    </vue-good-table>
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
  }
};
</script>
```

我们有带`prop.column.label`检查的`table-column`槽来检查我们想要改变的列标题。

我们用带有`em`标签的斜体字体显示 Name 列。

# 复选框表格

我们可以添加一个包含可选行的表。

为此，我们可以写:

```
<template>
  <div>
    <vue-good-table
      :columns="columns"
      :rows="rows"
      :select-options="{
        enabled: true,
        selectOnCheckboxOnly: true, 
        selectionInfoClass: 'selected',
        selectionText: 'rows selected',
        clearSelectionText: 'clear',
        disableSelectInfo: true, 
        selectAllByGroup: true, 
      }"
    ></vue-good-table>
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
  }
};
</script>
```

我们用一个对象添加了`selected-options`道具。

`enabled`属性让我们能够选择行。

`selectOnCheckOnly`让我们在勾选复选框时选择行。

`selectionText`让我们显示用我们自己的文本选择的行数。

`clearSelectionText`是显示在链接上的文本，用于清除选择。

`disableSelectInfo`让我们禁用选定的行信息。如果是`false`，该信息应显示在表格上方。

并且`selectAllByGroup`让我们设置是否可以一次选择所有行。

# 结论

我们可以通过填充插槽来改变列和行，以显示我们想要的内容。

此外，我们可以使行可选。