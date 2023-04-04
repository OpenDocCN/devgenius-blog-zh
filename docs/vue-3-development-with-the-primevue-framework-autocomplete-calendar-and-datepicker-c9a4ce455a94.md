# 使用 PrimeVue 框架开发 vue 3——自动完成、日历和日期选择器

> 原文：<https://blog.devgenius.io/vue-3-development-with-the-primevue-framework-autocomplete-calendar-and-datepicker-c9a4ce455a94?source=collection_archive---------3----------------------->

![](img/9ca443dd7eec84b0b6e44e9f1c20ea5a.png)

[David Ballew](https://unsplash.com/@daveballew?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

PrimeVue 是一个与 Vue 3 兼容的 UI 框架。

在本文中，我们将了解如何开始使用 PrimeVue 开发 Vue 3 应用程序。

# 自动完成

我们可以添加 PrimeVue 自带的`AutoComplete`来添加一个自动完成下拉菜单。

例如，我们可以写:

`main.js`

```
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from "primevue/config";
import AutoComplete from "primevue/autocomplete";const app = createApp(App);
app.use(PrimeVue);
app.component("AutoComplete", AutoComplete);
app.mount("#app");
```

`App.vue`

```
<template>
  <div>
    <AutoComplete
      v-model="fruit"
      :suggestions="filteredFruits"
      [@complete](http://twitter.com/complete)="search($event)"
      field="fruit"
    >
      <template #item="{ item }">
        <div>
          <div>{{ item }}</div>
        </div>
      </template>
    </AutoComplete>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      fruit: null,
      filteredFruits: ["apple", "orange", "grape"],
      fruits: ["apple", "orange", "grape"],
    };
  },
  methods: {
    search({ query }) {
      if (!query.trim()) {
        this.filteredFruits = [...this.fruits];
        return;
      }
      this.filteredFruits = this.fruits.filter((f) => f.includes(query));
    },
  },
};
</script>
```

在`main.js`中，我们导入了`Autocomplete`组件。

这个道具有一系列的建议

建议项呈现在`item`槽中。

我们监听`complete`事件来运行`search`函数，以获取过滤的项目。

`search`的第一个参数中的`query`属性包含我们输入的搜索查询。

# 日历

PrimeVue 附带了一个日历组件，用于呈现日期选择器。

为了使用它，我们写:

`main.js`

```
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from "primevue/config";
import Calendar from 'primevue/calendar';const app = createApp(App);
app.use(PrimeVue);
app.component("Calendar", Calendar);
app.mount("#app");
```

`App.vue`

```
<template>
  <div>
    <Calendar v-model="value" />
    <p>{{ value }}</p>
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

我们将`Calendar`组件导入并注册到我们的应用程序中。

然后，我们可以使用组件中的`Calendar`组件来呈现一个输入，当我们单击它时，显示一个日期选择器弹出窗口。

我们用`v-model`将我们选择的值绑定到`value`反应属性。

我们可以更改选择模式，让我们选择多个日期或一个日期范围。

例如，如果我们写:

```
<template>
  <div>
    <Calendar v-model="value" selectionMode="multiple" />
    <p>{{ value }}</p>
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

然后我们可以选择多个日期。

`value`是日期数组，而不是单个日期。

如果我们将`selectionMode`改为`'range'`:

```
<template>
  <div>
    <Calendar v-model="value" selectionMode="range" />
    <p>{{ value }}</p>
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

然后我们可以选择 2 个日期，分别是开始日期和结束日期。

我们可以用`dateFormat`按钮改变所选日期的格式:

```
<template>
  <div>
    <Calendar v-model="value" dateFormat="dd.mm.yy" />
    <p>{{ value }}</p>
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

格式代码可以由以下部分组成:

*   `d` —一个月中的某一天(没有前导零)
*   `dd` —一个月中的某一天(2 位数字)
*   `o` —一年中的某一天(无前导零)
*   `oo` —一年中的某一天(3 位数字)
*   `D` —日名短
*   `DD` —日名长
*   `m` —一年中的月份(无前导零)
*   `mm` —一年中的月份(2 位数字)
*   `M` —月名短
*   `MM` —月份名称长
*   `y` —年份(2 位数字)
*   `yy` —年份(4 位数字)
*   `@` — Unix 时间戳(自 1970 年 1 月 1 日以来的毫秒数)
*   `!` — Windows ticks(自 0001 年 1 月 1 日起 100 纳秒)
*   `‘…’` —文字文本
*   `‘’` —单引号
*   其他任何东西——文字文本

# 结论

我们可以使用 PrimeVue 轻松地将自动完成下拉列表和日期选择器添加到 Vue 3 应用程序中。