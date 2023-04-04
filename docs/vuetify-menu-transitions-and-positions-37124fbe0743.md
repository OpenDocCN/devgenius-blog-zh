# 菜单转换和位置

> 原文：<https://blog.devgenius.io/vuetify-menu-transitions-and-positions-37124fbe0743?source=collection_archive---------1----------------------->

![](img/e18f32509e06921656aa650ae02efeff.png)

由 [Abdulla Faiz](https://unsplash.com/@afaiz?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 菜单自定义过渡

自定义过渡可以添加到菜单中。

Vuetify 有 3 个标准转换，分别是`scale`、`slide-x`和`slide-y`。

例如，我们可以写:

```
<template>
  <v-row justify="space-around">
    <v-menu bottom origin="center center" transition="scale-transition">
      <template v-slot:activator="{ on, attrs }">
        <v-btn color="primary" dark v-bind="attrs" v-on="on">Scale Transition</v-btn>
      </template> <v-list>
        <v-list-item v-for="(item, i) in items" :key="i">
          <v-list-item-title>{{ item.title }}</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-menu>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    items: [
      { title: "Click Me" },
      { title: "Click Me 2" },
      { title: "Click Me 3" },
    ],
  }),
};
</script>
```

我们有`transition`道具来设置过渡效果。

`on`对象作为`v-on`的值传入。

这让我们可以监听菜单事件。

和`attrs`是我们传递给`v-bind`的道具，这样我们可以设置道具来显示菜单。

`transition`支柱的其他选择是`slide-x-transition`和`slide-y-transition`。

# 禁用菜单

我们可以用`disabled`道具禁用菜单。

例如，我们可以写:

```
<template>
  <v-row justify="space-around">
    <v-menu bottom origin="center center" transition="scale-transition" disabled>
      <template v-slot:activator="{ on, attrs }">
        <v-btn color="primary" dark v-bind="attrs" v-on="on">Menu</v-btn>
      </template><v-list>
        <v-list-item v-for="(item, i) in items" :key="i">
          <v-list-item-title>{{ item.title }}</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-menu>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    items: [
      { title: "Click Me" },
      { title: "Click Me 2" },
      { title: "Click Me 3" },
    ],
  }),
};
</script>
```

然后按钮不会打开菜单。

# x 偏移

我们可以水平移动菜单使激活器可见。

例如，我们可以写:

```
<template>
  <v-row justify="space-around">
    <v-col cols="12">
      <v-menu bottom origin="center center" transition="scale-transition" offset-x>
        <template v-slot:activator="{ on, attrs }">
          <v-btn color="primary" dark v-bind="attrs" v-on="on">Menu</v-btn>
        </template> <v-list>
          <v-list-item v-for="(item, i) in items" :key="i">
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item>
        </v-list>
      </v-menu>
    </v-col>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    items: [
      { title: "Click Me" },
      { title: "Click Me 2" },
      { title: "Click Me 3" },
    ],
  }),
};
</script>
```

我们用`offset-x`道具将菜单移到按钮的右边。

# y 偏移

同样，我们可以垂直移动菜单。

例如，我们可以写:

```
<template>
  <v-row justify="space-around">
    <v-col cols="12">
      <v-menu bottom origin="center center" transition="scale-transition" offset-y>
        <template v-slot:activator="{ on, attrs }">
          <v-btn color="primary" dark v-bind="attrs" v-on="on">Menu</v-btn>
        </template> <v-list>
          <v-list-item v-for="(item, i) in items" :key="i">
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item>
        </v-list>
      </v-menu>
    </v-col>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    items: [
      { title: "Click Me" },
      { title: "Click Me 2" },
      { title: "Click Me 3" },
    ],
  }),
};
</script>
```

将菜单移动到菜单按钮上方。

# 结论

我们可以添加具有不同过渡和位置的菜单。