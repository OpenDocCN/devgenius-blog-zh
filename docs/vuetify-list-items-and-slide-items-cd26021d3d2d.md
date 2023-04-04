# 列表项目和幻灯片项目

> 原文：<https://blog.devgenius.io/vuetify-list-items-and-slide-items-cd26021d3d2d?source=collection_archive---------5----------------------->

![](img/d33bad0b01881ecb02a2bf6cb768d49c.png)

照片由 [Chimene Gaspar](https://unsplash.com/@chigraph?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 强制列表项目

我们可以添加`mandatory`道具来强制选择一个项目:

```
<template>
  <v-container class="grey lighten-5">
    <v-row>
      <v-col>
        <v-list flat>
          <v-list-item-group v-model="model" color="indigo" active-class="border">
            <v-list-item v-for="(item, i) in items" :key="i">
              <v-list-item-icon>
                <v-icon v-text="item.icon"></v-icon>
              </v-list-item-icon><v-list-item-content>
                <v-list-item-title v-text="item.text"></v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list-item-group>
        </v-list>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    items: [
      {
        icon: "mdi-wifi",
        text: "Wifi",
      },
      {
        icon: "mdi-bluetooth",
        text: "Bluetooth",
      },
      {
        icon: "mdi-chart-donut",
        text: "Data Usage",
      },
    ],
    model: undefined
  }),
};
</script>
```

# 自定义活动类

可以设置`active-class`属性来为活动项目设置自定义类。

例如，我们可以写:

```
<template>
  <v-container class="grey lighten-5">
    <v-row>
      <v-col>
        <v-list flat>
          <v-list-item-group v-model="model" color="indigo" active-class="border">
            <v-list-item v-for="(item, i) in items" :key="i">
              <v-list-item-icon>
                <v-icon v-text="item.icon"></v-icon>
              </v-list-item-icon><v-list-item-content>
                <v-list-item-title v-text="item.text"></v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list-item-group>
        </v-list>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    items: [
      {
        icon: "mdi-wifi",
        text: "Wifi",
      },
      {
        icon: "mdi-bluetooth",
        text: "Bluetooth",
      },
      {
        icon: "mdi-chart-donut",
        text: "Data Usage",
      },
    ],
    model: undefined
  }),
};
</script><style scoped>
.border {
  border: 1px solid red;
}
</style>
```

我们刚刚添加了`border`类，看到一个红色的轮廓。

# 幻灯片组

`v-slide-group`组件用于显示分页信息。

例如，我们可以写:

```
<template>
  <v-container class="grey lighten-5">
    <v-row>
      <v-col>
        <v-sheet class="mx-auto" elevation="8" max-width="800">
          <v-slide-group
            v-model="model"
            class="pa-4"
            prev-icon="mdi-minus"
            next-icon="mdi-plus"
            show-arrows
          >
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

添加`v-slide-group`组件，组件内有`v-slide-item`组件。

我们使用`active`布尔值来检查项目是否被选中。

而`toggle`功能让我们切换`active`状态。

# 结论

我们可以用列表项目组和幻灯片项目组对项目进行分组。