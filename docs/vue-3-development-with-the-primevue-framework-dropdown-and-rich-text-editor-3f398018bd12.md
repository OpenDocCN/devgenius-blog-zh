# 使用 PrimeVue 框架开发 vue 3——下拉菜单和富文本编辑器

> 原文：<https://blog.devgenius.io/vue-3-development-with-the-primevue-framework-dropdown-and-rich-text-editor-3f398018bd12?source=collection_archive---------0----------------------->

![](img/94002eaea2fed7f1a6d7deb1d295055b.png)

安迪·福尔摩斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

PrimeVue 是一个与 Vue 3 兼容的 UI 框架。

在本文中，我们将了解如何开始使用 PrimeVue 开发 Vue 3 应用程序。

# 下拉式

我们可以用 PrimeVue 的`Dropdown`组件在我们的 Vue 3 应用中添加一个下拉菜单。

例如，我们可以写:

`main.js`

```
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from "primevue/config";
import Dropdown from 'primevue/dropdown';
import 'primevue/resources/primevue.min.css'
import 'primevue/resources/themes/saga-blue/theme.css'
import 'primeicons/primeicons.css'
import 'primeflex/primeflex.css';const app = createApp(App);
app.use(PrimeVue);
app.component("Dropdown", Dropdown);
app.mount("#app");
```

`App.vue`

```
<template>
  <div>
    <Dropdown
      v-model="selectedCity"
      :options="cities"
      optionLabel="name"
      placeholder="Select a City"
    />
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      selectedCity: "",
      cities: [
        { name: "New York", code: "NY" },
        { name: "Miami", code: "MI" },
        { name: "London", code: "LDN" },
      ],
    };
  },
  methods: {},
};
</script>
```

根据`optionLabel`道具的设置，向用户显示`name`。

而`code`是`value`属性的值。

`placeholder`显示为下拉占位符。

我们用`v-model`将该值绑定到`selectedCity`反应属性。

`filter` prop 将呈现一个输入框，让我们过滤选择:

```
<template>
  <div>
    <Dropdown
      v-model="selectedCity"
      :options="cities"
      optionLabel="name"
      placeholder="Select a City"
      filter
    />
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      selectedCity: "",
      cities: [
        { name: "New York", code: "NY" },
        { name: "Miami", code: "MI" },
        { name: "London", code: "LDN" },
      ],
    };
  },
  methods: {},
};
</script>
```

我们可以使用`option`槽定制每个选项的显示方式。

我们可以定制如何使用`value`槽显示所选值:

```
<template>
  <div>
    <Dropdown
      v-model="selectedCity"
      :options="cities"
      optionLabel="name"
      placeholder="Select a City"
      showClear
    >
      <template #value="slotProps">
        <div class="p-dropdown-code">
          <span v-if="slotProps.value">{{ slotProps.value.name }}</span>
          <span v-else>
            {{ slotProps.placeholder }}
          </span>
        </div>
      </template>
      <template #option="slotProps">
        <div class="p-dropdown-name">
          <span v-if="slotProps.option">{{ slotProps.option.name }}</span>
          <span v-else>
            {{ slotProps.placeholder }}
          </span>
        </div>
      </template>
    </Dropdown>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      selectedCity: "",
      cities: [
        { name: "New York", code: "NY" },
        { name: "Miami", code: "MI" },
        { name: "London", code: "LDN" },
      ],
    };
  },
  methods: {},
};
</script>
```

# 富文本编辑器

我们可以使用 PrimeVue 的`Editor`组件将富文本编辑器添加到我们的 Vue 3 应用程序中。

它基于 Quill 富文本编辑器。

为了添加它，我们运行`npm i quill`来安装鹅毛笔编辑器包，然后我们编写:

`main.js`

```
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from "primevue/config";
import Editor from 'primevue/editor';
import 'primevue/resources/primevue.min.css'
import 'primevue/resources/themes/saga-blue/theme.css'
import 'primeicons/primeicons.css'
import 'primeflex/primeflex.css';const app = createApp(App);
app.use(PrimeVue);
app.component("Editor", Editor);
app.mount("#app");
```

`App.vue`

```
<template>
  <div>
    <Editor v-model="value" editorStyle="height: 320px" />
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: "",
    };
  },
  methods: {},
};
</script>
```

我们用`editorStyle`属性为文本编辑器设置样式。

我们输入到编辑器中的值被绑定到带有`v-model`的`value`反应属性。

# 结论

我们可以使用 PrimeVue 轻松地将下拉菜单和富文本编辑器添加到我们的 Vue 3 应用程序中。