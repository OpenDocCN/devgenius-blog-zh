# Vuetify —月摘者

> 原文：<https://blog.devgenius.io/vuetify-month-pickers-f25fcf6c9cdf?source=collection_archive---------6----------------------->

![](img/cc355fdb3b4bef34e3d0c092561708bd.png)

照片由[里奇·史密斯](https://unsplash.com/@richwilliamsmith?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 月份选择器-允许的月份

我们可以用日期选择器更改允许的月份。

例如，我们可以写:

```
<template>
  <v-row justify="space-around">
    <v-date-picker
      v-model="date"
      :allowed-dates="allowedMonths"
      type="month"
      class="mt-4"
      min="2020-06"
      max="2020-10"
    ></v-date-picker>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    date: new Date().toISOString().substr(0, 10),
  }),
  methods: {
    allowedMonths: (val) => parseInt(val.split("-")[1], 10) % 2 === 0,
  },
};
</script>
```

我们有`allowedMonths`方法来返回允许日期的条件。

此外，我们有`min`和`max`属性来设置日期范围。

# 多月采摘者

我们可以允许月份选择器让用户选择多个月份。

例如，我们可以写:

```
<template>
  <v-row justify="center">
    <v-date-picker v-model="months" type="month" multiple></v-date-picker>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    months: ["2020-09", "2020-10"],
  }),
};
</script>
```

让我们选择几个月。

`multiple`道具允许多重选择。

`v-model`的值是日期字符串数组，而不是日期字符串。

# 月份选择器—设置选择器宽度

我们可以设置月份选择器的宽度。

`width`支柱设定宽度:

```
<template>
  <v-row justify="center">
    <v-date-picker v-model="date" type="month" width="290" class="mt-4"></v-date-picker>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    date: new Date().toISOString().substr(0, 7),
  }),
};
</script>
```

此外，我们可以设置`full-width`道具让月份选择器充满屏幕:

```
<template>
  <v-row justify="center">
    <v-date-picker v-model="date" type="month" full-width class="mt-4"></v-date-picker>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    date: new Date().toISOString().substr(0, 7),
  }),
};
</script>
```

# 月份选择器国际化

属性让我们设置月份选择器的语言。

例如，我们可以写:

```
<template>
  <v-row justify="space-around">
    <v-date-picker v-model="date" type="month" locale="sv-se"></v-date-picker>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    date: new Date().toISOString().substr(0, 7),
  }),
};
</script>
```

我们用`locale`道具设置月份选择器以瑞典语显示。

# 月份选择器图标

月份选择器图标让我们用各种道具来设置图标。

例如，我们可以写:

```
<template>
  <v-row justify="center">
    <v-date-picker
      v-model="date"
      type="month"
      year-icon="mdi-calendar-blank"
      prev-icon="mdi-skip-previous"
      next-icon="mdi-skip-next"
    ></v-date-picker>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    date: new Date().toISOString().substr(0, 7),
  }),
};
</script>
```

我们设置`year-icon`、`prev-icon`和`next-icon`道具来设置年份和导航图标。

# 结论

我们可以用各种道具来设置月份选择器组件，以对它们进行样式化。