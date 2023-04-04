# 颜色和日期选择器

> 原文：<https://blog.devgenius.io/vuetify-color-and-date-pickers-ec4c4594239e?source=collection_archive---------5----------------------->

![](img/28c6368b3fc518ea79826fb99ab9b439.png)

照片由 [Brienne Hong](https://unsplash.com/@briennehong?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 颜色选择器输入

我们可以用`hide-input`道具隐藏颜色选择器输入。

例如，我们可以写:

```
<template>
  <v-row justify="space-around">
    <v-color-picker class="ma-2" hide-inputs></v-color-picker>
  </v-row>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({}),
};
</script>
```

现在我们看不到显示的数字。

# 隐藏画布

`hide-canvas`道具隐藏画布:

```
<template>
  <v-row justify="space-around">
    <v-color-picker class="ma-2" hide-canvas></v-color-picker>
  </v-row>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({}),
};
</script>
```

画布的高度可以用`canvas-height`道具设定:

```
<template>
  <v-row justify="space-around">
    <v-color-picker class="ma-2" canvas-height="300"></v-color-picker>
  </v-row>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({}),
};
</script>
```

`dot-size`道具改变画布上的点大小:

```
<template>
  <v-row justify="space-around">
    <v-color-picker class="ma-2" dot-size="30"></v-color-picker>
  </v-row>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({}),
};
</script>
```

# 日期/月份选择器

我们可以用`v-date-picker`组件添加日期或月份选择器。

例如，我们可以写:

```
<template>
  <v-row justify="space-around">
    <v-date-picker v-model="picker" color="green lighten-1"></v-date-picker>
  </v-row>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    picker: new Date().toISOString().substr(0, 10),
  }),
};
</script>
```

我们用`color`道具设置顶栏的颜色。

我们将`v-model`值设置为一个日期字符串。

# 日期选择器-允许的日期

我们可以设置允许选择的最小和最大日期。

例如，我们可以写:

```
<template>
  <v-row justify="space-around">
    <v-date-picker
      v-model="picker"
      color="green lighten-1"
      :allowed-dates="allowedDates"
      class="mt-4"
      min="2020-06-15"
      max="2021-03-20"
    ></v-date-picker>
  </v-row>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    picker: new Date().toISOString().substr(0, 10),
  }), methods: {
    allowedDates: (val) => parseInt(val.split("-")[2], 10) % 2 === 0,
  },
};
</script>
```

使用`allowed-dates`属性将其设置为验证日期的方法。

`val`是字符串形式的日期值。

这样，我们只能选择偶数的日子。

`min`和`max`设置允许选择的最小和最大日期。

# 日期选取器—设置选取器宽度

我们可以用`width`道具设置日期选择器的宽度。

例如，我们可以写:

```
<template>
  <v-row justify="space-around">
    <v-date-picker v-model="picker" width="290" class="mt-4"></v-date-picker>
  </v-row>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    picker: new Date().toISOString().substr(0, 10),
  }),
};
</script>
```

用`width`支柱设定宽度。

此外，我们可以使用`full-width`道具来设置日期选择器的宽度:

```
<template>
  <v-row justify="space-around">
    <v-date-picker v-model="date" full-width :landscape="$vuetify.breakpoint.smAndUp" class="mt-4"></v-date-picker>
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

`full-width`道具使日期选择器填满屏幕的宽度。

# 结论

我们可以用 Vuetify 添加颜色选择器和日期选择器。