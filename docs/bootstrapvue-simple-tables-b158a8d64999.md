# BootstrapVue —简单表格

> 原文：<https://blog.devgenius.io/bootstrapvue-simple-tables-b158a8d64999?source=collection_archive---------7----------------------->

![](img/11507186e155b48fb29abb2536699648.png)

由[迈克·本纳](https://unsplash.com/@mbenna?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

为了制作好看的 Vue 应用，我们需要设计组件的样式。

为了使我们的生活更容易，我们可以使用内置样式的组件。

我们来看看如何使用`b-table-simple`组件定制表格内容。

# 简单表格

`b-table-simple`组件让我们像在常规 HTML 中一样创建一个表格。

因此，我们可以完全控制每个细胞。

我们可以添加`striped`、`bordered`、`borderless`、`outlined`、`small`、`hover`、`dark`、`fixed`、`responsive`和`sticky-header`道具来按照我们喜欢的方式设计桌子。

例如，我们可以通过编写以下内容来创建一个:

```
<template>
  <div id="app">
    <b-table-simple responsive>
      <b-thead>
        <b-tr>
          <b-th>id</b-th>
          <b-th>first</b-th>
          <b-th>last</b-th>
        </b-tr>
      </b-thead>
      <b-tbody>
        <b-tr v-for="i of items" :key="i.id">
          <b-td>{{i.id}}</b-td>
          <b-td>{{i.firstName}}</b-td>
          <b-td>{{i.lastName}}</b-td>
        </b-tr>
      </b-tbody>
    </b-table-simple>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      items: [
        { id: 1, firstName: "alex", lastName: "green" },
        {
          id: 2,
          firstName: "may",
          lastName: "smith"
        },
        { id: 3, firstName: "james", lastName: "jones" }
      ]
    };
  }
};
</script>
```

我们有`b-table-simple`组件。

在它里面，我们有`b-thead`组件来添加表格标题。

`b-tr`添加行。

`b-th`添加表头单元格。

`b-tbody`增加表体。

并且`b-td`添加体细胞。

简单的桌子可以像普通桌子一样叠放。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-table-simple stacked>
      <b-tbody>
        <b-tr v-for="i of items" :key="i.id">
          <b-td stacked-heading="id">{{i.id}}</b-td>
          <b-td stacked-heading="first">{{i.firstName}}</b-td>
          <b-td stacked-heading="last">{{i.lastName}}</b-td>
        </b-tr>
      </b-tbody>
    </b-table-simple>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      items: [
        { id: 1, firstName: "alex", lastName: "green" },
        {
          id: 2,
          firstName: "may",
          lastName: "smith"
        },
        { id: 3, firstName: "james", lastName: "jones" }
      ]
    };
  }
};
</script>
```

我们在每个表格单元格上使用带有`stacked-heading`的`stacked`属性来指定要显示的字段名称。

# 简单表格和粘性列

我们可以用`sticky-column`道具做一个粘柱。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-table-simple responsive>
      <b-thead>
        <b-tr>
          <b-th sticky-column>id</b-th>
          <b-th>first</b-th>
          <b-th>last</b-th>
        </b-tr>
      </b-thead>
      <b-tbody>
        <b-tr v-for="i of items" :key="i.id">
          <b-td sticky-column>{{i.id}}</b-td>
          <b-td>{{i.firstName}}</b-td>
          <b-td>{{i.lastName}}</b-td>
        </b-tr>
      </b-tbody>
    </b-table-simple>
  </div>
</template>
<script>
export default {
  name: "App",
  data() {
    return {
      items: [
        { id: 1, firstName: "alex", lastName: "green" },
        {
          id: 2,
          firstName: "may",
          lastName: "smith"
        },
        { id: 3, firstName: "james", lastName: "jones" }
      ]
    };
  }
};
</script>
```

我们只是把`sticky-column`支柱放在列单元格上，使列具有粘性。

# 助手组件

帮助器组件被添加到表格中，让我们能够以定制的方式呈现事物。

`b-table-simple`可以包括`b-tbody`、`b-thead`、`b-tfoot`、`b-te`、`b-td`和`b-th`组件。

`b-table`只能包括`b-tr`、`b-td`和`b-th`组件。

![](img/86e9de611621521d4f638a4fd7574d91.png)

马修·布罗德尔在 Unsplash[上的照片](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用`b-table-simple`组件以更灵活的方式创建表格。

此外，我们可以在表中添加粘性列

它们也可以显示为堆叠的表格。