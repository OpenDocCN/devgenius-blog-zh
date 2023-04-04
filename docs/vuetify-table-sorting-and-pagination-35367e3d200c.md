# Vuetify —表格排序和分页

> 原文：<https://blog.devgenius.io/vuetify-table-sorting-and-pagination-35367e3d200c?source=collection_archive---------1----------------------->

![](img/666da745762890ce15b62e9371b028fc.png)

由 [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 外部排序

我们可以用各种道具在外部控制排序。

例如，我们可以写:

```
<template>
  <div>
    <v-data-table
      :headers="headers"
      :items="desserts"
      :sort-by.sync="sortBy"
      :sort-desc.sync="sortDesc"
      class="elevation-1"
    ></v-data-table>
    <div class="text-center pt-2">
      <v-btn color="primary" class="mr-2" @click="toggleOrder">Toggle sort order</v-btn>
      <v-btn color="primary" @click="nextSort">Sort next column</v-btn>
    </div>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    sortBy: "fat",
    sortDesc: false,
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
    toggleOrder() {
      this.sortDesc = !this.sortDesc;
    },
    nextSort() {
      let index = this.headers.findIndex((h) => h.value === this.sortBy);
      index = (index + 1) % this.headers.length;
      this.sortBy = this.headers[index].value;
    },
  },
};
</script>
```

我们更改`sortBy`和`sortDesc`属性值，让我们更改列排序依据，以及我们是分别按升序还是降序排序。

当点击按钮改变属性值时，需要使用`sync`修改器来正确更新表格。

# 服务器端分页和排序

我们可以在服务器端进行分页和排序。

例如，我们可以写:

```
<template>
  <div>
    <v-data-table
      :headers="headers"
      :items="desserts"
      :options.sync="options"
      :server-items-length="totalDesserts"
      :loading="loading"
      class="elevation-1"
    ></v-data-table>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      totalDesserts: 0,
      desserts: [],
      loading: true,
      options: {},
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
    };
  },
  watch: {
    options: {
      async handler() {
        const { items, total } = await this.getDataFromApi();
        this.desserts = items;
        this.totalDesserts = total;
      },
      deep: true,
    },
  },
  async mounted() {
    const { items, total } = await this.getDataFromApi();
    this.desserts = items;
    this.totalDesserts = total;
  },
  methods: {
    getDataFromApi() {
      this.loading = true;
      return new Promise((resolve, reject) => {
        const { sortBy, sortDesc, page, itemsPerPage } = this.options; let items = this.getDesserts();
        const total = items.length; if (sortBy.length === 1 && sortDesc.length === 1) {
          items = items.sort((a, b) => {
            const sortA = a[sortBy[0]];
            const sortB = b[sortBy[0]]; if (sortDesc[0]) {
              if (sortA < sortB) return 1;
              if (sortA > sortB) return -1;
              return 0;
            } else {
              if (sortA < sortB) return -1;
              if (sortA > sortB) return 1;
              return 0;
            }
          });
        } if (itemsPerPage > 0) {
          items = items.slice((page - 1) * itemsPerPage, page * itemsPerPage);
        } setTimeout(() => {
          this.loading = false;
          resolve({
            items,
            total,
          });
        }, 1000);
      });
    },
    getDesserts() {
      return [
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
      ];
    },
  },
};
</script>
```

`getDataFromApi`方法返回一个承诺，该承诺解析为我们想要在表中填充的数据。

排序是用承诺中的`sort`方法完成的。

在`mounted`钩子中，我们获取数据并设置它。

`this.desserts`拥有物品。

`totalDesserts`是甜点总数的数字。

我们将`totalDessert`设为`server-items-length`的值。

而`items`将`desserts`数组作为其值。

# 结论

我们可以使用 Vuetify 对来自客户端或服务器端的数据进行排序和分页。