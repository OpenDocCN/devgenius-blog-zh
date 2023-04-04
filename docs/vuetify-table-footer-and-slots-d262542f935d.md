# Vuetify —表尾和槽

> 原文：<https://blog.devgenius.io/vuetify-table-footer-and-slots-d262542f935d?source=collection_archive---------3----------------------->

![](img/8541319349d9693f3da0532d6a35434d.png)

史蒂夫·萨伍施在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 页脚道具

我们可以改变`v-data-table`的`footer-props`道具来改变页脚。

例如，我们可以写:

```
<template>
  <v-data-table
    :headers="headers"
    :items="desserts"
    :items-per-page="5"
    item-key="name"
    class="elevation-1"
    :footer-props="{
      showFirstLastPage: true,
      firstIcon: 'mdi-arrow-collapse-left',
      lastIcon: 'mdi-arrow-collapse-right',
      prevIcon: 'mdi-minus',
      nextIcon: 'mdi-plus'
    }"
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

我们有一个具有`firstIcon`、`lastIcon`、`prevIcon`和`nextIcon`属性的对象来改变表格的导航按钮。

`showFirstLastPage`让我们显示前往第一页和最后一页的按钮。

# 可过滤的列

我们可以用`filterable`属性将一些列设置为可过滤的。

例如，我们可以写:

```
<template>
  <v-card>
    <v-card-title>
      <v-text-field v-model="search" append-icon="search" label="Search" single-line hide-details></v-text-field>
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
        filterable: false,
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

禁用第一列的筛选。

我们将`filterable`属性添加到`headers`数组中的对象中。

# 时间

我们可以用我们的内容填充各种插槽。

例如，我们可以写:

```
<template>
  <div>
    <v-select v-model="enabled" :items="slots" label="Slot" clearable></v-select>
    <v-data-table
      :headers="headers"
      :items="items"
      :search="search"
      :hide-default-header="hideHeaders"
      :show-select="showSelect"
      :loading="isLoading"
      hide-default-footer
      item-key="name"
      class="elevation-1"
    >
      <template v-if="isEnabled('top')" v-slot:top>
        <div>This is content above the actual table</div>
      </template> <template
        v-show="isEnabled('header.data-table-select')"
        v-slot:header.data-table-select="{ on, props }"
      >
        <v-simple-checkbox color="purple" v-bind="props" v-on="on"></v-simple-checkbox>
      </template><template v-if="isEnabled('header')" v-slot:header="{ props: { headers } }">
        <thead>
          <tr>
            <th :colspan="headers.length">This is a header</th>
          </tr>
        </thead>
      </template><template v-show="isEnabled('progress')" v-slot:progress>
        <v-progress-linear color="purple" :height="10" indeterminate></v-progress-linear>
      </template> <template
        v-show="isEnabled('item.data-table-select')"
        v-slot:item.data-table-select="{ isSelected, select }"
      >
        <v-simple-checkbox color="green" :value="isSelected" @input="select($event)"></v-simple-checkbox>
      </template> <template
        v-show="isEnabled('item.<name>')"
        v-slot:item.name="{ item }"
      >{{ item.name.toUpperCase() }}</template><template v-show="isEnabled('body.prepend')" v-slot:body.prepend="{ headers }">
        <tr>
          <td :colspan="headers.length">This is a prepended row</td>
        </tr>
      </template> <template v-show="isEnabled('body')" v-slot:body="{ items }">
        <tbody>
          <tr v-for="item in items" :key="item.name">
            <td>{{ item.name }}</td>
          </tr>
        </tbody>
      </template> <template v-show="isEnabled('no-data')" v-slot:no-data>NO DATA HERE!</template> <template v-show="isEnabled('no-results')" v-slot:no-results>NO RESULTS HERE!</template> <template v-show="isEnabled('body.append')" v-slot:body.append="{ headers }">
        <tr>
          <td :colspan="headers.length">This is an appended row</td>
        </tr>
      </template> <template v-show="isEnabled('footer')" v-slot:footer>
        <div>This is a footer</div>
      </template>
    </v-data-table>
  </div>
</template>
<script>
const desserts = [
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
];export default {
  name: "HelloWorld",
  data: () => ({
    enabled: null,
    search: null,
    slots: [
      "body",
      "body.append",
      "body.prepend",
      "footer",
      "header.data-table-select",
      "header",
      "progress",
      "item.data-table-select",
      "item.<name>",
      "no-data",
      "no-results",
      "top",
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
    ],
    items: desserts,
  }),
  computed: {
    showSelect() {
      return (
        this.isEnabled("header.data-table-select") ||
        this.isEnabled("item.data-table-select")
      );
    },
    hideHeaders() {
      return !this.showSelect;
    },
    isLoading() {
      return this.isEnabled("progress");
    },
  }, watch: {
    enabled(slot) {
      if (slot === "no-data") {
        this.items = [];
      } else if (slot === "no-results") {
        this.search = "...";
      } else {
        this.search = null;
        this.items = desserts;
      }
    },
  }, methods: {
    isEnabled(slot) {
      return this.enabled === slot;
    },
  },
};
</script>
```

我们添加了带有插槽的`template`元素。

正文有许多位置，当没有数据或没有结果时显示数据，还有页脚等等。

下拉菜单设置哪个被启用，我们使用`v-show`显示被启用的。

# 结论

我们可以用自己的内容填充各种插槽。

页脚图标也可以更改。