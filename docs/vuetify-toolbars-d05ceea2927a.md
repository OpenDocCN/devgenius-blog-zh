# 虚拟化—工具栏

> 原文：<https://blog.devgenius.io/vuetify-toolbars-d05ceea2927a?source=collection_archive---------0----------------------->

![](img/347a9b622d2ecde8ebfb38982fc59c2b.png)

照片由[JESHOOTS.COM](https://unsplash.com/@jeshoots?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 工具栏

我们可以用`v-toolbar`组件添加工具栏。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row class="text-center">
      <v-col col="12">
        <v-card color="grey lighten-4" flat height="200px" tile>
          <v-toolbar prominent extended>
            <v-app-bar-nav-icon></v-app-bar-nav-icon> <v-toolbar-title>Title</v-toolbar-title> <v-spacer></v-spacer> <v-btn icon>
              <v-icon>mdi-magnify</v-icon>
            </v-btn> <v-btn icon>
              <v-icon>mdi-heart</v-icon>
            </v-btn> <v-btn icon>
              <v-icon>mdi-dots-vertical</v-icon>
            </v-btn>
          </v-toolbar>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({}),
};
</script>
```

用`v-toolbar`组件添加一个工具栏。

`v-app-bar-nav-icon`添加导航图标。

`v-toolbar-title`让我们添加标题。

`v-space`让我们给工具栏项目添加间距，

`v-btn`让我们添加按钮。

# 密集工具栏

我们可以给`v-toolbar`加上`dense`道具，让它更密集。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row class="text-center">
      <v-col col="12">
        <v-card color="grey lighten-4" flat height="200px" tile>
          <v-toolbar dense>
            <v-app-bar-nav-icon></v-app-bar-nav-icon> <v-toolbar-title>Title</v-toolbar-title> <v-spacer></v-spacer> <v-btn icon>
              <v-icon>mdi-magnify</v-icon>
            </v-btn> <v-btn icon>
              <v-icon>mdi-heart</v-icon>
            </v-btn> <v-btn icon>
              <v-icon>mdi-dots-vertical</v-icon>
            </v-btn>
          </v-toolbar>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({}),
};
</script>
```

使工具栏变短。

# 黑暗变体

为了使工具栏显示为黑色背景，我们可以添加`dark`道具；

```
<template>
  <v-container>
    <v-row class="text-center">
      <v-col col="12">
        <v-card color="grey lighten-4" flat height="200px" tile>
          <v-toolbar dark>
            <v-spacer></v-spacer> <v-btn icon>
              <v-icon>mdi-reply</v-icon>
            </v-btn> <v-btn icon>
              <v-icon>mdi-dots-vertical</v-icon>
            </v-btn>
          </v-toolbar>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({}),
};
</script>
```

# 背景颜色

我们也可以改变工具栏的背景颜色。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row class="text-center">
      <v-col col="12">
        <v-card color="grey lighten-4" flat height="200px" tile>
          <v-toolbar color="primary" dark>
            <v-spacer></v-spacer> <v-btn icon>
              <v-icon>mdi-reply</v-icon>
            </v-btn> <v-btn icon>
              <v-icon>mdi-dots-vertical</v-icon>
            </v-btn>
          </v-toolbar>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({}),
};
</script>
```

我们将`color`道具设置为`primary`使其变成蓝色。

我们添加了`dark`道具来使图标变成白色。

# 带背景的突出工具栏

我们可以添加一个带有背景图片的工具栏。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row class="text-center">
      <v-col col="12">
        <v-toolbar dark prominent src="https://cdn.vuetifyjs.com/images/backgrounds/vbanner.jpg">
          <v-app-bar-nav-icon></v-app-bar-nav-icon> <v-toolbar-title>App</v-toolbar-title> <v-spacer></v-spacer> <v-btn icon>
            <v-icon>mdi-export</v-icon>
          </v-btn>
        </v-toolbar>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({}),
};
</script>
```

我们有带有`dark`和`prominent`道具的`v-toolbar`组件来改变背景颜色。

`src`设置图像的 URL。

![](img/4fd84f53b12a63f34888da07b78770f6.png)

照片由[路易斯·汉瑟@shotsoflouis](https://unsplash.com/@louishansel?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以添加各种颜色、大小和背景图像的工具栏。