# 使用 PrimeVue 框架开发 vue 3—表格样式

> 原文：<https://blog.devgenius.io/vue-3-development-with-the-primevue-framework-table-styles-96b4f3e52ed1?source=collection_archive---------4----------------------->

![](img/37abc8697b45fc01ca86cacd2a099322.png)

基思·米斯纳在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

PrimeVue 是一个与 Vue 3 兼容的 UI 框架。

在本文中，我们将了解如何开始使用 PrimeVue 开发 Vue 3 应用程序。

# 表格单元格大小

我们可以用 PrimeVue 提供的类来改变表格单元格的大小。

例如，我们可以写:

```
<template>
  <div>
    <DataTable :value="cars" class="p-datatable-sm">
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

我们添加了`p-datatable-sm`来使单元格变得特别小。

此外，我们可以添加`p-datatable-lg`使单元格变得特别大。

# 表格网格线

我们可以通过编写以下内容向单元格添加表格网格线:

```
<template>
  <div>
    <DataTable :value="cars" class="p-datatable-gridlines">
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

`p-datatable-gridlines`让我们添加网格线。

# 条纹表行

我们可以通过添加`p-datatable-striped`类来使表行成条状:

```
<template>
  <div>
    <DataTable :value="cars" class="p-datatable-striped">
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

# 表格列组

我们可以用`ColumnGroup`组件将表格列组合在一起:

```
<template>
  <div>
    <DataTable :value="cars" class="p-datatable-striped">
      <ColumnGroup type="header">
        <Row>
          <Column header="Vin" :rowspan="1"></Column>
          <Column header="Specs" :colspan="3"></Column>
        </Row>
      </ColumnGroup>
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

我们添加了`type='header'`属性来用`ColumnGroup`标题替换默认的列标题。

# 结论

我们可以使用 PrimeVue 将各种风格的表格添加到我们的 Vue 3 应用程序中。