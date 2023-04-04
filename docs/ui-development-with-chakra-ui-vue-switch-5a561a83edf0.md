# 使用 Chakra UI Vue-Switch 进行 UI 开发

> 原文：<https://blog.devgenius.io/ui-development-with-chakra-ui-vue-switch-5a561a83edf0?source=collection_archive---------10----------------------->

![](img/b17f6b06e087f5628c64c1d8d62e13fe.png)

阿尔瓦罗·雷耶斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Chakra UI Vue 是一个为 Vue.js 制作的 UI 框架，让我们可以将好看的 UI 组件添加到我们的 Vue 应用程序中。

本文将介绍如何开始使用 Chakra UI Vue 进行 UI 开发。

# 转换

我们可以用 Chakra UI Vue 添加一个开关组件。

例如，我们可以写:

```
<template>
  <c-box>
    <c-form-control>
      <c-form-label html-for="email-alerts">Enable email alerts?</c-form-label>
      <c-switch id="email-alerts" />
    </c-form-control>
  </c-box>
</template><script>
import { CBox, CSwitch, CFormControl, CFormLabel } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CSwitch,
    CFormControl,
    CFormLabel,
  },
};
</script>
```

我们可以添加一个`c-switch`组件来添加一个开关。

`c-form-label`是交换机的标签。

`c-form-control`是容器。

我们可以用`size`道具改变它的大小:

```
<template>
  <c-box>
    <c-switch size="lg" />
  </c-box>
</template><script>
import { CBox, CSwitch } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CSwitch,
  },
};
</script>
```

`lg`使其变大。

我们也可以将其设置为`sm`使其变小，或者`md`使其变中等大小。

此外，我们可以用`color`道具改变开关打开时的颜色:

```
<template>
  <c-box>
    <c-switch color="vue" />
  </c-box>
</template><script>
import { CBox, CSwitch } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CSwitch,
  },
};
</script>
```

# 结论

我们可以添加一个带有`c-switch`组件的开关。