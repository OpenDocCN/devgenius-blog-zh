# 使用 Chakra UI Vue 的 UI 开发—工具提示

> 原文：<https://blog.devgenius.io/ui-development-with-chakra-ui-vue-tooltips-fc79c4cf153a?source=collection_archive---------6----------------------->

![](img/2018a75d959029e1935b95f9170d62db.png)

照片由[朱利安·霍格桑](https://unsplash.com/@julianhochgesang?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Chakra UI Vue 是一个为 Vue.js 制作的 UI 框架，让我们可以将好看的 UI 组件添加到我们的 Vue 应用程序中。

本文将介绍如何开始使用 Chakra UI Vue 进行 UI 开发。

# 工具提示

我们可以用查克拉 UI Vue 添加一个工具提示。

例如，我们可以写:

```
<template>
  <c-box> <c-tooltip label="tooltip">Hover Me</c-tooltip> </c-box>
</template><script>
import { CBox, CTooltip } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CTooltip,
  },
};
</script>
```

我们添加了`c-tooltip`组件来添加工具提示。

`label`有工具提示文本。

当我们悬停在“悬停我”上时，我们会看到显示的工具提示。

我们可以添加一个图标作为悬停内容。

例如，我们可以写:

```
<template>
  <c-box>
    <c-tooltip label="Welcome home" placement="bottom">
      <c-icon name="phone" />
    </c-tooltip>
  </c-box>
</template><script>
import { CBox, CTooltip, CIcon } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CTooltip,
    CIcon,
  },
};
</script>
```

添加图标。

`placement`设置工具提示的位置。

我们可以用`bg`道具改变工具提示的背景颜色:

```
<template>
  <c-box>
    <c-tooltip label="Welcome home" placement="bottom" bg="blue.600">
      <c-icon name="phone" />
    </c-tooltip>
  </c-box>
</template><script>
import { CBox, CTooltip, CIcon } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CTooltip,
    CIcon,
  },
};
</script>
```

我们可以用`has-arrow`道具给工具提示添加一个箭头:

```
<template>
  <c-box>
    <c-tooltip label="Welcome home" placement="bottom" has-arrow>
      <c-icon name="phone" />
    </c-tooltip>
  </c-box>
</template><script>
import { CBox, CTooltip, CIcon } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CTooltip,
    CIcon,
  },
};
</script>
```

# 结论

我们可以用 Chakra UI Vue 轻松添加工具提示。