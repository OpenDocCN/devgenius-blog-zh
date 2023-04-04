# 使用 PrimeVue 框架开发 vue 3——滚动表格、可扩展行和可编辑表格单元格

> 原文：<https://blog.devgenius.io/vue-3-development-with-the-primevue-framework-scrolling-table-expandable-rows-and-editable-d3bba7466653?source=collection_archive---------4----------------------->

![](img/99bf6be3491752d2c8868920ba5cbd86.png)

由 [Abel Y Costa](https://unsplash.com/@abelycosta?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

PrimeVue 是一个与 Vue 3 兼容的 UI 框架。

在本文中，我们将了解如何开始使用 PrimeVue 开发 Vue 3 应用程序。

# 滚动表格

我们可以用`scrollable`和`scrollHeight`道具来滚动表格。

例如，我们可以写:

```
<template>
  <div>
    <DataTable
      :value="cars"
      scrollable
      scrollHeight="200px"
    >
      <Column field="vin" header="Vin"> </Column>
      <Column field="year" header="Year"></Column>
      <Column field="brand" header="Brand"></Column>
      <Column field="color" header="Color"></Column>
    </DataTable>
  </div>
</template><script>
const seed = [
  { brand: "Volkswagen", year: 2012, color: "Orange", vin: "dsad231ff" },
  { brand: "Audi", year: 2011, color: "Black", vin: "gwregre345" },
  { brand: "Renault", year: 2005, color: "Gray", vin: "h354htr" },
];
let cars = [...seed];
for (let i = 1; i <= 100; i++) {
  cars.push(...seed);
}export default {
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

我们可以用`frozenValue`道具冻结一行或多行:

```
<template>
  <div>
    <DataTable
      :value="cars"
      scrollable
      scrollHeight="200px"
      :frozenValue="frozenValue"
    >
      <Column field="vin" header="Vin"> </Column>
      <Column field="year" header="Year"></Column>
      <Column field="brand" header="Brand"></Column>
      <Column field="color" header="Color"></Column>
    </DataTable>
  </div>
</template><script>
const seed = [
  { brand: "Volkswagen", year: 2012, color: "Orange", vin: "dsad231ff" },
  { brand: "Audi", year: 2011, color: "Black", vin: "gwregre345" },
  { brand: "Renault", year: 2005, color: "Gray", vin: "h354htr" },
];
let cars = [...seed];
for (let i = 1; i <= 100; i++) {
  cars.push(...seed);
}export default {
  name: "App",
  data() {
    return {
      cars,
      frozenValue: [seed[0]],
    };
  },
  methods: {},
};
</script>
```

# 可扩展的表格行

我们可以使用`expander`属性添加可扩展的表格行:

```
<template>
  <div>
    <DataTable
      :value="cars"
      v-model:selection="selectedCars"
      v-model:expandedRows="expandedRows"
      dataKey="vin"
      [@row](http://twitter.com/row)-expand="onRowExpand"
      [@row](http://twitter.com/row)-collapse="onRowCollapse"
    >
      <Column :expander="true" headerStyle="width: 3rem" />
      <Column field="vin" header="Vin"> </Column>
      <Column field="year" header="Year"></Column>
      <Column field="brand" header="Brand"></Column>
      <Column field="color" header="Color"></Column>
      <template #expansion="slotProps">
        <div>
          <h5>{{ slotProps.data.vin }}</h5>
        </div>
      </template>
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
      expandedRows: [],
    };
  },
  methods: {
    onRowExpand() {
      console.log("expanded");
    },
    onRowCollapse() {
      console.log("collapsed");
    },
  },
};
</script>
```

`expansion`槽有扩展行的内容。

`slotProps.data`有表格行数据。

我们可以通过监听`row-expand`和`row-collapse`事件来监听行展开和折叠时发出的事件。

自从我们与`v-model`绑定以来，`expandedRows`反应属性已经扩展了行数。

# 可编辑的表格单元格

我们可以通过填充`editor`槽使表格单元格可编辑:

```
<template>
  <div>
    <DataTable :value="cars" editMode="cell">
      <Column field="vin" header="Vin">
        <template #editor="slotProps">
          <InputText v-model="slotProps.data[slotProps.column.props.field]" />
        </template>
      </Column>
      <Column field="year" header="Year">
        <template #editor="slotProps">
          <InputText v-model="slotProps.data[slotProps.column.props.field]" />
        </template>
      </Column>
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

单元格的值存储在`slotProps.data[slotProps.column.props.field]`变量中。

# 结论

我们可以使用 PrimeVue 将滚动、可扩展的行和可编辑的表格单元格添加到 Vue 3 应用程序中。