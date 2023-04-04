# 使用 PrimeVue 框架开发 vue 3—组织结构图和分页器

> 原文：<https://blog.devgenius.io/vue-3-development-with-the-primevue-framework-org-chart-and-paginator-aef34a1d9561?source=collection_archive---------3----------------------->

![](img/2493ba4bb38000d2e204cb3cb1489281.png)

照片由 [sebastien cordat](https://unsplash.com/@seb_crdt?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

PrimeVue 是一个与 Vue 3 兼容的 UI 框架。

在本文中，我们将了解如何开始使用 PrimeVue 开发 Vue 3 应用程序。

# 组织结构图

PrimeVue 带有一个组织结构图组件，让我们可以轻松地添加一个。

要添加一个，我们写:

`main.js`

```
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from "primevue/config";
import OrganizationChart from 'primevue/organizationchart';
import 'primevue/resources/primevue.min.css'
import 'primevue/resources/themes/saga-blue/theme.css'
import 'primeicons/primeicons.css'
import 'primeflex/primeflex.css';const app = createApp(App);
app.use(PrimeVue);
app.component("OrganizationChart", OrganizationChart);
app.mount("#app");
```

`App.vue`

```
<template>
  <div>
    <OrganizationChart :value="data">
      <template #default="slotProps">
        <span>{{ slotProps.node.data.label }}</span>
      </template>
    </OrganizationChart>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      data: {
        key: "0",
        data: { label: "F.C. Barcelona" },
        children: [
          {
            key: "0_0",
            data: { label: "Barcelona" },
            children: [
              {
                key: "0_0_0",
                data: { label: "Chelsea" },
              },
              {
                key: "0_0_1",
                data: { label: "Barcelona" },
              },
            ],
          },
          {
            key: "0_1",
            data: { label: "Real Madrid" },
            children: [
              {
                key: "0_1_0",
                data: { label: "Bayern Munich" },
              },
              {
                key: "0_1_1",
                data: { label: "Real Madrid" },
              },
            ],
          },
        ],
      },
    };
  },
  methods: {},
};
</script>
```

我们在`main.js`中注册了`OrganizationChart`组件。

然后我们将`OrganizationChart`组件添加到我们的`App.vue`组件中。

`value`被设置为`data`，它是一个具有`key`、`data`和`children`属性的对象。

`key`是唯一的 ID。`data`是一个具有`label`属性的对象，我们通过填充缺省槽来显示它。

`slotProps.node.data`具有每个对象相同的`data`属性。

`children`有一个子节点对象数组。

# 分页器

我们可以将`Paginator`组件添加到我们的 Vue 3 应用程序中，让我们将页面导航添加到我们的应用程序中。

要添加它，我们可以写:

`main.js`

```
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from "primevue/config";
import Paginator from 'primevue/paginator';
import 'primevue/resources/primevue.min.css'
import 'primevue/resources/themes/saga-blue/theme.css'
import 'primeicons/primeicons.css'
import 'primeflex/primeflex.css';const app = createApp(App);
app.use(PrimeVue);
app.component("Paginator", Paginator);
app.mount("#app");
```

`App.vue`

```
<template>
  <div>
    <Paginator :rows="10" :totalRecords="200"></Paginator>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {};
  },
  methods: {},
};
</script>
```

我们注册`Pagination`组件并将其添加到`App`中。

我们设置`rows`来设置每页的条目数。

和`totalRecords`有记录总数。

此外，我们可以设置偏移量，将初始页面设置为不同的页面:

```
<template>
  <div>
    <Paginator :rows="10" :totalRecords="200" :first="20"></Paginator>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {};
  },
  methods: {},
};
</script>
```

等价地，我们可以写:

```
<template>
  <div>
    <Paginator
      :rows="10"
      :totalRecords="200"
      v-model:first="offset"
    ></Paginator>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      offset: 20,
    };
  },
  methods: {},
};
</script>
```

现在第一页是第三页。

此外，我们可以使用`rowsPerPageOptions`属性添加每页行数下拉列表:

```
<template>
  <div>
    <Paginator
      :rows="10"
      :totalRecords="200"
      v-model:first="offset"
      :rowsPerPageOptions="[10, 20, 30]"
    >
    </Paginator>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      offset: 20,
    };
  },
  methods: {},
};
</script>
```

# 结论

我们可以使用 PrimeVue 在 Vue 3 应用程序中添加组织结构图和分页组件。