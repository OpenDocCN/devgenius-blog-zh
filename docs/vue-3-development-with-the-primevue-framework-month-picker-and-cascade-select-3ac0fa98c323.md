# 使用 PrimeVue 框架开发 vue 3—月份选择器和级联选择

> 原文：<https://blog.devgenius.io/vue-3-development-with-the-primevue-framework-month-picker-and-cascade-select-3ac0fa98c323?source=collection_archive---------5----------------------->

![](img/7e147a591ecbddc15976829a41ecef16.png)

汉斯·维维克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

PrimeVue 是一个与 Vue 3 兼容的 UI 框架。

在本文中，我们将了解如何开始使用 PrimeVue 开发 Vue 3 应用程序。

# 月份选择器

我们可以通过编写以下内容来添加月份选择器:

```
<template>
  <div>
    <Calendar
      v-model="value"
      view="month"
      dateFormat="mm/yy"
      yearNavigator
      yearRange="2000:2025"
    /><p>{{ value }}</p>
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
</script><style scoped>
.special-day {
  font-style: italic;
}
</style>
```

我们添加了`yearNavigator`属性，让我们添加一个下拉菜单来选择年份。

`yearRange`设置我们可以选择的年份。

`dateFormat`格式化要在输入中显示的日期。

`view`设置日期选择器的查看模式。

`touchUI`道具让我们可以通过触摸来操作日期选择器:

```
<template>
  <div>
    <Calendar v-model="value" touchUI />
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
</script><style scoped>
.special-day {
  font-style: italic;
}
</style>
```

# 级联选择

我们可以使用 CascadeSelect 组件来显示选项的嵌套结构。

为了补充它，我们写道:

`main.js`

```
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from "primevue/config";
import CascadeSelect from 'primevue/cascadeselect';
import 'primevue/resources/themes/bootstrap4-light-blue/theme.css'
import 'primeicons/primeicons.css'const app = createApp(App);
app.use(PrimeVue);
app.component("CascadeSelect", CascadeSelect);
app.mount("#app");
```

`App.vue`

```
<template>
  <div>
    <CascadeSelect
      v-model="selectedCity"
      :options="countries"
      optionLabel="cname"
      optionGroupLabel="name"
      :optionGroupChildren="['states', 'cities']"
      style="minwidth: 14rem"
    />
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      selectedCity: null,
      countries: [
        {
          name: "Canada",
          code: "CA",
          states: [
            {
              name: "Quebec",
              cities: [
                { cname: "Montreal", code: "C-MO" },
                { cname: "Quebec City", code: "C-QU" },
              ],
            },
            {
              name: "Ontario",
              cities: [
                { cname: "Ottawa", code: "C-OT" },
                { cname: "Toronto", code: "C-TO" },
              ],
            },
          ],
        },
        {
          name: "United States",
          code: "US",
          states: [
            {
              name: "California",
              cities: [
                { cname: "Los Angeles", code: "US-LA" },
                { cname: "San Francisco", code: "US-SF" },
              ],
            },
            {
              name: "Florida",
              cities: [
                { cname: "Jacksonville", code: "US-JA" },
                { cname: "Miami", code: "US-MI" },
              ],
            },
            {
              name: "Texas",
              cities: [
                { cname: "Austin", code: "US-AU" },
                { cname: "Dallas", code: "US-DA" },
              ],
            },
          ],
        },
      ],
    };
  },
  methods: {},
};
</script>
```

我们将`options`属性设置为`countries`，这是一个对象数组。

`optionLabel`有选项标签的属性名。

`optionGroupLabel`有下拉标签的属性名。

我们可以使用`option`槽定制选项的显示方式:

```
<template>
  <div>
    <CascadeSelect
      v-model="selectedCity"
      :options="countries"
      optionLabel="cname"
      optionGroupLabel="name"
      :optionGroupChildren="['states', 'cities']"
      style="minwidth: 14rem"
    >
      <template #option="slotProps">
        <div>
          <i class="pi pi-compass p-mr-3" v-if="slotProps.option.cities"></i>
          <i class="pi pi-map-marker p-mr-3" v-if="slotProps.option.cname"></i>
          <span>{{ slotProps.option.cname || slotProps.option.name }}</span>
        </div>
      </template>
    </CascadeSelect>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      selectedCity: null,
      countries: [
        {
          name: "Canada",
          code: "CA",
          states: [
            {
              name: "Quebec",
              cities: [
                { cname: "Montreal", code: "C-MO" },
                { cname: "Quebec City", code: "C-QU" },
              ],
            },
            {
              name: "Ontario",
              cities: [
                { cname: "Ottawa", code: "C-OT" },
                { cname: "Toronto", code: "C-TO" },
              ],
            },
          ],
        },
        {
          name: "United States",
          code: "US",
          states: [
            {
              name: "California",
              cities: [
                { cname: "Los Angeles", code: "US-LA" },
                { cname: "San Francisco", code: "US-SF" },
              ],
            },
            {
              name: "Florida",
              cities: [
                { cname: "Jacksonville", code: "US-JA" },
                { cname: "Miami", code: "US-MI" },
              ],
            },
            {
              name: "Texas",
              cities: [
                { cname: "Austin", code: "US-AU" },
                { cname: "Dallas", code: "US-DA" },
              ],
            },
          ],
        },
      ],
    };
  },
  methods: {},
};
</script>
```

`slotProps.option`有选项对象。

# 结论

我们可以在我们的 Vue 3 应用程序中添加下拉菜单，这些下拉菜单与级联选择组件的嵌套条目一起工作。

此外，我们可以使用 PrimeVue 将月份选择器添加到 Vue 3 应用程序中。