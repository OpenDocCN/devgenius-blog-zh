# 使用 Chakra UI Vue 开发 UI——简单网格

> 原文：<https://blog.devgenius.io/ui-development-with-chakra-ui-vue-simple-grid-9306dab68732?source=collection_archive---------5----------------------->

![](img/35644d4aebefbff6e0fe8b48347aee9a.png)

安德鲁·舒尔茨在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Chakra UI Vue 是一个为 Vue.js 制作的 UI 框架，让我们可以将好看的 UI 组件添加到我们的 Vue 应用程序中。

本文将介绍如何开始使用 Chakra UI Vue 进行 UI 开发。

# 简单网格

我们可以使用`c-simple-grid`组件轻松添加响应性布局。

例如，我们可以写:

```
<template>
  <c-box>
    <c-simple-grid :columns="2" :spacing="10">
      <c-box background="green" height="80px"></c-box>
      <c-box background="green" height="80px"></c-box>
      <c-box background="green" height="80px"></c-box>
      <c-box background="green" height="80px"></c-box>
      <c-box background="green" height="80px"></c-box>
    </c-simple-grid>
  </c-box>
</template><script>
import { CBox, CSimpleGrid } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CSimpleGrid,
  },
};
</script>
```

添加两列网格，每个网格项目之间的间距为 10px。

我们使用`c-box`组件来添加网格项。

我们将`height`设置为 80px。

`background`设置每个网格项目的背景颜色。

我们可以通过将`columns`属性设置为一个数组，将最小屏幕宽度的列数设置为最大，从而使网格布局具有响应性。

例如，我们可以写:

```
<template>
  <c-box>
    <c-simple-grid :columns="[2, null, 3]" :spacing="10">
      <c-box background="green" height="80px"></c-box>
      <c-box background="green" height="80px"></c-box>
      <c-box background="green" height="80px"></c-box>
      <c-box background="green" height="80px"></c-box>
      <c-box background="green" height="80px"></c-box>
    </c-simple-grid>
  </c-box>
</template><script>
import { CBox, CSimpleGrid } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CSimpleGrid,
  },
};
</script>
```

那么对于窄屏幕，每行显示 2 列，对于宽屏幕，每行显示 3 列。

此外，我们可以设置`min-child-width`属性来设置每个网格项目的最小宽度。

例如，我们可以写:

```
<template>
  <c-box>
    <c-simple-grid min-child-width="120px" :spacing="10">
      <c-box background="green" height="80px"></c-box>
      <c-box background="green" height="80px"></c-box>
      <c-box background="green" height="80px"></c-box>
      <c-box background="green" height="80px"></c-box>
      <c-box background="green" height="80px"></c-box>
    </c-simple-grid>
  </c-box>
</template><script>
import { CBox, CSimpleGrid } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CSimpleGrid,
  },
};
</script>
```

将每个网格项目的最小宽度设置为 120 像素。

此外，我们可以用`spacing-x`支柱调整水平间距，用`spacing-y`支柱改变垂直间距:

```
<template>
  <c-box>
    <c-simple-grid min-child-width="120px" spacing-x="40px" spacing-y="20px">
      <c-box background="green" height="80px"></c-box>
      <c-box background="green" height="80px"></c-box>
      <c-box background="green" height="80px"></c-box>
      <c-box background="green" height="80px"></c-box>
      <c-box background="green" height="80px"></c-box>
    </c-simple-grid>
  </c-box>
</template><script>
import { CBox, CSimpleGrid } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CSimpleGrid,
  },
};
</script>
```

# 结论

我们可以使用 Chakra UI Vue 轻松添加网格布局。