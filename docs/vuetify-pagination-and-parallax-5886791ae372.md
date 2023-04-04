# 分页和视差

> 原文：<https://blog.devgenius.io/vuetify-pagination-and-parallax-5886791ae372?source=collection_archive---------4----------------------->

![](img/99158fe443e3071dd944fd13d5ceacc4.png)

照片由[埃胡德·纽豪斯](https://unsplash.com/@paramir?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 分页

我们可以用`v-pagination`组件添加分页链接。

该组件由`v-model`指令控制。

例如，我们可以写:

```
<template>
  <div class="text-center">
    <v-container>
      <v-row justify="center">
        <v-col cols="8">
          <v-container class="max-width">
            <v-pagination v-model="page" class="my-4" :length="15"></v-pagination>
          </v-container>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    page: 1,
  }),
};
</script>
```

添加分页链接。

`v-pagination`组件有`length`道具来设置显示多少个按钮。

`v-model`有模式获取和设置`page`的状态。

# 限制链接数量

我们可以用`total-visible`属性设置可见页面的最大数量。

例如，我们可以写:

```
<template>
  <div class="text-center">
    <v-container>
      <v-row justify="center">
        <v-col cols="8">
          <v-container class="max-width">
            <v-pagination v-model="page" class="my-4" :length="15" :total-visible="8"></v-pagination>
          </v-container>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    page: 1,
  }),
};
</script>
```

我们有`total-visible`属性来设置要显示的按钮的最大数量。

# 圆形按钮

`circle`道具把按钮变成圆圈。

例如，我们可以写:

```
<template>
  <div class="text-center">
    <v-container>
      <v-row justify="center">
        <v-col cols="8">
          <v-pagination v-model="page" :length="5" circle></v-pagination>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    page: 1,
  }),
};
</script>
```

让按钮转圈。

# 核标准情报中心

`prev-icon`和`next-icon`道具让我们改变上一个和下一个按钮图标:

```
<template>
  <div class="text-center">
    <v-pagination v-model="page" :length="4" prev-icon="mdi-menu-left" next-icon="mdi-menu-right"></v-pagination>
  </div>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    page: 1,
  }),
};
</script>
```

# 有缺陷的

我们可以使用`disabled`属性禁用分页按钮:

```
<template>
  <div class="text-center">
    <v-pagination :length="3" disabled></v-pagination>
  </div>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    page: 1,
  }),
};
</script>
```

# 视差

`v-parallax`组件让我们创建一个 3D 效果，使图像看起来比窗口滚动得慢。

例如，我们可以这样使用它:

```
<template>
  <v-parallax dark src="https://cdn.vuetifyjs.com/images/backgrounds/vbanner.jpg">
    <v-row align="center" justify="center">
      <v-col class="text-center" cols="12">
        <h1 class="display-1 font-weight-thin mb-4">Hello world</h1>
        <h4 class="subheading">Lorem ipsum dolor sit amet, consectetur adipiscing elit.</h4>
      </v-col>
    </v-row>
  </v-parallax>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    page: 1,
  }),
};
</script>
```

使用`src`道具来设置背景图片的 URL。

默认槽具有前景内容。

# 结论

我们有`v-pagination`组件来添加分页按钮。

我们有`v-parallax`组件来添加视差滚动效果。