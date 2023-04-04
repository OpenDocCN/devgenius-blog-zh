# 虚拟化—覆盖

> 原文：<https://blog.devgenius.io/vuetify-overlays-e9102b63dacc?source=collection_archive---------5----------------------->

![](img/78809be60f6dea522adfd2b1892e2241.png)

Sergo Karakozov 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 遮掩

我们可以用`v-overlay`组件添加覆盖图。

它用来强调特定的元素或部分。

它对发出状态变化的信号很有用。

我们可以这样写:

```
<template>
  <v-row align="center" justify="center">
    <v-card height="300" width="250">
      <v-row justify="center">
        <v-btn color="success" class="mt-12" @click="overlay = !overlay">Show Overlay</v-btn> <v-overlay :absolute="absolute" :value="overlay">
          <v-btn color="success" @click="overlay = false">Hide Overlay</v-btn>
        </v-overlay>
      </v-row>
    </v-card>
  </v-row>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    absolute: true,
    overlay: false,
  }),
};
</script>
```

我们有了`v-btn`组件来添加一个按钮来显示一个覆盖图。

我们有通过点击按钮触发的`v-overlay`组件。

`absolute`支柱使其定位在绝对位置。

`value`让我们设置覆盖。

# 不透明

我们可以改变`v-overlay`组件的不透明度。

例如，我们可以写:

```
<template>
  <v-row align="center" justify="center">
    <v-card height="300" width="250">
      <v-row justify="center">
        <v-btn color="orange lighten-2" class="mt-12" @click="overlay = !overlay">Show Overlay</v-btn> <v-overlay :absolute="absolute" :opacity="opacity" :value="overlay">
          <v-btn color="orange lighten-2" @click="overlay = false">Hide Overlay</v-btn>
        </v-overlay>
      </v-row>
    </v-card>
  </v-row>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    absolute: true,
    opacity: 1,
    overlay: false,
  }),
};
</script>
```

`opacity`道具让我们改变`v-overlay`的不透明度。

# z 指数

覆盖的 z 轴可以用`z-index`道具改变。

例如，我们可以写:

```
<template>
  <v-row justify="center">
    <v-btn class="white--text" color="teal" @click="overlay = !overlay">Show Overlay</v-btn> <v-overlay :z-index="zIndex" :value="overlay">
      <v-btn class="white--text" color="teal" @click="overlay = false">Hide Overlay</v-btn>
    </v-overlay>
  </v-row>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    overlay: false,
    zIndex: 0,
  }),
};
</script>
```

我们将`z-index`属性更改为我们想要放置在其他元素之上的值。

# 装货设备

覆盖图的顶部可以放置一个加载器图标。

例如，我们可以写:

```
<template>
  <div class="text-center">
    <v-btn color="deep-purple accent-4" class="white--text" @click="overlay = !overlay">
      Launch Application
      <v-icon right>mdi-open-in-new</v-icon>
    </v-btn> <v-overlay :value="overlay">
      <v-progress-circular indeterminate size="64"></v-progress-circular>
    </v-overlay>
  </div>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    overlay: false,
  }), watch: {
    overlay(val) {
      val &&
        setTimeout(() => {
          this.overlay = false;
        }, 3000);
    },
  },
};
</script>
```

`v-overlay`组件有`v-progress-circular`组件显示循环进度显示。

`overlay`观察者在 3 秒后将`this.overlay`设置为`false`。

# 高级叠加

我们可以有覆盖在卡片上的覆盖物。

例如，我们可以写:

```
<template>
  <v-hover>
    <template v-slot:default="{ hover }">
      <v-card class="mx-auto" max-width="344">
        <v-img src="https://cdn.vuetifyjs.com/images/cards/forest-art.jpg"></v-img> <v-card-text>
          <h2 class="title primary--text">Forests</h2>
          <p>Lorem ipsum</p>
        </v-card-text> <v-card-title>
          <v-rating :value="4" dense color="orange" background-color="orange" hover class="mr-2"></v-rating>
          <span class="primary--text subtitle-2">100 Reviews</span>
        </v-card-title> <v-fade-transition>
          <v-overlay v-if="hover" absolute color="#036358">
            <v-btn>See more info</v-btn>
          </v-overlay>
        </v-fade-transition>
      </v-card>
    </template>
  </v-hover>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    overlay: false,
  }),
};
</script>
```

当我们显示覆盖图时，`v-fade-transition`增加了过渡效果。

`v-overlay`在`v-fade-transition`组件内。

# 结论

我们可以用 Vuetify 的`v-overlay`组件添加覆盖图。