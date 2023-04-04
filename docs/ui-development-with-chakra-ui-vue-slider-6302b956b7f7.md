# 使用 Chakra UI Vue 开发用户界面—滑块

> 原文：<https://blog.devgenius.io/ui-development-with-chakra-ui-vue-slider-6302b956b7f7?source=collection_archive---------3----------------------->

![](img/782ad9ff06b1a0f0a8913032c792eb9a.png)

[伊凡·班杜拉](https://unsplash.com/@unstable_affliction?utm_source=medium&utm_medium=referral)在[unplash](https://unsplash.com?utm_source=medium&utm_medium=referral)上的照片

Chakra UI Vue 是为 Vue.js 制作的一个 UI 框架，它允许我们在 Vue 应用程序中添加漂亮的 UI 组件。

本文将介绍如何使用 Chakra UI Vue 开始进行 UI 开发。

# 滑块

我们可以添加一个带有`c-slider`成分的滑块。

例如，我们可以写:

```
<template>
  <c-box>
    <c-slider :max="20" @change="handleChange" :value="value">
      <c-slider-track />
      <c-slider-filled-track />
      <c-slider-thumb />
    </c-slider>
  </c-box>
</template><script>
import {
  CBox,
  CSlider,
  CSliderTrack,
  CSliderThumb,
  CSliderFilledTrack,
} from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CSlider,
    CSliderTrack,
    CSliderThumb,
    CSliderFilledTrack,
  },
  data() {
    return {
      value: 15,
    };
  },
  methods: {
    handleChange(val) {
      this.value = val;
    },
  },
};
</script>
```

我们设置`value`道具来设置其值。

`max`道具设定最大允许值。

`@change`设置为更新`value`的功能。

`c-slider-track`具有滑块轨道的空部分。

`c-slider-filled-track`具有滑块轨道的填充部分。

`c-slider-thumb`有滑块按钮。

我们可以使用`color`道具更改滑块颜色:

```
<template>
  <c-box>
    <c-slider color="pink" :max="20" @change="handleChange" :value="value">
      <c-slider-track />
      <c-slider-filled-track />
      <c-slider-thumb />
    </c-slider>
  </c-box>
</template><script>
import {
  CBox,
  CSlider,
  CSliderTrack,
  CSliderThumb,
  CSliderFilledTrack,
} from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CSlider,
    CSliderTrack,
    CSliderThumb,
    CSliderFilledTrack,
  },
  data() {
    return {
      value: 15,
    };
  },
  methods: {
    handleChange(val) {
      this.value = val;
    },
  },
};
</script>
```

# 结论

我们可以添加一个滑块到我们的 Vue 应用程序与查克拉用户界面 Vue。