# 使用 Chakra UI Vue-Stack 进行 UI 开发

> 原文：<https://blog.devgenius.io/ui-development-with-chakra-ui-vue-stack-a8c9a771062c?source=collection_archive---------12----------------------->

![](img/22a6abdb319fef560389d22d0c419577.png)

照片由[王思然·哈德森](https://unsplash.com/@hudsoncrafted?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Chakra UI Vue 是一个为 Vue.js 制作的 UI 框架，让我们可以将好看的 UI 组件添加到我们的 Vue 应用程序中。

本文将介绍如何开始使用 Chakra UI Vue 进行 UI 开发。

# 堆

我们可以用`c-stack`组件在屏幕上添加一堆项目。

例如，我们可以写:

```
<template>
  <c-box>
    <c-stack :spacing="5">
      <c-box :p="5" shadow="md" border-width="1px">
        <c-heading>See the Vue</c-heading>
        <c-text :mt="4">Vue.</c-text>
      </c-box>
      <c-box :p="5" shadow="md" border-width="1px">
        <c-heading>Go Nuxt</c-heading>
        <c-text :mt="4">Nuxt.</c-text>
      </c-box>
    </c-stack>
  </c-box>
</template><script>
import { CBox, CStack, CHeading, CText } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CStack,
    CHeading,
    CText,
  },
};
</script>
```

我们从想要堆叠在一起的`c-box`元素中包装出`c-stack`。

然后在`c-box`元素中，我们添加了`c-heading`和`c-text`组件，分别为容器和文本添加组件。

此外，我们可以添加`is-line`道具来并排添加容器:

```
<template>
  <c-box>
    <c-stack :spacing="5" is-inline>
      <c-box :p="5" shadow="md" border-width="1px">
        <c-heading>See the Vue</c-heading>
        <c-text :mt="4">Vue.</c-text>
      </c-box>
      <c-box :p="5" shadow="md" border-width="1px">
        <c-heading>Go Nuxt</c-heading>
        <c-text :mt="4">Nuxt.</c-text>
      </c-box>
    </c-stack>
  </c-box>
</template><script>
import { CBox, CStack, CHeading, CText } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CStack,
    CHeading,
    CText,
  },
};
</script>
```

我们可以使用`is-reverse`道具来颠倒项目的顺序:

```
<template>
  <c-box>
    <c-stack :spacing="5" is-reversed>
      <c-box :p="5" shadow="md" border-width="1px">
        <c-heading>See the Vue</c-heading>
        <c-text :mt="4">Vue.</c-text>
      </c-box>
      <c-box :p="5" shadow="md" border-width="1px">
        <c-heading>Go Nuxt</c-heading>
        <c-text :mt="4">Nuxt.</c-text>
      </c-box>
    </c-stack>
  </c-box>
</template><script>
import { CBox, CStack, CHeading, CText } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CStack,
    CHeading,
    CText,
  },
};
</script>
```

现在首先渲染第二个`c-box`，然后在其下方渲染第一个。

我们还可以用它来堆叠 HTML 元素:

```
<template>
  <c-box>
    <c-stack>
      <c-text>Chakra component 1</c-text>
      <p>HTML paragraph element</p>
      <h3>HTML heading element</h3>
      <c-text>Chakra component 2</c-text>
    </c-stack>
  </c-box>
</template><script>
import { CBox, CStack, CText } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CStack,
    CText,
  },
};
</script>
```

# 结论

我们可以使用 Chakra UI Vue 的`c-stack`组件来添加堆叠元素。