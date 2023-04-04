# 使用 Chakra UI Vue 的 UI 开发—加载微调器

> 原文：<https://blog.devgenius.io/ui-development-with-chakra-ui-vue-loading-spinner-35b10cd57239?source=collection_archive---------5----------------------->

![](img/d65dedc609740d85085bd893549ccefc.png)

[梁金生](https://unsplash.com/@filmprint?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Chakra UI Vue 是一个为 Vue.js 制作的 UI 框架，让我们可以将好看的 UI 组件添加到我们的 Vue 应用程序中。

本文将介绍如何开始使用 Chakra UI Vue 进行 UI 开发。

# 装载旋转器

我们可以用 Chakra UI Vue 在我们的 Vue 应用程序中添加一个加载微调器。

为了补充它，我们写道:

```
<template>
  <c-box>
    <c-spinner />
  </c-box>
</template><script>
import { CBox, CSpinner } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CSpinner,
  },
};
</script>
```

我们添加了`c-spinner`组件来显示微调器。

我们可以用`color`道具改变旋转器的颜色:

```
<template>
  <c-box>
    <c-spinner color="blue.500" />
  </c-box>
</template><script>
import { CBox, CSpinner } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CSpinner,
  },
};
</script>
```

我们可以用`size`道具改变它的大小:

```
<template>
  <c-box>
    <c-spinner size="lg" />
  </c-box>
</template><script>
import { CBox, CSpinner } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CSpinner,
  },
};
</script>
```

`lg`大。我们也可以将它设置为`xs`代表超小号、`sm`代表小号、`md`代表中号或`xl`代表超大号。

我们可以改变的其他选项包括厚度、旋转速度和空白区域的颜色。

为了改变它们，我们写道:

```
<template>
  <c-box>
    <c-spinner
      thickness="4px"
      speed="0.65s"
      empty-color="green.200"
      color="vue.500"
    />
  </c-box>
</template><script>
import { CBox, CSpinner } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CSpinner,
  },
};
</script>
```

`thickness`改变旋转环的厚度。

`speed`改变完成一次旋转循环所需的时间。

`empty-color`设置旋转环空白部分的颜色。

# 结论

我们可以用 Chakra UI Vue 在我们的 Vue 应用程序中添加一个加载微调器。