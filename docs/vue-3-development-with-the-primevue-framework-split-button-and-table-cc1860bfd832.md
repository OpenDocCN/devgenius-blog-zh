# 用 PrimeVue 框架开发 vue 3—拆分按钮和表格

> 原文：<https://blog.devgenius.io/vue-3-development-with-the-primevue-framework-split-button-and-table-cc1860bfd832?source=collection_archive---------1----------------------->

![](img/839cae2008ac51b6a75861ecddbb3bcb.png)

照片由[斯潘塞·戴维斯](https://unsplash.com/@spencerdavis?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

PrimeVue 是一个与 Vue 3 兼容的 UI 框架。

在本文中，我们将了解如何开始使用 PrimeVue 开发 Vue 3 应用程序。

# 拆分按钮

我们可以用`SplitButton`组件添加分割按钮。

例如，我们可以写:

`main.js`

```
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from "primevue/config";
import SplitButton from 'primevue/splitbutton';
import 'primevue/resources/primevue.min.css'
import 'primevue/resources/themes/saga-blue/theme.css'
import 'primeicons/primeicons.css'
import 'primeflex/primeflex.css';const app = createApp(App);
app.use(PrimeVue);
app.component("SplitButton", SplitButton);
app.mount("#app");
```

`App.vue`

```
<template>
  <div>
    <SplitButton label="Save" icon="pi pi-plus" :model="items"></SplitButton>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      items: [
        {
          label: "Update",
          icon: "pi pi-refresh",
          command: () => {
            alert("updated");
          },
        },
        {
          label: "Delete",
          icon: "pi pi-times",
          command: () => {
            alert("deleted");
          },
        },
      ],
    };
  },
  methods: {},
};
</script>
```

我们将拆分按钮下拉列表的项目添加到`items`数组中。

`label`有下拉项目的标签文本。

`icon`有下拉项的图标类。

`command`被设置为当我们点击下拉菜单时运行的函数。

我们将`items`设置为`models`的值，以显示下拉项目。

`label`有标签文本。

`icon`道具有图标类。

我们还可以添加以下类之一来更改按钮样式:

*   。p 按钮-辅助
*   。按钮-成功
*   。按钮信息
*   。p 按钮-警告
*   。按钮-帮助
*   。按钮-危险

# 数据表

我们可以用 PrimeVue 的`DataTable`和`Column`组件在我们的 Vue 3 应用中添加一个基本表格。

例如，我们可以写:

`main.js`

```
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from "primevue/config";
import DataTable from 'primevue/datatable';
import Column from 'primevue/column';
import ColumnGroup from 'primevue/columngroup';
import 'primevue/resources/primevue.min.css'
import 'primevue/resources/themes/saga-blue/theme.css'
import 'primeicons/primeicons.css'
import 'primeflex/primeflex.css';const app = createApp(App);
app.use(PrimeVue);
app.component("DataTable", DataTable);
app.component("Column", Column);
app.component("ColumnGroup", ColumnGroup);
app.mount("#app");
```

`App.vue`

```
<template>
  <div>
    <DataTable :value="cars">
      <Column field="vin" header="Vin"></Column>
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

我们有一个`cars`数组，它被设置为`value`属性的值。

然后我们用`Column`组件显示列数据。

`field`有要显示的`cars`数组条目的属性名。

`header`有相应的列标题显示给用户。

我们可以通过填充`Column`组件的`body`插槽来定制列数据的显示方式:

```
<template>
  <div>
    <DataTable :value="cars">
      <Column field="vin" header="Vin">
        <template #body="slotProps">
          <b>{{ slotProps.data.vin }}</b>
        </template>
      </Column>
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

我们用`slotProps.data`属性获取条目的数据。

# 结论

我们可以使用 PrimeVue 将拆分按钮和基本表格添加到我们的 Vue 3 应用程序中。