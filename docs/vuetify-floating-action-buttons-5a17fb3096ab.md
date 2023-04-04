# 虚拟化—浮动操作按钮

> 原文：<https://blog.devgenius.io/vuetify-floating-action-buttons-5a17fb3096ab?source=collection_archive---------1----------------------->

![](img/7d2cf803b654415a6b1d0030f0280e9f.png)

照片由[维达尔·诺德里-马西森](https://unsplash.com/@vidarnm?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 带快速拨号的浮动操作按钮

我们可以用`v-speed-dial`组件添加一个带有快速拨号的浮动动作按钮。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row class="text-center">
      <v-col col="12">
        <v-card id="create">
          <v-container fluid style='height: 300px'></v-container>
          <v-speed-dial
            v-model="fab"
            bottom
            direction="top"
            :open-on-hover="false"
            transition="slide-y-reverse-transition"
          >
            <template v-slot:activator>
              <v-btn v-model="fab" color="blue darken-2" dark fab>
                <v-icon v-if="fab">mdi-close</v-icon>
                <v-icon v-else>mdi-account-circle</v-icon>
              </v-btn>
            </template>
            <v-btn fab dark small color="green">
              <v-icon>mdi-pencil</v-icon>
            </v-btn>
            <v-btn fab dark small color="indigo">
              <v-icon>mdi-plus</v-icon>
            </v-btn>
            <v-btn fab dark small color="red">
              <v-icon>mdi-delete</v-icon>
            </v-btn>
          </v-speed-dial>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    fab: false,
  }),
};
</script>
```

我们有`v-container`组件来分隔按钮。

`activator`流有一个浮动的动作按钮。

`v-model`控制快速拨号显示。

当我们点击按钮时,`fab`的值会改变。

`v-speed-dial`显示也由`fab`状态控制。

当`fab`为`true`时会显示。

`bottom`道具使其显示在底部。

`direction`改变快速拨号显示的方向。

`open-on-hover`让我们控制是否在悬停时打开快速拨号。

`transition`让我们改变过渡效果。

# 横向屏幕

当我们改变标签时，我们可以显示不同的浮动动作按钮。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row class="text-center">
      <v-col col="12">
        <v-card id="lateral">
          <v-toolbar dark tabs flat color="indigo">
            <v-app-bar-nav-icon></v-app-bar-nav-icon>
            <v-toolbar-title>Page title</v-toolbar-title>
            <v-spacer></v-spacer>
            <v-btn icon>
              <v-icon>mdi-magnify</v-icon>
            </v-btn>
            <v-btn icon>
              <v-icon>mdi-dots-vertical</v-icon>
            </v-btn>
            <template v-slot:extension>
              <v-tabs v-model="tabs" align-with-title>
                <v-tab href="#one">Item One</v-tab>
                <v-tab href="#two">Item Two</v-tab>
                <v-tab href="#three">Item Three</v-tab>
                <v-tabs-slider color="pink"></v-tabs-slider>
              </v-tabs>
            </template>
          </v-toolbar>
          <v-card-text>
            <v-tabs-items v-model="tabs">
              <v-tab-item
                v-for="content in ['one', 'two', 'three']"
                :key="content"
                :value="content"
              >
                <v-card height="200px" flat></v-card>
              </v-tab-item>
            </v-tabs-items>
          </v-card-text>
          <v-fab-transition>
            <v-btn
              :key="activeFab.icon"
              :color="activeFab.color"
              fab
              large
              dark
              bottom
              left
              class="v-btn--example"
            >
              <v-icon>{{ activeFab.icon }}</v-icon>
            </v-btn>
          </v-fab-transition>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    fab: false,
    hidden: false,
    tabs: null,
  }), computed: {
    activeFab() {
      switch (this.tabs) {
        case "one":
          return { color: "success", icon: "mdi-pencil" };
        case "two":
          return { color: "red", icon: "mdi-plus" };
        case "three":
          return { color: "green", icon: "mdi-delete" };
        default:
          return {};
      }
    },
  },
};
</script>
```

我们有`color`道具来改变颜色。

`v-icon`有我们的图标。

我们使用一个`computed`属性来计算浮动动作按钮图标的值和要显示的颜色。

`large`道具使按钮变大。

`dark`使按钮变暗。

`bottom`和`left`使按钮显示在左下角。

![](img/c8b893fd8236cfd289fe55741c026a9d.png)

照片由[分享网格](https://unsplash.com/@sharegrid?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

浮动操作按钮可以静态和动态显示。

我们也可以用它们来触发快速拨号。