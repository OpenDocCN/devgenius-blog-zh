# 使用 Chakra UI Vue 的 UI 开发—网格布局和标题

> 原文：<https://blog.devgenius.io/ui-development-with-chakra-ui-vue-grid-layout-and-heading-d1c30ed32ded?source=collection_archive---------4----------------------->

![](img/ba9edee8a331fca793465c84466f5efe.png)

[吉米·奥菲西亚](https://unsplash.com/@ofisia?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Chakra UI Vue 是一个为 Vue.js 制作的 UI 框架，让我们可以将好看的 UI 组件添加到我们的 Vue 应用程序中。

本文将介绍如何开始使用 Chakra UI Vue 进行 UI 开发。

# 网格布局

我们可以用`c-grid`组件添加一个网格布局。

例如，我们可以写:

```
<template>
  <c-box>
    <c-grid w="600px" template-columns="repeat(5, 1fr)" gap="6">
      <c-box w="100%" h="10" bg="blue.300" />
      <c-box w="100%" h="10" bg="vue.300" />
      <c-box w="100%" h="10" bg="orange.300" />
      <c-box w="100%" h="10" bg="pink.300" />
      <c-box w="100%" h="10" bg="red.300" />
    </c-grid>
  </c-box>
</template><script>
import { CBox, CGrid } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CGrid,
  },
};
</script>
```

用它添加一个网格容器。

`template-columns`让我们定义网格的列。

`gap`让我们设置每个网格项目之间的间隙。

然后我们在其中添加了`c-box`来添加网格项。

`w`是宽度。`h`是身高。`bg`是背景色。

# 标题

我们可以用`c-heading`组件添加一个标题。

例如，我们可以写:

```
<template>
  <c-box>
    <c-heading>I'm a Heading</c-heading>
  </c-box>
</template><script>
import { CBox, CHeading } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CHeading,
  },
};
</script>
```

我们可以用`as`道具设置标签。

`size`道具设置标题的大小。

例如，我们可以写:

```
<template>
  <c-box>
    <c-heading as="h1" size="2xl">I'm a Heading</c-heading>
  </c-box>
</template><script>
import { CBox, CHeading } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CHeading,
  },
};
</script>
```

设置标签和尺寸。

`2xl`是最大的尺码。

其他尺寸包括`xl`、`lg`、`md`、`sm`和`xs`。

`xl`特大号。`lg`大。`md`是中等。`sm`小`xs`超小。

我们也可以用`fontSize`道具改变字体大小:

```
<template>
  <c-box>
    <c-heading as="h1" fontSize="50px">I'm a Heading</c-heading>
  </c-box>
</template><script>
import { CBox, CHeading } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CHeading,
  },
};
</script>
```

# 结论

我们可以用 Chakra UI Vue 添加网格布局和标题。