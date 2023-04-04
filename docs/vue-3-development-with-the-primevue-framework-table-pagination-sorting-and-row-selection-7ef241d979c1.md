# 使用 PrimeVue 框架开发 vue 3——表分页、排序和行选择

> 原文：<https://blog.devgenius.io/vue-3-development-with-the-primevue-framework-table-pagination-sorting-and-row-selection-7ef241d979c1?source=collection_archive---------1----------------------->

![](img/4a5aa84c5a17209ace1528959975589d.png)

照片由[索菲亚·巴博拉](https://unsplash.com/@sophiababoolal?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

PrimeVue 是一个与 Vue 3 兼容的 UI 框架。

在本文中，我们将了解如何开始使用 PrimeVue 开发 Vue 3 应用程序。

# 表格分页

我们可以使用 PrimeVue 轻松地将表格分页添加到我们的 Vue 3 应用程序中。

例如，我们可以写:

```
<template>
  <div>
    <DataTable
      :value="cars"
      class="p-datatable-striped"
      paginator
      :rows="10"
      paginatorTemplate="CurrentPageReport FirstPageLink PrevPageLink PageLinks NextPageLink LastPageLink RowsPerPageDropdown"
      :rowsPerPageOptions="[10, 20, 50]"
      currentPageReportTemplate="Showing {first} to {last} of {totalRecords}"
    >
      <Column field="vin" header="Vin"> </Column>
      <Column field="year" header="Year"></Column>
      <Column field="brand" header="Brand"></Column>
      <Column field="color" header="Color"></Column>
      <template #paginatorLeft>
        <Button type="button" icon="pi pi-refresh" class="p-button-text" />
      </template>
      <template #paginatorRight>
        <Button type="button" icon="pi pi-cloud" class="p-button-text" />
      </template>
    </DataTable>
  </div>
</template><script>
const seed = [
  { brand: "Volkswagen", year: 2012, color: "Orange", vin: "dsad231ff" },
  { brand: "Audi", year: 2011, color: "Black", vin: "gwregre345" },
  { brand: "Renault", year: 2005, color: "Gray", vin: "h354htr" },
];let cars = [...seed];
for (let i = 1; i <= 100; i++) {
  cars.push(...seed);
}
export default {
  name: "App",
  data() {
    return {
      cars,
    };
  },
  methods: {},
};
</script>
```

我们添加了`paginator`属性来启用分页。

`rows`有每页的行数。

`paginatorTemplate`让我们用一个字符串指定分页栏的布局。

`rowsPerPageOptions`有分页的每页行数选项。

`currentPageReportTemplate`有字符串模板向用户显示页面信息。

我们填充了`paginatorLeft`和`paginatorRight`插槽来添加导航页面的按钮。

# 表格排序

我们可以用每一列上的`sortable`属性使表格列可排序。

例如，我们可以写:

```
<template>
  <div>
    <DataTable :value="cars" sortMode="multiple">
      <Column field="vin" header="Vin" sortable="true"> </Column>
      <Column field="year" header="Year" sortable="true"></Column>
      <Column field="brand" header="Brand" sortable="true"></Column>
      <Column field="color" header="Color" sortable="true"></Column>
    </DataTable>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      cars: [
        { brand: "Volkswagen", year: 2012, color: "Orange", vin: "dsad231ff" },
        { brand: "Audi", year: 2011, color: "Black", vin: "gwregre345" },
        { brand: "Renault", year: 2005, color: "Gray", vin: "h354htr" },
      ],
    };
  },
  methods: {},
};
</script>
```

我们将`sortable`设置为`true`来对每一列进行排序。

`sortMode`设置为`'multiple'`让我们按多列排序。

# 表格行选择

我们可以使用`selectionMode`属性为表格行启用选择:

```
<template>
  <div>
    <DataTable
      :value="cars"
      v-model:selection="selectedCars"
      selectionMode="single"
      dataKey="vin"
    >
      <Column field="vin" header="Vin"> </Column>
      <Column field="year" header="Year"></Column>
      <Column field="brand" header="Brand"></Column>
      <Column field="color" header="Color"></Column>
    </DataTable>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      selectedCars: [],
      cars: [
        { brand: "Volkswagen", year: 2012, color: "Orange", vin: "dsad231ff" },
        { brand: "Audi", year: 2011, color: "Black", vin: "gwregre345" },
        { brand: "Renault", year: 2005, color: "Gray", vin: "h354htr" },
      ],
    };
  },
  methods: {},
};
</script>
```

`dataKey`具有每一行的唯一标识符的属性名。

我们用`v-model`将选择绑定到`selectedCars`反应属性。

# 结论

我们可以用 PrimeVue 的表格组件为表格添加分页、行选择和排序。