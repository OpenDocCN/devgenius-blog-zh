# Vuetify —表格槽和分页

> 原文：<https://blog.devgenius.io/vuetify-table-slots-and-pagination-faae82282181?source=collection_archive---------0----------------------->

![](img/26d6742b43cb001ea782e2838e4a1dff.png)

照片由 [Amit Lahav](https://unsplash.com/@amit_lahav?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 自定义默认标题

我们可以用`header.<name>`槽定制默认头。

`<name>`是发送给`headers`的头项中`value`属性的名称。

例如，我们可以写:

```
<template>
  <v-data-table :headers="headers" :items="desserts" class="elevation-1">
    <template v-slot:header.name="{ header }">{{ header.text.toUpperCase() }}</template>
  </v-data-table>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    headers: [
      {
        text: "Dessert (100g serving)",
        align: "start",
        value: "name",
      },
      { text: "Calories", value: "calories" },
      { text: "Fat (g)", value: "fat" },
    ],
    desserts: [
      {
        name: "Frozen Yogurt",
        calories: 159,
        fat: 6.0,
      },
      {
        name: "Ice cream sandwich",
        calories: 237,
        fat: 9.0,
      },
      {
        name: "Eclair",
        calories: 262,
        fat: 16.0,
      },
    ],
  }),
};
</script>
```

添加带有我们自己标题的表格。

我们填充了`header.name`槽来定制`name`列标题的显示。

我们将其显示为大写字母。

# 自定义列

我们可以定制一些带`item.<name>`槽的立柱。

`<name>`是我们要更改的列的属性名。

例如，我们可以写:

```
<template>
  <v-data-table :headers="headers" :items="desserts" class="elevation-1">
    <template v-slot:item.calories="{ item }">
      <v-chip :color="getColor(item.calories)" dark>{{ item.calories }}</v-chip>
    </template>
  </v-data-table>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    headers: [
      {
        text: "Dessert (100g serving)",
        align: "start",
        value: "name",
      },
      { text: "Calories", value: "calories" },
      { text: "Fat (g)", value: "fat" },
    ],
    desserts: [
      {
        name: "Frozen Yogurt",
        calories: 159,
        fat: 6.0,
      },
      {
        name: "Ice cream sandwich",
        calories: 237,
        fat: 9.0,
      },
      {
        name: "Eclair",
        calories: 262,
        fat: 16.0,
      },
    ],
  }),
  methods: {
    getColor(calories) {
      if (calories > 400) return "red";
      else if (calories > 200) return "orange";
      else return "green";
    },
  },
};
</script>
```

我们在`item.calories`插槽中添加了一个芯片来改变卡路里栏的显示方式。

# 外部分页

我们可以用`options`属性添加外部分页。

例如，我们可以写:

```
<template>
  <div>
    <v-data-table
      :headers="headers"
      :items="desserts"
      :page.sync="page"
      :items-per-page="itemsPerPage"
      hide-default-footer
      class="elevation-1"
      [@page](http://twitter.com/page)-count="pageCount = $event"
    ></v-data-table>
    <div class="text-center pt-2">
      <v-pagination v-model="page" :length="pageCount"></v-pagination>
      <v-text-field
        :value="itemsPerPage"
        label="Items per page"
        type="number"
        min="-1"
        max="15"
        [@input](http://twitter.com/input)="itemsPerPage = parseInt($event, 10)"
      ></v-text-field>
    </div>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    page: 1,
    pageCount: 0,
    itemsPerPage: 10,
    headers: [
      {
        text: "Dessert (100g serving)",
        align: "start",
        value: "name",
      },
      { text: "Calories", value: "calories" },
      { text: "Fat (g)", value: "fat" },
    ],
    desserts: [
      {
        name: "Frozen Yogurt",
        calories: 159,
        fat: 6.0,
      },
      {
        name: "Ice cream sandwich",
        calories: 237,
        fat: 9.0,
      },
      {
        name: "Eclair",
        calories: 262,
        fat: 16.0,
      },
    ],
  }),
  methods: {
    getColor(calories) {
      if (calories > 400) return "red";
      else if (calories > 200) return "orange";
      else return "green";
    },
  },
};
</script>
```

向我们的页面添加一个文本字段，让用户更改每页的项目。

此外，我们添加了`v-pagination`组件来添加分页按钮。

# 结论

我们可以用各种组件和插槽来改变列和分页按钮的显示方式。