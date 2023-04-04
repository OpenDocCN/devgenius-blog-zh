# 使用 PrimeVue 框架开发 vue 3——富文本编辑器工具栏、输入组和输入掩码

> 原文：<https://blog.devgenius.io/vue-3-development-with-the-primevue-framework-rich-text-editor-toolbar-input-groups-and-input-f2dc6f21d4f0?source=collection_archive---------2----------------------->

![](img/07d05bf2358ba88935a201fab0301a62.png)

照片由 [Aditya Joshi](https://unsplash.com/@adijoshi11?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

PrimeVue 是一个与 Vue 3 兼容的 UI 框架。

在本文中，我们将了解如何开始使用 PrimeVue 开发 Vue 3 应用程序。

# 富文本编辑器工具栏配置

我们可以通过填充`toolbar`槽来改变 PrimeVue 富文本编辑器工具栏的配置:

```
<template>
  <div>
    <Editor v-model="value" editorStyle="height: 320px">
      <template #toolbar>
        <span class="ql-formats">
          <button class="ql-bold"></button>
          <button class="ql-italic"></button>
          <button class="ql-underline"></button>
        </span>
      </template>
    </Editor>
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

我们添加按钮，让我们在编辑器中设置文本样式。

# 输入组

我们可以通过添加 PrimeVue 框架提供的一些类来添加输入组。

例如，我们可以写:

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
    <div class="p-col-12 p-md-4">
      <div class="p-inputgroup">
        <span class="p-inputgroup-addon">
          <i class="pi pi-user"></i>
        </span>
        <InputText placeholder="Username" />
      </div>
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

我们在`main.js`中注册`InputText`组件。

然后我们添加`p-inputgroup`类，使 div 成为一个输入组容器。

`p-inputgroup-addon`类让我们将项目附加到输入组的输入中。

我们也可以把它加到右边:

```
<template>
  <div>
    <div class="p-col-12 p-md-4">
      <div class="p-inputgroup">
        <InputText placeholder="Username" />
        <span class="p-inputgroup-addon">
          <i class="pi pi-user"></i>
        </span>
      </div>
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

我们也可以添加一个复选框作为输入组插件:

`main.js`

```
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from "primevue/config";
import InputText from 'primevue/inputtext';
import Checkbox from 'primevue/checkbox';
import 'primevue/resources/primevue.min.css'
import 'primevue/resources/themes/saga-blue/theme.css'
import 'primeicons/primeicons.css'
import 'primeflex/primeflex.css';const app = createApp(App);
app.use(PrimeVue);
app.component("InputText", InputText);
app.component("Checkbox", Checkbox);
app.mount("#app");
```

`App.vue`

```
<template>
  <div>
    <div class="p-col-12 p-md-4">
      <div class="p-inputgroup">
        <span class="p-inputgroup-addon">
          <Checkbox v-model="checked" binary />
        </span>
        <InputText placeholder="Username" />
      </div>
    </div>
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

我们用`p-inputgroup-addon`类将`Checkbox`添加到 span 中。

# 输入掩码

PrimeVue 带有`InputMask`组件，允许我们在 Vue 3 应用程序中添加输入遮罩。

例如，我们可以写:

`main.js`

```
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from "primevue/config";
import InputMask from 'primevue/inputmask';
import 'primevue/resources/primevue.min.css'
import 'primevue/resources/themes/saga-blue/theme.css'
import 'primeicons/primeicons.css'
import 'primeflex/primeflex.css';const app = createApp(App);
app.use(PrimeVue);
app.component("InputMask", InputMask);
app.mount("#app");
```

`App.vue`

```
<template>
  <div>
    <InputMask v-model="value" mask="a*-999-a999" />
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

注册并添加`InputMask`组件。

`mask`属性指定了输入可以接受的格式。

我们用`v-model`将输入的值绑定到`value`反应属性。

# 结论

我们可以用 PrimeVue 添加一个带有自定义工具栏、输入组和输入掩码的富文本编辑器。