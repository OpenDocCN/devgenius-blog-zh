# 使用 PrimeVue 框架开发 vue 3—日期和时间选择器

> 原文：<https://blog.devgenius.io/vue-3-development-with-the-primevue-framework-date-and-time-picker-ccb7e0acb183?source=collection_archive---------1----------------------->

![](img/b52e4c3602485c75c0c7b17b7add208d.png)

安迪·霍姆斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

PrimeVue 是一个与 Vue 3 兼容的 UI 框架。

在本文中，我们将了解如何开始使用 PrimeVue 开发 Vue 3 应用程序。

# 日期和时间选择器

我们可以用`showTime`属性添加一个日期和时间选择器:

```
<template>
  <div>
    <Calendar v-model="value" showTime />
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

`showTime`用日期选择器添加时间选择器。

我们可以用`hourFormat`道具改变时间格式:

```
<template>
  <div>
    <Calendar v-model="value" showTime hourFormat="12" />
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

我们可以把它改成`12`或`24`小时制。

我们可以限制使用`minDate`和`maxDate`按钮选择的日期:

```
<template>
  <div>
    <Calendar v-model="value" :minDate="minDateValue" :maxDate="maxDateValue" />
    <p>{{ value }}</p>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: null,
      minDateValue: new Date(2020, 11, 1),
      maxDateValue: new Date(2020, 11, 10),
    };
  },
  methods: {},
};
</script>
```

我们也可以使用`disableDates`属性禁用日期选择:

```
<template>
  <div>
    <Calendar v-model="value" :disabledDates="invalidDates" />
    <p>{{ value }}</p>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: null,
      invalidDates: [new Date(2020, 11, 1), new Date(2020, 11, 10)],
    };
  },
  methods: {},
};
</script>
```

我们还可以按星期几禁用日期:

```
<template>
  <div>
    <Calendar v-model="value" :disabledDays="[0, 6]" />
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

0 是星期天，6 是星期六。

此外，我们可以在日期选择器的底部添加一个按钮，让用户使用`showButtonBar` prop 选择日期:

```
<template>
  <div>
    <Calendar v-model="value" showButtonBar />
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

我们还可以使用`numberOfMonths`属性在日期选择器中显示多个月:

```
<template>
  <div>
    <Calendar v-model="value" :numberOfMonths="3" />
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

我们可以编写自己的页眉和页脚内容:

```
<template>
  <div>
    <Calendar v-model="value">
      <template #header>Header</template>
      <template #footer>Footer</template>
    </Calendar>
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

我们可以自定义日期在默认时间段的显示方式:

```
<template>
  <div>
    <Calendar v-model="value">
      <template #date="slotProps">
        <strong
          v-if="slotProps.date.day > 10 && slotProps.date.day < 15"
          class="special-day"
          >{{ slotProps.date.day }}</strong
        >
        <template v-else>{{ slotProps.date.day }}</template>
      </template>
    </Calendar>
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

`slotProps.date.day`获取日数值。

# 结论

我们可以使用 PrimeVue 框架将带有各种定制的日期选择器添加到我们的 Vue 3 应用程序中。