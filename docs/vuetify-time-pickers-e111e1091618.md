# Vuetify —时间挑选者

> 原文：<https://blog.devgenius.io/vuetify-time-pickers-e111e1091618?source=collection_archive---------3----------------------->

![](img/6dfe5f21b93b96df4beef4646fbf65df.png)

[Icons8 团队](https://unsplash.com/@icons8?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 时间挑选者

我们可以用`v-time-picker`组件添加一个时间选择器。

例如，我们可以这样使用它:

```
<template>
  <v-row justify="space-around">
    <v-time-picker v-model="time" color="green lighten-1"></v-time-picker>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    time: undefined,
  }),
};
</script>
```

我们添加了`v-time-picker`组件来添加时间选择器。

`color`道具设置标题的颜色。

`v-model`具有选择的时间值。

# 有缺陷的

我们可以用`disabled`道具禁用日期选择器:

```
<template>
  <v-row justify="space-around">
    <v-time-picker v-model="time" color="green lighten-1" disabled></v-time-picker>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    time: undefined,
  }),
};
</script>
```

现在我们不能选择一个日期。

# 只读

我们可以使用`readonly`属性将时间选择器设置为只读:

```
<template>
  <v-row justify="space-around">
    <v-time-picker v-model="time" color="green lighten-1" readonly></v-time-picker>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    time: undefined,
  }),
};
</script>
```

我们不会从常规的时间选择器中看到任何风格差异，但我们不能用它来选择时间。

# 24 小时格式

`format`道具让我们改变时间的格式。

要将其更改为 24 小时格式，我们编写:

```
<template>
  <v-row justify="space-around">
    <v-time-picker v-model="time" color="green lighten-1" format="24hr"></v-time-picker>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    time: undefined,
  }),
};
</script>
```

# 允许的次数

可以用`allowed-hours`和`allowed-minutes`道具限制可以拾取的时间:

```
<template>
  <v-row justify="space-around">
    <v-time-picker
      v-model="time"
      :allowed-hours="allowedHours"
      :allowed-minutes="allowedMinutes"
      class="mt-4"
      format="24hr"
      scrollable
      min="9:30"
      max="22:15"
    ></v-time-picker>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    time: "11:15",
  }),
  methods: {
    allowedHours: (v) => v % 2,
    allowedMinutes: (v) => v >= 10 && v <= 50,
  },
};
</script>
```

我们将`allowed-hours`属性设置为`allowedHours`函数。

它让我们返回用户可以选择的时间的条件。

我们可以对分钟和步长使用类似的函数。

`v`参数有小时和分钟。

例如，我们可以写:

```
<template>
  <v-row justify="space-around">
    <v-time-picker v-model="timeStep" :allowed-minutes="allowedStep" class="mt-4" format="24hr"></v-time-picker>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    time: "11:15",
    timeStep: "10:10",
  }),
  methods: {
    allowedStep: (m) => m % 10 === 0,
  },
};
</script>
```

我们有`allowedStep`函数，我们用它作为`allowed-minutes`属性的值。

`m`已经分钟了。

# 结论

我们可以添加一个设置了各种限制的时间选择器。

它的颜色也可以改变。