# 使用 PrimeVue 框架的 Vue 3 开发—按钮组多重选择和滑块

> 原文：<https://blog.devgenius.io/vue-3-development-with-the-primevue-framework-button-group-multiple-selections-and-sliders-f1e8549a619?source=collection_archive---------8----------------------->

![](img/8f28fe6c3479aa9519701724e8572cdf.png)

[李创锐](https://unsplash.com/@shawn99lee?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

PrimeVue 是一个与 Vue 3 兼容的 UI 框架。

在本文中，我们将了解如何开始使用 PrimeVue 开发 Vue 3 应用程序。

# 按钮组多重选择

我们可以添加一个有多个选择的按钮组。

例如，我们可以写:

```
<template>
  <div>
    <SelectButton
      v-model="selectedCity"
      :options="cities"
      optionLabel="name"
      multiple
    />
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

我们添加了`multiple`属性来支持多重选择。

此外，我们可以通过填充`option`槽来自定义显示的项目:

```
<template>
  <div>
    <SelectButton
      v-model="selectedCity"
      :options="cities"
      optionLabel="name"
      multiple
    >
      <template #option="slotProps">
        <div>
          <b>{{ slotProps.option.name }}</b>
        </div>
      </template>
    </SelectButton>
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

我们从`slotProps.option`插槽中获取选项数据。

# 滑块

PrimeVue 带有一个滑块输入组件。

例如，我们可以写:

`main.js`

```
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from "primevue/config";
import Slider from 'primevue/slider';
import 'primevue/resources/primevue.min.css'
import 'primevue/resources/themes/saga-blue/theme.css'
import 'primeicons/primeicons.css'
import 'primeflex/primeflex.css';const app = createApp(App);
app.use(PrimeVue);
app.component("Slider", Slider);
app.mount("#app");
```

`App.vue`

```
<template>
  <div>
    <Slider v-model="value" />
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: 0,
    };
  },
  methods: {},
};
</script>
```

我们添加了`Slider`组件，并用`v-model`将选择的值绑定到`value`反应属性。

此外，我们可以用`range`道具将其更改为范围滑块:

```
<template>
  <div>
    <Slider v-model="value" range />
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: [20, 80],
    };
  },
  methods: {},
};
</script>
```

我们可以使用`orientation`道具将范围滑块设置为垂直显示:

```
<template>
  <div>
    <Slider v-model="value" orientation="vertical" />
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: 0,
    };
  },
  methods: {},
};
</script>
```

我们可以用`step`道具将滑块值捕捉到最近的增量:

```
<template>
  <div>
    <Slider v-model="value" :step="20" />
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: 0,
    };
  },
  methods: {},
};
</script>
```

此外，我们可以使用`min`和`max`道具设置最小值和最大值:

```
<template>
  <div>
    <Slider v-model="value" :step="20" :min="50" :max="200" />
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: 0,
    };
  },
  methods: {},
};
</script>
```

# 结论

我们可以使用 PrimeVue 在 Vue 3 应用程序中添加允许多重选择和滑块的按钮组。