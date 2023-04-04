# 使用 PrimeVue 框架的 Vue 3 开发—单选按钮、评级输入和选择按钮组

> 原文：<https://blog.devgenius.io/vue-3-development-with-the-primevue-framework-radio-button-rating-input-and-select-button-group-8a52436844ad?source=collection_archive---------2----------------------->

![](img/eeb2ac738d6956cbb26ca6955717bf45.png)

照片由 [Rhys Moult](https://unsplash.com/@rhysatwork?utm_source=medium&utm_medium=referral) 在[unsprash](https://unsplash.com?utm_source=medium&utm_medium=referral)上拍摄

PrimeVue 是一个与 Vue 3 兼容的用户界面框架。

在本文中，我们将了解如何使用 PrimeVue 开始开发 Vue 3 应用程序。

# 单选按钮

我们可以用 PrimeVue 的`RadioButton`组件将单选按钮添加到我们的 Vue 3 应用程序中。

为此，我们写道:

`main.js`

```
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from "primevue/config";
import RadioButton from 'primevue/radiobutton';
import 'primevue/resources/primevue.min.css'
import 'primevue/resources/themes/saga-blue/theme.css'
import 'primeicons/primeicons.css'
import 'primeflex/primeflex.css';const app = createApp(App);
app.use(PrimeVue);
app.component("RadioButton", RadioButton);
app.mount("#app");
```

`App.vue`

```
<template>
  <div>
    <div class="p-field-radiobutton">
      <RadioButton id="city" name="city" value="Los Angeles" v-model="city" />
      <label for="city">Los Angeles</label>
    </div>
    <div class="p-field-radiobutton">
      <RadioButton id="city2" name="city" value="New York" v-model="city" />
      <label for="city2">New York</label>
    </div>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      city: null,
    };
  },
  methods: {},
};
</script>
```

我们在`main.js`注册`RadioButton`组件

然后我们将它们添加到`App`组件中。

我们将两者绑定到相同的活性属性，从而将它们设置为所选属性的`value`属性。

`p-field-radiobutton`类允许我们添加带有标签的单选按钮。

# 评级输入

我们可以使用`Rating`组件将评级输入添加到我们的 Vue 3 应用程序中。

例如，我们可以写:

`main.js`

```
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from "primevue/config";
import Rating from 'primevue/rating';
import 'primevue/resources/primevue.min.css'
import 'primevue/resources/themes/saga-blue/theme.css'
import 'primeicons/primeicons.css'
import 'primeflex/primeflex.css';const app = createApp(App);
app.use(PrimeVue);
app.component("Rating", Rating);
app.mount("#app");
```

`App.vue`

```
<template>
  <div>
    <Rating v-model="val" />
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      val: null,
    };
  },
  methods: {},
};
</script>
```

我们可以通过`stars`道具改变显示的星星数量:

```
<template>
  <div>
    <Rating v-model="val" :stars="8" />
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      val: null,
    };
  },
  methods: {},
};
</script>
```

此外，我们可以通过将`cancel`设置为`false`来移除用于重置值的取消图标:

```
<template>
  <div>
    <Rating v-model="val" :stars="8" :cancel="false" />
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      val: null,
    };
  },
  methods: {},
};
</script>
```

# 选择按钮组

PrimeVue 附带了`SelectButton`组件，让我们添加一个按钮组。

我们可以点击一个值来选择它。

例如，我们可以写:

`main.js`

```
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from "primevue/config";
import SelectButton from 'primevue/selectbutton';
import 'primevue/resources/primevue.min.css'
import 'primevue/resources/themes/saga-blue/theme.css'
import 'primeicons/primeicons.css'
import 'primeflex/primeflex.css';const app = createApp(App);
app.use(PrimeVue);
app.component("SelectButton", SelectButton);
app.mount("#app");
```

`App.vue`

```
<template>
  <div>
    <SelectButton v-model="selectedCity" :options="cities" optionLabel="name" />
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      selectedCity: null,
      cities: [
        { name: "London", code: "LND" },
        { name: "Paris", code: "PRS" },
        { name: "Rome", code: "RM" },
      ],
    };
  },
  methods: {},
};
</script>
```

我们用`options`道具添加`SelectButton`组件，为按钮组设置选项。

我们用`v-model`指令将所选值绑定到`selectedCity`反应性上。

`optionLabel`被设置为属性名，其值是我们想要显示给用户的。

# 结论

我们可以在带有 PrimeVue 的 Vue 3 应用程序中添加单选按钮、评级输入和按钮组。