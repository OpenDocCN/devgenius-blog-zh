# 天气卡和旋转木马

> 原文：<https://blog.devgenius.io/vuetify-weather-card-and-carousel-2f8fdf90b472?source=collection_archive---------3----------------------->

![](img/ee070a58f4be8a52f5ae7bc0331c2d89.png)

照片由 [lanah nel](https://unsplash.com/@laans?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 天气卡

我们可以用 Vuetify 添加天气卡。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-card class="mx-auto" max-width="400">
          <v-list-item two-line>
            <v-list-item-content>
              <v-list-item-title class="headline">London</v-list-item-title>
              <v-list-item-subtitle>Mon, 12:30 PM, Mostly sunny</v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item> <v-card-text>
            <v-row align="center">
              <v-col class="display-3" cols="6">23&deg;C</v-col>
              <v-col cols="6">
                <v-img
                   src="https://cdn.vuetifyjs.com/images/cards/sun.png"
                  alt="Sunny image"
                  width="92"
                ></v-img>
              </v-col>
            </v-row>
          </v-card-text> <v-list-item>
            <v-list-item-icon>
              <v-icon>mdi-send</v-icon>
            </v-list-item-icon>
            <v-list-item-subtitle>23 km/h</v-list-item-subtitle>
          </v-list-item> <v-list-item>
            <v-list-item-icon>
              <v-icon>mdi-cloud-download</v-icon>
            </v-list-item-icon>
            <v-list-item-subtitle>48%</v-list-item-subtitle>
          </v-list-item> <v-slider v-model="time" :max="6" :tick-labels="labels" class="mx-4" ticks></v-slider> <v-list class="transparent">
            <v-list-item v-for="item in forecast" :key="item.day">
              <v-list-item-title>{{ item.day }}</v-list-item-title> <v-list-item-icon>
                <v-icon>{{ item.icon }}</v-icon>
              </v-list-item-icon> <v-list-item-subtitle class="text-right">{{ item.temp }}</v-list-item-subtitle>
            </v-list-item>
          </v-list> <v-divider></v-divider> <v-card-actions>
            <v-btn text>Full Report</v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    labels: ["SU", "MO", "TU", "WED", "TH", "FR", "SA"],
    time: 0,
    forecast: [
      {
        day: "Tuesday",
        icon: "mdi-white-balance-sunny",
        temp: "24\xB0/12\xB0",
      },
      {
        day: "Wednesday",
        icon: "mdi-white-balance-sunny",
        temp: "22\xB0/14\xB0",
      },
      { day: "Thursday", icon: "mdi-cloud", temp: "25\xB0/15\xB0" },
    ],
  }),
};
</script>
```

我们在文本的`v-list-item`后面添加一个`v-card`。

`v-card-text`有天气的度数和图像。

在那下面，我们有云和风速的文本。

我们有每天的滑动条。

和一个预测列表。

预测是从`forecast`对象呈现的。

在底部，我们有动作的`v-btn`。

# 旋转木马

`v-carousel`组件用于在旋转计时器上显示大量可视内容。

我们可以写一个简单的例子:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-carousel cycle height="400" hide-delimiter-background show-arrows-on-hover>
          <v-carousel-item v-for="(slide, i) in slides" :key="i">
            <v-sheet :color="colors[i]" height="100%">
              <v-row class="fill-height" align="center" justify="center">
                <div class="display-3">Slide {{ slide }}</div>
              </v-row>
            </v-sheet>
          </v-carousel-item>
        </v-carousel>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    colors: [
      "indigo",
      "warning",
      "pink darken-2",
      "red lighten-1",
      "deep-purple accent-4",
    ],
    slides: [1, 2, 3, 4, 5],
  }),
};
</script>
```

我们使用内部带有`v-carousel-item` s 的`v-carousel`组件。

幻灯片的背景颜色用`color`道具设置。

![](img/cdc11800d16ce47f132dc6593bb5656e.png)

埃里克·汤普金斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

我们可以用 Vuetify 添加一个旋转木马和天气卡。