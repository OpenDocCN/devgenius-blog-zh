# vue-good-table —行选择和搜索

> 原文：<https://blog.devgenius.io/vue-good-table-row-selection-and-searching-223d587a01b3?source=collection_archive---------3----------------------->

![](img/375d9d860cc06c3543878bee628ec66c.png)

[澳门图片社](https://unsplash.com/@macauphotoagency?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

从头开始创建表是一件痛苦的事情。

这也是为什么有很多表格插件让我们轻松添加表格的原因。

在本文中，我们将了解如何使用 vue-good-table 插件将表格添加到 Vue 应用程序中。

# 选择事件

我们可以监听表行 select all 事件，该事件是在选择了所有表行时发出的。

例如，我们可以写:

```
<template>
  <div>
    <vue-good-table
      :columns="columns"
      :rows="rows"
      :select-options="{ enabled: true }"
      [@on](http://twitter.com/on)-select-all="onSelectAll"
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
        { id: 3, name: "Mary", age: 16, birthDay: "2004-10-30" }
      ]
    };
  },
  methods: {
    onSelectAll(params) {
      console.log(params);
    }
  }
};
</script>
```

我们给每一行添加一个复选框，让我们选择带有一个对象的`select-options`属性设置为`true`的行。

此外，我们添加了一个复选框来选择左上角的所有行。

# 搜索选项

我们可以给我们的`vue-good-table`添加一个搜索框。

我们所要做的就是添加一个带有各种选项的对象的`search-options`道具。

例如，我们可以写:

```
<template>
  <div>
    <vue-good-table
      :columns="columns"
      :rows="rows"
      :search-options="{
        enabled: true,
        placeholder: 'Search table',          
      }"
      [@on](http://twitter.com/on)-search="onSearch"
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
        { id: 3, name: "Mary", age: 16, birthDay: "2004-10-30" }
      ]
    };
  }
};
</script>
```

我们添加了带有各种属性的对象的`search-options`道具。

设置为`true`的`enabled`属性将使搜索框显示在表格上方。

`placeholder`让我们为搜索框添加一个占位符。

我们还可以改变`trigger`属性来改变搜索的触发方式。

例如，我们可以写:

```
<template>
  <div>
    <vue-good-table
      :columns="columns"
      :rows="rows"
      :search-options="{
        enabled: true,
        placeholder: 'Search table',   
        trigger: 'enter'       
      }"
      [@on](http://twitter.com/on)-search="onSearch"
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
        { id: 3, name: "Mary", age: 16, birthDay: "2004-10-30" }
      ]
    };
  }
};
</script>
```

我们将`trigger`设置为`'enter'`,这样只有当我们按下回车键时才进行搜索。

此外，我们可以添加`skipDiacritics`属性来阻止 vue-good-table 比较重音字符。

这会加快我们的搜索速度。

例如，我们补充道:

```
<template>
  <div>
    <vue-good-table
      :columns="columns"
      :rows="rows"
      :search-options="{
        enabled: true,
        skipDiacritics: true,     
      }"
      [@on](http://twitter.com/on)-search="onSearch"
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
        { id: 3, name: "Mary", age: 16, birthDay: "2004-10-30" }
      ]
    };
  }
};
</script>
```

跳过重音字符比较。

为了定制如何进行搜索，我们可以将`searchFn`属性设置为我们自己的函数。

例如，我们可以写:

```
<template>
  <div>
    <vue-good-table
      :columns="columns"
      :rows="rows"
      :search-options="{
        enabled: true,
        searchFn     
      }"
      [@on](http://twitter.com/on)-search="onSearch"
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
        { id: 3, name: "Mary", age: 16, birthDay: "2004-10-30" }
      ]
    };
  },
  methods: {
    searchFn(row, col, cellValue, searchTerm) {
      return cellValue === searchTerm;
    }
  }
};
</script>
```

我们有带搜索词的`searchTerm`。,

`cellValue`具有可与搜索词进行比较以找到匹配行的单元格值。

`row`和`col`分别有行和列对象。

# 结论

我们可以添加一个搜索框，用 vue-good-table 进行表格搜索。

此外，我们可以启用行选择并获取所选择的内容。