# 虚拟化—小吃条和迷你图

> 原文：<https://blog.devgenius.io/vuetify-snackbars-and-sparklines-6598e0d4c8be?source=collection_archive---------1----------------------->

![](img/df4650b4bc7ef2bd225892b1db57d1cd.png)

Stephanie McCabe 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# Snackbar 超时

我们可以在 snackbar 上添加一个`timeout`道具，以便在它自动关闭时进行更改:

```
<template>
  <div class="text-center">
    <v-btn dark color="red darken-2" @click="snackbar = true">Open Snackbar</v-btn> <v-snackbar v-model="snackbar" :multi-line="multiLine" :timeout="3000">
      {{ text }}
      <template v-slot:action="{ attrs }">
        <v-btn color="red" text v-bind="attrs" @click="snackbar = false">Close</v-btn>
      </template>
    </v-snackbar>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    multiLine: true,
    snackbar: false,
    text:
      "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Morbi a neque arcu. Phasellus ac tincidunt elit.",
  }),
};
</script>
```

超时以毫秒为单位。

# 垂直拉杆

我们可以用`vertical`属性制作一个垂直的 snackbar。

例如，我们可以写:

```
<template>
  <div class="text-center">
    <v-btn dark color="red darken-2" @click="snackbar = true">Open Snackbar</v-btn> <v-snackbar v-model="snackbar" :multi-line="multiLine" vertical>
      {{ text }}
      <template v-slot:action="{ attrs }">
        <v-btn color="red" text v-bind="attrs" @click="snackbar = false">Close</v-btn>
      </template>
    </v-snackbar>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    multiLine: true,
    snackbar: false,
    text:
      "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Morbi a neque arcu. Phasellus ac tincidunt elit.",
  }),
};
</script>
```

现在关闭按钮将显示在文本下方。

# 变体

零食有不同的种类。我们可以添加`text`、`shaped`或`outlined`道具来改变 snackbar 变体。

例如，我们可以写:

```
<template>
  <div class="text-center">
    <v-btn dark color="red darken-2" @click="snackbar = true">Open Snackbar</v-btn> <v-snackbar v-model="snackbar" :multi-line="multiLine" rounded="pill">
      {{ text }}
      <template v-slot:action="{ attrs }">
        <v-btn color="red" text v-bind="attrs" @click="snackbar = false">Close</v-btn>
      </template>
    </v-snackbar>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    multiLine: true,
    snackbar: false,
    text:
      "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Morbi a neque arcu. Phasellus ac tincidunt elit.",
  }),
};
</script>
```

我们添加了值为`pill`的`rounded`道具来使角变圆。

# 迷你图

迷你图是我们可以显示的小图形。

我们可以用`v-sparkline`组件添加迷你图:

```
<template>
  <v-container fluid>
    <v-sparkline
      :fill="fill"
      :gradient="gradient"
      :line-width="width"
      :padding="padding"
      :smooth="radius || false"
      :value="value"
      auto-draw
    ></v-sparkline>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    gradient: ["red", "orange", "yellow"],
    padding: 8,
    radius: 10,
    value: [0, 2, 5, 9, 5, 10, 3, 5, 0, 0, 1, 8, 2, 9, 0],
    width: 2,
  }),
};
</script>
```

我们有了带有图形渐变颜色的`gradient`道具。

`value`属性具有直线的 y 坐标值。

`line-width`是线条的宽度。

`padding`是图形的填充。

`smooth`是线条的边界半径。

所有长度都以像素为单位。

# 结论

我们可以添加迷你图来显示图表。

Snaackbars 有各种选项，我们可以更改。