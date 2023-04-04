# 使用 PrimeVue 框架开发 vue 3——文本区、三种状态的复选框和按钮

> 原文：<https://blog.devgenius.io/vue-3-development-with-the-primevue-framework-textarea-checkbox-with-three-states-and-button-8ab46578b28a?source=collection_archive---------0----------------------->

![](img/569f409e12b4002ad15a4cd2936aa4c0.png)

[千斤顶花](https://unsplash.com/@jhf?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

PrimeVue 是一个与 Vue 3 兼容的 UI 框架。

在本文中，我们将了解如何开始使用 PrimeVue 开发 Vue 3 应用程序。

# 文本区域

PrimeVue 附带了`Textarea`组件，允许我们在 Vue 3 应用程序中添加一个文本区域。

为了补充它，我们写道:

`main.js`

```
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from "primevue/config";
import Textarea from 'primevue/textarea';
import 'primevue/resources/primevue.min.css'
import 'primevue/resources/themes/saga-blue/theme.css'
import 'primeicons/primeicons.css'
import 'primeflex/primeflex.css';const app = createApp(App);
app.use(PrimeVue);
app.component("Textarea", Textarea);
app.mount("#app");
```

`App.vue`

```
<template>
  <div>
    <Textarea v-model="value" autoResize rows="5" cols="30" />
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

`autoResize`道具会根据输入的文本自动调整大小。

`rows`和`cols`分别设置显示的行数和列数。

# 开关按钮

我们可以添加`ToggleButton`组件，让我们在 Vue 3 应用程序中添加一个切换按钮。

例如，我们可以写:

```
<template>
  <div>
    <ToggleButton
      v-model="checked"
      onLabel="yes"
      offLabel="no"
      onIcon="pi pi-check"
      offIcon="pi pi-times"
    />
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

添加一个绑定到`checked`反应属性的切换按钮。

`onLabel`具有当`checked`为`true`时显示的文本。

`offLabel`具有当`checked`为`false`时显示的文本。

`onIcon`具有当`checked`为`true`时应用的图标类。

`offIcon`具有当`checked`为`false`时应用的图标类。

# 具有三种状态的复选框

除了使用`TriStateCheckbox`组件的选中和未选中状态之外，我们还可以添加一个具有不确定状态的复选框。

为了补充它，我们写道:

`main.js`

```
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from "primevue/config";
import TriStateCheckbox from 'primevue/tristatecheckbox';
import 'primevue/resources/primevue.min.css'
import 'primevue/resources/themes/saga-blue/theme.css'
import 'primeicons/primeicons.css'
import 'primeflex/primeflex.css';const app = createApp(App);
app.use(PrimeVue);
app.component("TriStateCheckbox", TriStateCheckbox);
app.mount("#app");
```

`App.vue`

```
<template>
  <div>
    <TriStateCheckbox v-model="value" />
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: false,
    };
  },
  methods: {},
};
</script>
```

来补充一下。

# 小跟班

PrimeVue 带有各种样式的按钮。

要添加一个基本按钮，我们可以写:

`main.js`

```
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from "primevue/config";
import Button from 'primevue/button';
import 'primevue/resources/primevue.min.css'
import 'primevue/resources/themes/saga-blue/theme.css'
import 'primeicons/primeicons.css'
import 'primeflex/primeflex.css';const app = createApp(App);
app.use(PrimeVue);
app.component("Button", Button);
app.mount("#app");
```

`App.vue`

```
<template>
  <div>
    <Button label="Submit" />
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

属性是显示给用户的文本。

我们可以通过编写以下内容向按钮添加图标:

```
<template>
  <div>
    <Button label="Submit" icon="pi pi-check" iconPos="right" />
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

有我们想要添加的图标的类。

`iconPos`有图标的位置。

# 结论

我们可以使用 PrimeVue 在 Vue 3 应用程序中添加文本区域、切换按钮、3 状态复选框和按钮。