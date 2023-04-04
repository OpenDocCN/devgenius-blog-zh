# 使用 PrimeVue 框架开发 vue 3——复选框、芯片输入和颜色选择器

> 原文：<https://blog.devgenius.io/vue-3-development-with-the-primevue-framework-checkbox-chips-input-and-color-picker-79bbfd339df6?source=collection_archive---------4----------------------->

![](img/eda595be9988e6c041a1069932134816.png)

照片由 [𝓴𝓘𝓡𝓚 𝕝𝔸𝕀](https://unsplash.com/@laimannung?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

PrimeVue 是一个与 Vue 3 兼容的 UI 框架。

在本文中，我们将了解如何开始使用 PrimeVue 开发 Vue 3 应用程序。

# 检验盒

我们可以通过编写以下内容来添加复选框:

`main.js`

```
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from "primevue/config";
import Checkbox from 'primevue/checkbox';
import 'primevue/resources/primevue.min.css'
import 'primevue/resources/themes/saga-blue/theme.css'
import 'primeicons/primeicons.css'
import 'primeflex/primeflex.css';const app = createApp(App);
app.use(PrimeVue);
app.component("Checkbox", Checkbox);
app.mount("#app");
```

`App.vue`

```
<template>
  <div class="card">
    <div class="p-field-checkbox">
      <Checkbox name="city1" value="Miami" v-model="cities" />
      <label> Miami</label>
    </div>
    <div class="p-field-checkbox">
      <Checkbox name="city" value="Los Angeles" v-model="cities" />
      <label>Los Angeles</label>
    </div>
    <div class="p-field-checkbox">
      <Checkbox name="city" value="New York" v-model="cities" />
      <label>New York</label>
    </div>
    <div class="p-field-checkbox">
      <Checkbox name="city" value="San Francisco" v-model="cities" />
      <label>San Francisco</label>
    </div>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      cities: [],
    };
  },
  methods: {},
};
</script>
```

我们添加了`primevue.min.css`来添加核心 CSS。

`theme.css`有主题 CSS。

我们还需要`primeicons.css`来添加复选框图标。

然后我们注册`Checkbox`组件，让我们使用复选框。

在`App.vue`中，我们添加了`Checkbox`组件。

我们将每一个绑定到同一个反应属性，以便添加多个检查值。

# 芯片输入

我们可以使用 PrimeVue 的`Chips`组件将芯片输入添加到我们的 Vue 3 应用程序中。

例如，我们可以写:

`main.js`

```
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from "primevue/config";
import Chips from 'primevue/chips';
import 'primevue/resources/primevue.min.css'
import 'primevue/resources/themes/saga-blue/theme.css'
import 'primeicons/primeicons.css'
import 'primeflex/primeflex.css';const app = createApp(App);
app.use(PrimeVue);
app.component("Chips", Chips);
app.mount("#app");
```

`App.vue`

```
<template>
  <div>
    <Chips v-model="value">
      <template #chip="slotProps">
        <div>
          <span>{{ slotProps.value }} </span>
          <i class="pi pi-user-plus" style="font-size: 14px"></i>
        </div>
      </template>
    </Chips>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: '',
    };
  },
  methods: {},
};
</script>
```

我们添加了`Chips`组件，并将其绑定到`value`反应属性。

我们可以选择填充`chip`槽来获得我们用`slotProps.value`属性输入的值，并在芯片中显示我们想要的内容。

# 颜色选择器

`ColorPicker`组件让我们添加一个颜色选择器组件。

为了使用它，我们写:

`main.js`

```
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from "primevue/config";
import ColorPicker from 'primevue/colorpicker';
import 'primevue/resources/primevue.min.css'
import 'primevue/resources/themes/saga-blue/theme.css'
import 'primeicons/primeicons.css'
import 'primeflex/primeflex.css';const app = createApp(App);
app.use(PrimeVue);
app.component("ColorPicker", ColorPicker);
app.mount("#app");
```

`App.vue`

```
<template>
  <div>
    <ColorPicker v-model="color" />
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      color: "",
    };
  },
  methods: {},
};
</script>
```

我们注册了`ColorPicker`组件并在`App.vue`中使用它。

我们将带有`v-model`的拾色器颜色值绑定到`color`反应属性。

属性让我们可以内嵌显示颜色选择器预览:

```
<template>
  <div>
    <ColorPicker v-model="color" inline  />
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      color: "",
    };
  },
  methods: {},
};
</script>
```

颜色选择器将显示在页面上，而不是弹出窗口中。

# 结论

我们可以使用 PrimeVue 框架将复选框、芯片输入和颜色选择器添加到 Vue 3 应用程序中。