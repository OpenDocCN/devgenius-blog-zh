# 使用 Swiper 6 将 Swiper Carousel 添加到 Vue 3 应用程序中

> 原文：<https://blog.devgenius.io/add-a-swiper-carousel-into-a-vue-3-app-with-swiper-6-1dcef0bfa1cc?source=collection_archive---------3----------------------->

![](img/c073ddffcbd8e6e911697f725892e3b4.png)

[楚瀚 Z.](https://unsplash.com/@chuhan?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Swiper for Vue.js 允许我们在 Vue 3 应用程序中添加一个旋转木马。

在本文中，我们将看看如何使用 Swiper 在我们的 Vue 3 应用程序中添加一个旋转木马。

# 装置

我们可以通过运行以下命令来安装 Swiper 包:

```
npm i swiper
```

# 基本用法

一旦我们安装了这个包，我们可以通过编写以下代码来添加一个滑块:

```
<template>
  <swiper
    :slides-per-view="3"
    :space-between="50"
    @swiper="onSwiper"
    @slideChange="onSlideChange"
  >
    <swiper-slide v-for="n of 50" :key="n">Slide {{ n }}</swiper-slide>
  </swiper>
</template>
<script>
import { Swiper, SwiperSlide } from "swiper/vue";
import 'swiper/swiper-bundle.css'export default {
  components: {
    Swiper,
    SwiperSlide,
  },
  methods: {
    onSwiper(swiper) {
      console.log(swiper);
    },
    onSlideChange() {
      console.log("slide change");
    },
  },
};
</script>
```

我们添加了`swiper`组件来添加滑块。

`slides-per-view`让我们设置每个视图显示的幻灯片数量。

`space-between`以像素为单位显示幻灯片之间的间距。

它发出在创建滑块时发出的`swiper`事件。

`slideChange`当我们更换幻灯片时，事件被激发。

我们导入`swiper/swiper-bundle.css`文件来为幻灯片添加样式。

`swiper-slide`有滑块的滑块。

如果设置为`true`，我们可以添加`zoom`属性来启用缩放模式的附加包装器。

`virtualIndex`有虚拟幻灯片的索引。

# `SwiperSlide`老虎机道具

我们可以从`swiper-slide`的组件的 slot props 中得到一些数据。

例如，我们使用`isActive`道具来查看幻灯片是否处于活动状态:

```
<template>
  <swiper
    :slides-per-view="3"
    :space-between="50"
    @swiper="onSwiper"
    @slideChange="onSlideChange"
  >
    <swiper-slide v-slot="{ isActive }" v-for="n of 50" :key="n"
      >Slide {{ n }} {{ isActive ? "active" : "not active" }}</swiper-slide
    >
  </swiper>
</template>
<script>
import { Swiper, SwiperSlide } from "swiper/vue";
import "swiper/swiper-bundle.css";export default {
  components: {
    Swiper,
    SwiperSlide,
  },
  methods: {
    onSwiper(swiper) {
      console.log(swiper);
    },
    onSlideChange() {
      console.log("slide change");
    },
  },
};
</script>
```

它还提供了`isPrev` slot prop 来查看幻灯片是否是活动幻灯片的前一张幻灯片。

`isNext`槽道具，查看幻灯片是否是活动幻灯片旁边的幻灯片。

`isVisible`表示当前幻灯片是否可见。

`isDuplicate`表示该幻灯片是否为重复幻灯片。

# 时间

我们可以填充由`swiper`组件提供的插槽。

例如，我们可以写:

```
<template>
  <swiper
    :slides-per-view="3"
    :space-between="50"
    @swiper="onSwiper"
    @slideChange="onSlideChange"
  >
    <swiper-slide v-for="n of 50" :key="n">Slide {{ n }}</swiper-slide>
    <template v-slot:container-start>
      <span>Container Start</span>
    </template>
    <template v-slot:container-end>
      <span>Container End</span>
    </template>
    <template v-slot:wrapper-start>
      <span>Wrapper Start</span>
    </template>
    <template v-slot:wrapper-end>
      <span>Wrapper End</span>
    </template>
  </swiper>
</template>
<script>
import { Swiper, SwiperSlide } from "swiper/vue";
import "swiper/swiper-bundle.css";export default {
  components: {
    Swiper,
    SwiperSlide,
  },
  methods: {
    onSwiper(swiper) {
      console.log(swiper);
    },
    onSlideChange() {
      console.log("slide change");
    },
  },
};
</script>
```

我们填充了`container-start`槽，将内容添加到滑块容器的顶部。

`container-end`将内容添加到滑块容器的底部

`wrapper-start`在第一张幻灯片前添加内容。

`wrapper-end`在最后一张幻灯片后添加内容。

# 结论

我们可以使用 Swiper 6 将滑块添加到我们的 Vue 3 应用程序中。