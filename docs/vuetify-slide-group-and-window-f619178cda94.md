# 虚拟化—幻灯片组和窗口

> 原文：<https://blog.devgenius.io/vuetify-slide-group-and-window-f619178cda94?source=collection_archive---------2----------------------->

![](img/c3ab48091b18347f0e15c1b3f03f433f.png)

由 [Rob Wingate](https://unsplash.com/@robwingate?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 幻灯片组居中活动项目

我们可以用`center-active`道具将选中的项目放在滑动组的中心:

```
<template>
  <v-container class="grey lighten-5">
    <v-row>
      <v-col>
        <v-sheet class="mx-auto" elevation="8" max-width="800">
          <v-slide-group v-model="model" class="pa-4" center-active show-arrows>
            <v-slide-item v-for="n in 15" :key="n" v-slot:default="{ active, toggle }">
              <v-card
                :color="active ? 'primary' : 'grey lighten-1'"
                class="ma-4"
                height="200"
                width="100"
                @click="toggle"
              >
                <v-row class="fill-height" align="center" justify="center">
                  <v-scale-transition>
                    <v-icon
                      v-if="active"
                      color="white"
                      size="48"
                      v-text="'mdi-close-circle-outline'"
                    ></v-icon>
                  </v-scale-transition>
                </v-row>
              </v-card>
            </v-slide-item>
          </v-slide-group>
        </v-sheet>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    model: undefined,
  }),
};
</script>
```

现在，当我们点击一个项目时，它将总是居中。

# Windows 操作系统

组件让我们将内容从一个窗格转移到另一个窗格。

例如，我们可以写:

```
<template>
  <v-container class="grey lighten-5">
    <v-row>
      <v-col>
        <v-card flat tile>
          <v-window v-model="onboarding" vertical>
            <v-window-item v-for="n in length" :key="`card-${n}`">
              <v-card color="grey" height="200">
                <v-row class="fill-height" align="center" justify="center" tag="v-card-text">
                  <h1 style="font-size: 5rem;" class="white--text">Slide {{ n }}</h1>
                </v-row>
              </v-card>
            </v-window-item>
          </v-window> <v-card-actions class="justify-space-between">
            <v-btn text @click="prev">
              <v-icon>mdi-chevron-left</v-icon>
            </v-btn>
            <v-item-group v-model="onboarding" class="text-center" mandatory>
              <v-item v-for="n in length" :key="`btn-${n}`" v-slot:default="{ active, toggle }">
                <v-btn :input-value="active" icon @click="toggle">
                  <v-icon>mdi-record</v-icon>
                </v-btn>
              </v-item>
            </v-item-group>
            <v-btn text @click="next">
              <v-icon>mdi-chevron-right</v-icon>
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    length: 6,
    onboarding: 0,
  }), methods: {
    next() {
      this.onboarding =
        this.onboarding + 1 === this.length ? 0 : this.onboarding + 1;
    },
    prev() {
      this.onboarding =
        this.onboarding - 1 < 0 ? this.length - 1 : this.onboarding - 1;
    },
  },
};
</script>
```

我们有带有`v-model`的`v-window`组件来获取和设置我们所在幻灯片的索引。

`v-card`是我们滚动浏览的内容。

`v-card-actions`组件具有用于在幻灯片之间移动的导航控件。

`vertical`道具使幻灯片垂直显示。

通过改变`onboarding`值，使用`next`和`prev`方法移动载玻片，该值绑定到`v-model`以改变载玻片。

# 反面的

我们可以用`reverse`组件反转`v-window`组件:

```
<template>
  <v-container class="grey lighten-5">
    <v-row>
      <v-col>
        <v-card flat tile>
          <v-window v-model="onboarding" vertical reverse>
            <v-window-item v-for="n in length" :key="`card-${n}`">
              <v-card color="grey" height="200">
                <v-row class="fill-height" align="center" justify="center" tag="v-card-text">
                  <h1 style="font-size: 5rem;" class="white--text">Slide {{ n }}</h1>
                </v-row>
              </v-card>
            </v-window-item>
          </v-window> <v-card-actions class="justify-space-between">
            <v-btn text @click="prev">
              <v-icon>mdi-chevron-left</v-icon>
            </v-btn>
            <v-item-group v-model="onboarding" class="text-center" mandatory>
              <v-item v-for="n in length" :key="`btn-${n}`" v-slot:default="{ active, toggle }">
                <v-btn :input-value="active" icon @click="toggle">
                  <v-icon>mdi-record</v-icon>
                </v-btn>
              </v-item>
            </v-item-group>
            <v-btn text @click="next">
              <v-icon>mdi-chevron-right</v-icon>
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    length: 6,
    onboarding: 0,
  }), methods: {
    next() {
      this.onboarding =
        this.onboarding + 1 === this.length ? 0 : this.onboarding + 1;
    },
    prev() {
      this.onboarding =
        this.onboarding - 1 < 0 ? this.length - 1 : this.onboarding - 1;
    },
  },
};
</script>
```

现在，当我们单击导航控件时，幻灯片会反向移动。