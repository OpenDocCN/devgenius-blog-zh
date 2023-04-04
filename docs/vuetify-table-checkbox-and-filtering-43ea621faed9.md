# Vuetify —表格复选框和过滤

> 原文：<https://blog.devgenius.io/vuetify-table-checkbox-and-filtering-43ea621faed9?source=collection_archive---------1----------------------->

![](img/e1912befaf46611b4831be8714531e5a.png)

格伦·卡斯滕斯-彼得斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 简单复选框

我们可以在表格中添加一个复选框。

我们可以添加`v-simple-checkbox`组件来添加复选框。

例如，我们可以写:

```
<template>
  <div>
    <v-data-table :headers="headers" :items="desserts" class="elevation-1">
      <template v-slot:item.glutenfree="{ item }">
        <v-simple-checkbox v-model="item.glutenfree" disabled></v-simple-checkbox>
      </template>
    </v-data-table>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    desserts: [
      {
        name: "Frozen Yogurt",
        calories: 159,
        fat: 6.0,
        glutenfree: true,
      },
      {
        name: "Ice cream sandwich",
        calories: 237,
        fat: 9.0,
        glutenfree: false,
      },
      {
        name: "Eclair",
        calories: 262,
        fat: 16.0,
        glutenfree: false,
      },
    ],
    headers: [
      {
        text: "Dessert (100g serving)",
        align: "start",
        sortable: false,
        value: "name",
      },
      { text: "Calories", value: "calories" },
      { text: "Fat (g)", value: "fat" },
      { text: "Gluten-Free", value: "glutenfree" },
    ],
  }),
};
</script>
```

添加一个带有`v-data-table`组件的表格。

`v-slot:item.glutenfree`插槽允许我们添加一个复选框来设置条目的`glutenfree`属性的值。

# 可扩展行

`show-expand`道具让我们在每一行渲染一个扩展图标。

要添加它，我们可以写:

```
<template>
  <v-data-table
    :headers="headers"
    :items="desserts"
    :expanded.sync="expanded"
    item-key="name"
    show-expand
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar flat>
        <v-toolbar-title>Expandable Table</v-toolbar-title>
      </v-toolbar>
    </template>
    <template v-slot:expanded-item="{ headers, item }">
      <td :colspan="headers.length">More info about {{ item.name }}</td>
    </template>
  </v-data-table>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    expanded: [],
    desserts: [
      {
        name: "Frozen Yogurt",
        calories: 159,
        fat: 6.0,
        glutenfree: true,
      },
      {
        name: "Ice cream sandwich",
        calories: 237,
        fat: 9.0,
        glutenfree: false,
      },
      {
        name: "Eclair",
        calories: 262,
        fat: 16.0,
        glutenfree: false,
      },
    ],
    headers: [
      {
        text: "Dessert (100g serving)",
        align: "start",
        sortable: false,
        value: "name",
      },
      { text: "Calories", value: "calories" },
      { text: "Fat (g)", value: "fat" },
      { text: "Gluten-Free", value: "glutenfree" },
    ],
  }),
};
</script>
```

我们用自己的项目填充`expanded-item`插槽。

`expanded`状态让我们获取并设置展开哪些行。

它被用作`expanded.sync`属性的值，因此它可以获取和设置这些值。

# 自定义过滤

我们可以在表中添加过滤。

例如，我们可以写:

```
<template>
  <div>
    <v-data-table
      :headers="headers"
      :items="desserts"
      item-key="name"
      class="elevation-1"
      :search="search"
      :custom-filter="filterOnlyCapsText"
    >
      <template v-slot:top>
        <v-text-field v-model="search" label="Search (UPPER CASE ONLY)" class="mx-4"></v-text-field>
      </template>
      <template v-slot:body.append>
        <tr>
          <td></td>
          <td>
            <v-text-field v-model="calories" type="number" label="Less than"></v-text-field>
          </td>
          <td colspan="4"></td>
        </tr>
      </template>
    </v-data-table>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      search: "",
      calories: "",
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
    };
  },
  computed: {
    headers() {
      return [
        {
          text: "Dessert (100g serving)",
          align: "start",
          sortable: false,
          value: "name",
        },
        {
          text: "Calories",
          value: "calories",
          filter: (value) => {
            return !this.calories || value < parseInt(this.calories);
          },
        },
        { text: "Fat (g)", value: "fat" },
      ];
    },
  },
  methods: {
    filterOnlyCapsText(value, search, item) {
      return (
        value != null &&
        search != null &&
        typeof value === "string" &&
        value.toString().toLocaleUpperCase().indexOf(search) !== -1
      );
    },
  },
};
</script>
```

我们有`filterOnlyCapsText`方法让用户使用大写文本进行搜索。

此外，我们有`header` computed 属性，这样当`this.calories`根据我们输入的内容改变时，我们可以适当地过滤值。

# 结论

我们可以添加自己的过滤逻辑来过滤表格。