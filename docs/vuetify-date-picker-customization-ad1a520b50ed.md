# Vuetify —日期选取器自定义

> 原文：<https://blog.devgenius.io/vuetify-date-picker-customization-ad1a520b50ed?source=collection_archive---------6----------------------->

![](img/decf63ee9f3a7c4cbd7ad64d755e498c.png)

由 [Wiktor Karkocha](https://unsplash.com/@rotkif?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 日期选取器的高度

我们可以用`flat`和`elevation`道具设置日期选择器的高度。

要使用`flat`这个道具，我们可以写:

```
<template>
  <v-row justify="space-around">
    <v-date-picker v-model="date" flat></v-date-picker>
  </v-row>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    date: new Date().toISOString().substr(0, 10),
  }),
};
</script>
```

把它弄平。

我们可以写:

```
<template>
  <v-row justify="space-around">
    <v-date-picker v-model="date" elevation="15"></v-date-picker>
  </v-row>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    date: new Date().toISOString().substr(0, 10),
  }),
};
</script>
```

在日期选取器下方添加阴影。

# 日期选择器—对显示的月/年变化做出反应

我们可以用自己的观察器观察`v-model`值。

例如，我们可以写:

```
<template>
  <v-row>
    <v-col cols="12" sm="6" class="my-2 px-1">
      <v-date-picker ref="picker" v-model="date" :picker-date.sync="pickerDate" full-width></v-date-picker>
    </v-col>
    <v-col cols="12" sm="6" class="my-2 px-1">
      <div class="title">Month ({{ date || 'change month...' }})</div>
      <ul class="ma-4">
        <li v-for="note in notes" :key="note">{{ note }}</li>
      </ul>
    </v-col>
  </v-row>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    date: new Date().toISOString().substr(0, 10),
    pickerDate: null,
    notes: [],
    allNotes: ["foo", "bar", "baz"],
  }),
  watch: {
    pickerDate(val) {
      this.notes = [
        this.allNotes[Math.floor(Math.random() * 3)],
        this.allNotes[Math.floor(Math.random() * 3)],
      ];
    },
  },
};
</script>
```

`picker-date`道具有选择的月份值。

然后我们可以观察`pickerDate`值，并随着月份值的变化做我们想做的事情。

# 日期选择器国际化

Vuetify 日期选择器支持国际化。

例如，我们可以写:

```
<template>
  <v-row justify="space-around">
    <v-date-picker v-model="date" :first-day-of-week="0" locale="zh-cn"></v-date-picker>
  </v-row>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    date: new Date().toISOString().substr(0, 10),
  }),
};
</script>
```

我们有`locales`和日期选择器的区域设置。

属性将一周的第一天设置为我们想要的那一天。

0 是星期天，它上升到 6 是星期六。

# 日期选择器图标

我们可以根据需要设置日期选择器图标。

例如，我们可以写:

```
<template>
  <v-row justify="space-around">
    <v-date-picker
      v-model="date"
      year-icon="mdi-calendar-blank"
      prev-icon="mdi-skip-previous"
      next-icon="mdi-skip-next"
    ></v-date-picker>
  </v-row>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    date: new Date().toISOString().substr(0, 10),
  }),
};
</script>
```

我们改变`year-icon`、`prev-icon`和`next-icon`让我们分别改变年份和导航图标。

# 只读日期选取器

我们可以使用`readonly`属性将日期选择器设置为只读:

```
<template>
  <v-row justify="center">
    <v-date-picker v-model="date" readonly></v-date-picker>
  </v-row>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    date: new Date().toISOString().substr(0, 10),
  }),
};
</script>
```

现在我们不能从日期选择器中选择任何内容。

# 结论

我们可以用 Vuetify 自定义日期选择器。