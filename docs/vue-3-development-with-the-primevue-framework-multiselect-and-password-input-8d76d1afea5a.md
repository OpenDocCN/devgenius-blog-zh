# 用 PrimeVue 框架开发 vue 3——多选和密码输入

> 原文：<https://blog.devgenius.io/vue-3-development-with-the-primevue-framework-multiselect-and-password-input-8d76d1afea5a?source=collection_archive---------4----------------------->

![](img/533222f8faffe150757070212a135752.png)

照片由 [Waranont (Joe)](https://unsplash.com/@tricell1991?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

PrimeVue 是一个与 Vue 3 兼容的 UI 框架。

在本文中，我们将了解如何开始使用 PrimeVue 开发 Vue 3 应用程序。

# 多选

PrimeVue 带有一个多选下拉组件，让我们选择多个选项。

要添加它，我们可以添加`MultiSelect`组件:

`main.js`

```
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from "primevue/config";
import MultiSelect from 'primevue/multiselect';
import 'primevue/resources/primevue.min.css'
import 'primevue/resources/themes/saga-blue/theme.css'
import 'primeicons/primeicons.css'
import 'primeflex/primeflex.css';const app = createApp(App);
app.use(PrimeVue);
app.component("MultiSelect", MultiSelect);
app.mount("#app");
```

`App.vue`

```
<template>
  <div>
    <MultiSelect
      v-model="selectedCity"
      :options="cities"
      optionLabel="name"
    />
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      selectedCity: null,
      cities: [
        { name: "Miami", code: "MI" },
        { name: "Rome", code: "RM" },
        { name: "London", code: "LDN" },
        { name: "Istanbul", code: "IST" },
        { name: "Paris", code: "PRS" },
      ],
    };
  },
  methods: {},
};
</script>
```

`options`让我们设置渲染选项。

`optionLabel`有要渲染的属性名称作为选择。

这个`filter`道具增加了一个输入框，让用户搜索选项。

我们可以用`display='chip'`道具将选定的选项显示为筹码:

```
<template>
  <div>
    <MultiSelect
      v-model="selectedCity"
      :options="cities"
      optionLabel="name"
      display="chip"
    />
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      selectedCity: null,
      cities: [
        { name: "Miami", code: "MI" },
        { name: "Rome", code: "RM" },
        { name: "London", code: "LDN" },
        { name: "Istanbul", code: "IST" },
        { name: "Paris", code: "PRS" },
      ],
    };
  },
  methods: {},
};
</script>
```

我们可以通过填充`value`槽来定制所选选项的显示方式。

我们可以通过填充`option`插槽来定制每个选项的显示方式:

```
<template>
  <div>
    <MultiSelect
      v-model="selectedCity"
      :options="cities"
      optionLabel="name"
      display="chip"
    >
      <template #value="slotProps">
        <div v-for="option of slotProps.value" :key="option.brand">
          <span>{{ option.name }}</span>
        </div>
        <template v-if="!slotProps.value || slotProps.value.length === 0">
          Select Brands
        </template>
      </template>
      <template #option="slotProps">
        <div>
          <span>{{ slotProps.option.name }}</span>
        </div>
      </template>
    </MultiSelect>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      selectedCity: null,
      cities: [
        { name: "Miami", code: "MI" },
        { name: "Rome", code: "RM" },
        { name: "London", code: "LDN" },
        { name: "Istanbul", code: "IST" },
        { name: "Paris", code: "PRS" },
      ],
    };
  },
  methods: {},
};
</script>
```

我们可以从每个插槽的`slotProps`中获得选项。

# 密码输入

PrimeVue 自带密码输入。

我们可以用`Password`组件添加它:

`main.js`

```
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from "primevue/config";
import Password from 'primevue/password';
import 'primevue/resources/primevue.min.css'
import 'primevue/resources/themes/saga-blue/theme.css'
import 'primeicons/primeicons.css'
import 'primeflex/primeflex.css';const app = createApp(App);
app.use(PrimeVue);
app.component("Password", Password);
app.mount("#app");
```

`App.vue`

```
<template>
  <div>
    <Password v-model="value" />
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: null,
    };
  },
  methods: {},
};
</script>
```

它带有一个密码强度表。

中等密码至少包含 1 个小写字符、1 个大写字符或数字，以及至少 6 个字符。

强密码至少包含 1 个小写字符、1 个大写字符、1 个数字字符和至少 8 个字符。

# 结论

我们可以使用 PrimeVue 框架将多选下拉菜单和密码输入添加到 Vue 3 应用程序中。