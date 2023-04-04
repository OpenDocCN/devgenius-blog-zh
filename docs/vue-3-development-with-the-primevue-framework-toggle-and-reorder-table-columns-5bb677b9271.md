# 使用 PrimeVue 框架开发 vue 3——切换和重新排序表格列

> 原文：<https://blog.devgenius.io/vue-3-development-with-the-primevue-framework-toggle-and-reorder-table-columns-5bb677b9271?source=collection_archive---------4----------------------->

![](img/80cb56f30b7ea3d3c6a1729558579b5d.png)

照片由[拉斐尔·诺盖拉](https://unsplash.com/@phaelnogueira?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

PrimeVue 是一个与 Vue 3 兼容的 UI 框架。

在本文中，我们将了解如何开始使用 PrimeVue 开发 Vue 3 应用程序。

# 切换表格列

我们可以添加一个多选下拉列表来打开和关闭表格列。

例如，我们可以写:

```
<template>
  <div>
    <DataTable :value="cars">
      <template #header>
        <div style="text-align: left">
          <MultiSelect
            :modelValue="selectedColumns"
            :options="columns"
            optionLabel="header"
            @update:modelValue="onToggle"
            placeholder="Select Columns"
            style="width: 20em"
          />
        </div>
      </template>
      <Column
        v-for="(col, index) of selectedColumns"
        :field="col.field"
        :header="col.header"
        :key="`${col.field}_${index}`"
      ></Column>
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
      selectedColumns: [],
      columns: [
        { field: "vin", header: "VIN" },
        { field: "year", header: "Year" },
        { field: "color", header: "Color" },
        { field: "brand", header: "Brand" },
      ],
    };
  },
  methods: {
    onToggle(value) {
      this.selectedColumns = this.columns.filter((col) => value.includes(col));
    },
  },
  created() {
    this.selectedColumns = this.columns;
  },
};
</script>
```

我们监听`update:modelValue`事件来运行代码，过滤掉我们禁用的列。

我们从`value`参数中获得选择的列。

# 可调整大小的列

我们可以用`resizableColumns`道具来调整列的大小:

```
<template>
  <div>
    <DataTable
      :value="cars"
      :resizableColumns="true"
      columnResizeMode="fit"
      class="p-datatable-gridlines"
    >
      <Column field="vin" header="Vin"> </Column>
      <Column field="year" header="Year"> </Column>
      <Column field="brand" header="Brand"></Column>
      <Column field="color" header="Color"></Column>
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

我们将属性设置为`true`来调整列的大小。

`columnResizeMode`设置为`'fit'`以使各列适合内容。

# 重新排序表格行

通过监听`row-reorder`事件，可以对表格行进行重新排序:

```
<template>
  <div>
    <DataTable
      :value="cars"
      :reorderableColumns="true"
      @column-reorder="onColReorder"
      @row-reorder="onRowReorder"
    >
      <Column
        :rowReorder="true"
        headerStyle="width: 3rem"
        :reorderableColumn="false"
      />
      <Column
        v-for="col of columns"
        :field="col.field"
        :header="col.header"
        :key="col.field"
      ></Column>
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
      columns: [
        { field: "vin", header: "VIN" },
        { field: "year", header: "Year" },
        { field: "color", header: "Color" },
        { field: "brand", header: "Brand" },
      ],
    };
  },
  methods: {
    onColReorder() {},
    onRowReorder(event) {
      this.cars = event.value;
    },
  },
};
</script>
```

我们用`event.value`属性得到重新排序的项目。

同样，我们可以监听`column-reorder`事件来监听列重新排序操作。

# 结论

我们可以在 PrimeVue 的表格中切换和重新排列表格的行和列。