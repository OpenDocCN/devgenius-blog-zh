# 使用 PrimeVue 框架开发 vue 3—数据视图

> 原文：<https://blog.devgenius.io/vue-3-development-with-the-primevue-framework-data-views-5dfabe63b3ca?source=collection_archive---------1----------------------->

![](img/738dd75d1cb988294cd1bdabdb20dc37.png)

保罗·吉尔摩在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

PrimeVue 是一个与 Vue 3 兼容的 UI 框架。

在本文中，我们将了解如何开始使用 PrimeVue 开发 Vue 3 应用程序。

# 数据视图

我们可以使用 PrimeVue 的`DataView`组件将数据视图添加到我们的应用程序中。

例如，我们可以写:

`main.js`

```
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from "primevue/config";
import DataView from 'primevue/dataview';
import DataViewLayoutOptions from 'primevue/dataviewlayoutoptions';
import Panel from 'primevue/panel';
import 'primevue/resources/primevue.min.css'
import 'primevue/resources/themes/saga-blue/theme.css'
import 'primeicons/primeicons.css'
import 'primeflex/primeflex.css';const app = createApp(App);
app.use(PrimeVue);
app.component("DataView", DataView);
app.component("DataViewLayoutOptions", DataViewLayoutOptions);
app.component("Panel", Panel);
app.mount("#app");
```

`App.vue`

```
<template>
  <div>
    <DataView :value="cars" layout='grid'>
      <template #list="slotProps">
        <div class="p-col-12">
          <div class="car-details">
            <div>
              <div class="p-grid">
                <div class="p-col-12">
                  Vin: <b>{{ slotProps.data.vin }}</b>
                </div>
                <div class="p-col-12">
                  Year: <b>{{ slotProps.data.year }}</b>
                </div>
                <div class="p-col-12">
                  Brand: <b>{{ slotProps.data.brand }}</b>
                </div>
                <div class="p-col-12">
                  Color: <b>{{ slotProps.data.color }}</b>
                </div>
              </div>
            </div>
          </div>
        </div>
      </template>
      <template #grid="slotProps">
        <div style="padding: 0.5em" class="p-col-12 p-md-3">
          <Panel :header="slotProps.data.vin" style="text-align: center">
            <div class="car-detail">
              {{ slotProps.data.year }} - {{ slotProps.data.color }}
            </div>
          </Panel>
        </div>
      </template>
    </DataView>
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

我们设置`value`属性来填充数据。

`list`槽用于渲染道具`layout`设置为`list`的物品。

当`layout`将其设置为`grid`时，`grid`槽用于渲染项目。

我们用`slotProps.data`属性获取条目数据。

`header`槽让我们填充标题内容。

而`footer`槽让我们填充页脚内容:

```
<template>
  <div>
    <DataView :value="cars" layout="list">
      <template #header>Header </template>
      <template #footer>Footer </template>
      <template #list="slotProps">
        <div class="p-col-12">
          <div class="car-details">
            <div>
              <div class="p-grid">
                <div class="p-col-12">
                  Vin: <b>{{ slotProps.data.vin }}</b>
                </div>
                <div class="p-col-12">
                  Year: <b>{{ slotProps.data.year }}</b>
                </div>
                <div class="p-col-12">
                  Brand: <b>{{ slotProps.data.brand }}</b>
                </div>
                <div class="p-col-12">
                  Color: <b>{{ slotProps.data.color }}</b>
                </div>
              </div>
            </div>
          </div>
        </div>
      </template>
      <template #grid="slotProps">
        <div style="padding: 0.5em" class="p-col-12 p-md-3">
          <Panel :header="slotProps.data.vin" style="text-align: center">
            <div class="car-detail">
              {{ slotProps.data.year }} - {{ slotProps.data.color }}
            </div>
          </Panel>
        </div>
      </template>
    </DataView>
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

# 结论

我们可以使用 PrimeVue 的`DataView`组件将数据视图添加到我们的 Vue 3 应用程序中。