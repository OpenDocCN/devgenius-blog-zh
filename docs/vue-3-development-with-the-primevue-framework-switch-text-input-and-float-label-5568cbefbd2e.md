# 使用 PrimeVue 框架开发 vue 3——开关、文本输入和浮动标签

> 原文：<https://blog.devgenius.io/vue-3-development-with-the-primevue-framework-switch-text-input-and-float-label-5568cbefbd2e?source=collection_archive---------3----------------------->

![](img/90c2d729bfe101782b555db35bac9b89.png)

照片由 [Aman Tyagi](https://unsplash.com/@loststoner__?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

PrimeVue 是一个与 Vue 3 兼容的 UI 框架。

在本文中，我们将了解如何开始使用 PrimeVue 开发 Vue 3 应用程序。

# 转换

PrimeVue 带有呈现开关的`InputSwitch`组件。

要添加它，我们可以写:

`main.js`

```
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from "primevue/config";
import InputSwitch from 'primevue/inputswitch';
import 'primevue/resources/primevue.min.css'
import 'primevue/resources/themes/saga-blue/theme.css'
import 'primeicons/primeicons.css'
import 'primeflex/primeflex.css';const app = createApp(App);
app.use(PrimeVue);
app.component("InputSwitch", InputSwitch);
app.mount("#app");
```

`App.vue`

```
<template>
  <div>
    <InputSwitch v-model="checked" />
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      checked: false,
    };
  },
  methods: {},
};
</script>
```

我们用`v-model`将开关的值绑定到一个反应性属性。

# 文本输入

我们可以通过使用`InputText`组件添加文本输入。

为了补充它，我们写道:

`main.js`

```
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from "primevue/config";
import InputText from 'primevue/inputtext';
import 'primevue/resources/primevue.min.css'
import 'primevue/resources/themes/saga-blue/theme.css'
import 'primeicons/primeicons.css'
import 'primeflex/primeflex.css';const app = createApp(App);
app.use(PrimeVue);
app.component("InputText", InputText);
app.mount("#app");
```

`App.vue`

```
<template>
  <div>
    <InputText type="text" v-model="value" />
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

我们将输入的值绑定到一个反应属性`v-model`。

此外，我们可以在带有几个类的`i`元素的输入旁边添加图标:

```
<template>
  <div>
    <span class="p-input-icon-left">
      <i class="pi pi-search" />
      <InputText type="text" v-model="value" placeholder="Search" />
    </span>
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

`p-input-icon-left`类让我们在输入框的左侧添加一个图标。

`p-input-icon-right`让我们在输入框的右侧添加一个图标。

`p-input-filled`类让我们给输入框添加一个灰色背景:

```
<template>
  <div class="p-input-filled">
    <InputText type="text" v-model="value" placeholder="Search" />
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

# 浮动标签

我们可以在应用程序中添加浮动标签。

例如，我们可以写:

```
<template>
  <div class="p-fluid p-grid">
    <div class="p-m-6">
      <span class="p-float-label">
        <InputText id="inputtext" type="text" v-model="value" />
        <label for="inputtext">InputText</label>
      </span>
    </div>
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

来补充一下。

我们将`p-float-label`类添加到容器中，以便添加浮动标签。

然后我们在容器中添加`label`元素，使其成为一个浮动标签。

这也适用于`Chips`、`InputMask`、`InputNumber`、`MultiSelect`和`Textarea`组件。

# 结论

我们可以使用 PrimeVue 在 Vue 3 应用程序中添加开关、文本输入和浮动标签。