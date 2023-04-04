# 使用 PrimeVue 框架开发 vue 3——输入掩码和数字输入

> 原文：<https://blog.devgenius.io/vue-3-development-with-the-primevue-framework-input-mask-and-numeric-inputs-43370abf2073?source=collection_archive---------6----------------------->

![](img/1e97088c026f66832d64182487decf41.png)

由 [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

PrimeVue 是一个与 Vue 3 兼容的 UI 框架。

在本文中，我们将了解如何开始使用 PrimeVue 开发 Vue 3 应用程序。

# 输入掩码

PrimeVue 输入掩码可由以下部分指定:

*   A-字母字符(a-z，A-Z)
*   9-数字字符(0–9)
*   * —字母数字字符(a-z，A-Z，0–9)

我们可以用属性为输入掩码指定占位符:

```
<template>
  <div>
    <InputMask v-model="value" mask="99/99/9999" slotChar="mm/dd/yyyy" />
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

`slotChar`的值显示为占位符。

我们可以用问号指定可选值:

```
<template>
  <div>
    <InputMask v-model="value" mask="(999) 999-9999? x99999" />
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

# 数字输入

PrimeVue 的`InputNumber`组件呈现为数字输入。

为了补充它，我们写道:

`main.js`

```
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from "primevue/config";
import InputNumber from 'primevue/inputnumber';
import 'primevue/resources/primevue.min.css'
import 'primevue/resources/themes/saga-blue/theme.css'
import 'primeicons/primeicons.css'
import 'primeflex/primeflex.css';const app = createApp(App);
app.use(PrimeVue);
app.component("InputNumber", InputNumber);
app.mount("#app");
```

`App.vue`

```
<template>
  <div>
    <InputNumber v-model="value" />
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

我们只能在`InputNumber`组件中输入数字。

此外，我们可以设置`mode`属性让我们输入十进制数:

```
<template>
  <div>
    <InputNumber
      v-model="value"
      mode="decimal"
      :minFractionDigits="2"
      :maxFractionDigits="2"
    />
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

我们添加了`minFractionDigits`和`maxFractionDigits`属性，让我们指定可以输入的十进制位数。

`locale`道具让我们根据地区改变数字的格式:

```
<template>
  <div>
    <InputNumber v-model="value" locale="en-IN" />
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

我们也可以用`currency`属性指定货币:

```
<template>
  <div>
    <InputNumber v-model="value" currency="EUR" />
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

此外，我们可以用`prefix`和`suffix`属性附加前缀或后缀:

```
<template>
  <div>
    <InputNumber v-model="value" prefix="Expires in " suffix=" days" />
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

我们还可以添加按钮，作为带有几个道具的数字输入插件:

```
<template>
  <div>
    <InputNumber
      v-model="value"
      showButtons
      buttonLayout="horizontal"
      decrementButtonClass="p-button-danger"
      incrementButtonClass="p-button-success"
      incrementButtonIcon="pi pi-plus"
      decrementButtonIcon="pi pi-minus"
    />
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

`showButtons`让我们展示一下按钮。

然后我们为增加和减少按钮设置类和图标。

# 结论

我们可以使用 PrimeVue 将输入掩码和数字输入添加到 Vue 3 应用程序中。