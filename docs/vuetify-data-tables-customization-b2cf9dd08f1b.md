# Vuetify —数据表定制

> 原文：<https://blog.devgenius.io/vuetify-data-tables-customization-b2cf9dd08f1b?source=collection_archive---------2----------------------->

![](img/38ff9803d3006af2ff26d9de82f0fdeb.png)

由 [KOBU 机构](https://unsplash.com/@kobuagency?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 数据表搜索

我们可以添加一个`search`属性来过滤数据表中的数据。

例如，我们可以写:

```
<template>
  <v-card>
    <v-card-title>
      Nutrition
      <v-spacer></v-spacer>
      <v-text-field
        v-model="search"
        append-icon="mdi-magnify"
        label="Search"
        single-line
        hide-details
      ></v-text-field>
    </v-card-title>
    <v-data-table :headers="headers" :items="desserts" :search="search"></v-data-table>
  </v-card>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    search: "",
    headers: [
      {
        text: "Dessert (100g serving)",
        align: "start",
        sortable: false,
        value: "name",
      },
      { text: "Calories", value: "calories" },
      { text: "Fat (g)", value: "fat" },
    ],
    desserts: [
      {
        name: "Frozen Yogurt",
        calories: 200,
        fat: 6.0,
      },
      {
        name: "Ice cream sandwich",
        calories: 200,
        fat: 9.0,
      },
      {
        name: "Eclair",
        calories: 300,
        fat: 16.0,
      },
    ],
  }),
};
</script>
```

我们添加绑定到`search`状态的`v-text-field`。

我们将`search`传递给`search`道具，我们可以搜索任何我们喜欢的列。

# 删除默认页眉和页脚

`hide-default-header`和`hide-default-footer`道具让我们移除默认的页眉和页脚。

例如，我们可以写:

```
<template>
  <v-data-table
    :headers="headers"
    :items="desserts"
    hide-default-header
    hide-default-footer
    class="elevation-1"
  ></v-data-table>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    search: "",
    headers: [
      {
        text: "Dessert (100g serving)",
        align: "start",
        sortable: false,
        value: "name",
      },
      { text: "Calories", value: "calories" },
      { text: "Fat (g)", value: "fat" },
    ],
    desserts: [
      {
        name: "Frozen Yogurt",
        calories: 200,
        fat: 6.0,
      },
      {
        name: "Ice cream sandwich",
        calories: 200,
        fat: 9.0,
      },
      {
        name: "Eclair",
        calories: 300,
        fat: 16.0,
      },
    ],
  }),
};
</script>
```

# 装载状态

我们可以添加`loading`道具来指示表正在加载。

此外，我们可以在表加载时显示一条消息。

例如，我们可以写:

```
<template>
  <v-data-table item-key="name" class="elevation-1" loading loading-text="Loading... Please wait"></v-data-table>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({}),
};
</script>
```

`loading-text`让我们设置要显示的加载文本。

# 稠密的

我们可以添加`dense`道具使行更短:

```
<template>
  <v-data-table :headers="headers" :items="desserts" dense class="elevation-1"></v-data-table>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    search: "",
    headers: [
      {
        text: "Dessert (100g serving)",
        align: "start",
        sortable: false,
        value: "name",
      },
      { text: "Calories", value: "calories" },
      { text: "Fat (g)", value: "fat" },
    ],
    desserts: [
      {
        name: "Frozen Yogurt",
        calories: 200,
        fat: 6.0,
      },
      {
        name: "Ice cream sandwich",
        calories: 200,
        fat: 9.0,
      },
      {
        name: "Eclair",
        calories: 300,
        fat: 16.0,
      },
    ],
  }),
};
</script>
```

现在行更短了。

# 结论

我们可以使用 Vuetify 定制我们的表，进行搜索、排序和分页。