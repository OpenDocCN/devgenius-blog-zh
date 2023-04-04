# 使用 Swiper 6 将 Swiper Carousel 添加到 Vue 3 应用程序中

> 原文：<https://blog.devgenius.io/add-a-swiper-carousel-into-a-vue-3-app-with-swiper-6-9b059035712?source=collection_archive---------1----------------------->

![](img/61c60095aa5917489d3ee18ec7e151a1.png)

照片由 [Joia de Jong](https://unsplash.com/@joyground?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Swiper for Vue.js 允许我们在 Vue 3 应用程序中添加一个旋转木马。

在本文中，我们将看看如何使用 Swiper 在我们的 Vue 3 应用程序中添加一个旋转木马。

# 缩略图浏览

我们可以用 Swiper 6 在我们的 Vue 3 应用程序中添加一个缩略图。

例如，我们可以写:

```
<template>
  <swiper :thumbs="{ swiper: thumbsSwiper }">
    <swiper-slide v-for="n of 50" :key="n"
      >Slide {{ n }}</swiper-slide
    >
  </swiper> <swiper
    @swiper="setThumbsSwiper"
    watch-slides-visibility
    watch-slides-progress
  >
    <swiper-slide v-for="n of 50" :virtualIndex="n" :key="n"
      >Thumbnail {{ n }}</swiper-slide
    >
  </swiper>
</template><script>
import SwiperCore, { Thumbs } from "swiper";
import { Swiper, SwiperSlide } from "swiper/vue";
import "swiper/swiper-bundle.css";SwiperCore.use([Thumbs]);export default {
  components: {
    Swiper,
    SwiperSlide,
  },
  data() {
    return {
      thumbsSwiper: null,
    };
  },
  methods: {
    setThumbsSwiper(swiper) {
      this.thumbsSwiper = swiper;
    },
  },
};
</script>
```

使第二次滑动成为缩略图滑动。

我们添加`watch-slides-visibility`和`watch-slides-progress`道具来观察第一个滑块的进度。

第一个 swiper 被监视是因为调用了`setThumbsSwiper`方法。

我们将`this.thumbSwiper`反应属性设置为第二个 swiper。

# 效果

Swiper 6 具有以下效果:

*   乏味的
*   立方
*   泛滥
*   翻转

我们可以写:

```
<template>
  <swiper effect="fade">
    <swiper-slide v-for="i of images" :key="i">
      <img :src="i" />
    </swiper-slide>
  </swiper>
</template><script>
import SwiperCore, { EffectFade } from "swiper";
import { Swiper, SwiperSlide } from "swiper/vue";
import "swiper/swiper.scss";
import "swiper/components/effect-fade/effect-fade.scss";SwiperCore.use([EffectFade]);export default {
  components: {
    Swiper,
    SwiperSlide,
  },
  data() {
    return {
      images: [
        "https://i.picsum.photos/id/23/200/300.jpg?hmac=NFze_vylqSEkX21kuRKSe8pp6Em-4ETfOE-oyLVCvJo",
        "https://i.picsum.photos/id/25/200/300.jpg?hmac=ScdLbPfGd_kI3MUHvJUb12Fsg1meDQEaHY_mM613BVM",
        "https://i.picsum.photos/id/26/200/300.jpg?hmac=E9i_aIqa_ifLvxqI2b1QTLCnhGQYJ83IpvaDfFM54bU",
      ],
    };
  },
};
</script>
```

将`effect`道具添加到`swiper`组件中。

我们还必须添加效果动画的 SCSS 文件和添加淡入淡出效果的`SwiperCore.use([EffectFade])`。

我们可以用类似的方法添加其他的效果，除了我们替换了 SCSS 和我们称之为`use`的模块。

# 结论

我们可以用 swiper 6 在我们的 Vue 3 应用程序中添加缩略图和 Swiper 效果。