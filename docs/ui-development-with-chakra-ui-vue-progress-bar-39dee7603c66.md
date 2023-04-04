# 使用 Chakra UI Vue 的 UI 开发—进度条

> 原文：<https://blog.devgenius.io/ui-development-with-chakra-ui-vue-progress-bar-39dee7603c66?source=collection_archive---------0----------------------->

![](img/346f0980f54cb476121f2fe2003ac680.png)

照片由 [Jungwoo Hong](https://unsplash.com/@hjwinunsplsh?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Chakra UI Vue 是一个为 Vue.js 制作的 UI 框架，让我们可以将好看的 UI 组件添加到我们的 Vue 应用程序中。

本文将介绍如何开始使用 Chakra UI Vue 进行 UI 开发。

# 进度条

我们可以用`c-progress`组件在 Vue 应用程序中添加一个进度条。

例如，我们可以写:

```
<template>
  <c-box>
    <c-progress :value="80" />
  </c-box>
</template><script>
import { CBox, CProgress } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CProgress,
  },
};
</script>
```

`value`具有 0 到 100 之间的进度值。

我们可以用`size`道具改变大小:

```
<template>
  <c-box>
    <c-progress :value="80" size="lg" />
  </c-box>
</template><script>
import { CBox, CProgress } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CProgress,
  },
};
</script>
```

`lg`使其变大。

我们也可以将它设置为`sm`代表小尺寸，或者`md`代表大尺寸。

另外，我们可以用`height`道具设置它的高度:

```
<template>
  <c-box>
    <c-progress :value="80" height="32px" />
  </c-box>
</template><script>
import { CBox, CProgress } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CProgress,
  },
};
</script>
```

我们可以用`is-animated`道具制作动画:

```
<template>
  <c-box>
    <c-progress :value="80" has-stripe is-animated />
  </c-box>
</template><script>
import { CBox, CProgress } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CProgress,
  },
};
</script>
```

`has-stripe`在进度条上添加条纹。

# 结论

我们可以使用 Chakra UI Vue 在 Vue 应用程序中添加进度条。