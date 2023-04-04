# 使用 PrimeVue 框架开发 vue 3——卡片、延迟内容和字段集

> 原文：<https://blog.devgenius.io/vue-3-development-with-the-primevue-framework-cards-deferred-content-and-fieldsets-a48f462138f3?source=collection_archive---------4----------------------->

![](img/3345b3d81c7cd92a4c6d4409e54fb60f.png)

照片由 [amirali mirhashemian](https://unsplash.com/@amir_v_ali?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

PrimeVue 是一个与 Vue 3 兼容的 UI 框架。

在本文中，我们将了解如何开始使用 PrimeVue 开发 Vue 3 应用程序。

# 卡片

卡片是容纳我们选择的内容的容器。

为了补充它，我们写道:

`main.js`

```
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from "primevue/config";
import Card from 'primevue/card';
import Button from 'primevue/button';
import 'primevue/resources/primevue.min.css'
import 'primevue/resources/themes/saga-blue/theme.css'
import 'primeicons/primeicons.css'
import 'primeflex/primeflex.css';const app = createApp(App);
app.use(PrimeVue);
app.component("Button", Button);
app.component("Card", Card);
app.mount("#app");
```

`App.vue`

```
<template>
  <div>
    <Card>
      <template #header> Header </template>
      <template #title> Advanced Card </template>
      <template #content> Lorem ipsum dolor sit amet </template>
      <template #footer>
        <Button icon="pi pi-check" label="Save" />
        <Button
          icon="pi pi-times"
          label="Cancel"
          class="p-button-secondary"
          style="margin-left: 0.5em"
        />
      </template>
    </Card>
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

我们添加了`Card`组件，并填充它提供的插槽来添加我们自己的内容。

`header`插槽让我们添加一个标题。

`title`让我们填充标题。

`content`让我们填充主要内容。

`footer`让我们填充页脚。

# 延期内容

`DeferredContent`组件让我们只在内容在视窗中时才加载内容。

为了使用它，我们写:

`main.js`

```
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from "primevue/config";
import DeferredContent from 'primevue/deferredcontent';
import Card from 'primevue/card';
import DataTable from 'primevue/datatable';
import Column  from 'primevue/column';
import 'primevue/resources/primevue.min.css'
import 'primevue/resources/themes/saga-blue/theme.css'
import 'primeicons/primeicons.css'
import 'primeflex/primeflex.css';const app = createApp(App);
app.use(PrimeVue);
app.component("DeferredContent", DeferredContent);
app.component("DataTable", DataTable);
app.component("Column", Column);
app.component("Card", Card);
app.mount("#app");
```

`App.vue`

```
<template>
  <div>
    <DeferredContent>
      <DataTable :value="cars">
        <Column field="vin" header="Vin"></Column>
        <Column field="year" header="Year"></Column>
        <Column field="brand" header="Brand"></Column>
        <Column field="color" header="Color"></Column>
      </DataTable>
    </DeferredContent>
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

我们将`DeferredContent`放在表格周围，只有当项目在视窗中时我们才希望加载它。

# 字段集

我们可以添加`Fieldset`组件来添加一个包含内容的框。

为了使用它，我们写:

`main.js`

```
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from "primevue/config";
import Fieldset from 'primevue/fieldset';
import 'primevue/resources/primevue.min.css'
import 'primevue/resources/themes/saga-blue/theme.css'
import 'primeicons/primeicons.css'
import 'primeflex/primeflex.css';const app = createApp(App);
app.use(PrimeVue);
app.component("Fieldset", Fieldset);
app.mount("#app");
```

`App.vue`

```
<template>
  <div>
    <Fieldset legend="Godfather I"> Lorem ipsum </Fieldset>
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

`legend`文本显示在框的顶部。

我们可以填充`legend`插槽来添加一个自定义标题:

```
<template>
  <div>
    <Fieldset>
      <template #legend> Header Content </template>
      Content
    </Fieldset>
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

我们可以把它和`toggleable`道具放在一起:

```
<template>
  <div>
    <Fieldset toggleable>
      <template #legend> Header Content </template>
      Content
    </Fieldset>
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

# 结论

我们可以使用 PrimeVue 将各种内容容器添加到 Vue 3 应用程序中。