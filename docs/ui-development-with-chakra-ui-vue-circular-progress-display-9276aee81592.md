# 使用 Chakra UI Vue 的 UI 开发—循环进度显示

> 原文：<https://blog.devgenius.io/ui-development-with-chakra-ui-vue-circular-progress-display-9276aee81592?source=collection_archive---------8----------------------->

![](img/7f5f9241ade9c364047e861933f18747.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Erlend Ekseth](https://unsplash.com/@er1end?utm_source=medium&utm_medium=referral) 拍摄

Chakra UI Vue 是一个为 Vue.js 制作的 UI 框架，让我们可以将好看的 UI 组件添加到我们的 Vue 应用程序中。

本文将介绍如何开始使用 Chakra UI Vue 进行 UI 开发。

# 循环进度显示

查克拉 UI Vue 带有一个循环进度组件。

要添加它，我们可以写:

```
<template>
  <c-box>
    <c-circular-progress :value="80" />
  </c-box>
</template><script>
import { CBox, CCircularProgress } from "@chakra-ui/vue";
export default {
  components: {
    CBox,
    CCircularProgress,
  },
};
</script>
```

添加`c-circular-progress`组件以添加循环进度显示。

`value`设置为进度值，可以在 0 到 100 之间。

我们可以用`size`道具改变大小:

```
<template>
  <c-box>
    <c-circular-progress :value="80" size="120px" />
  </c-box>
</template><script>
import { CBox, CCircularProgress } from "@chakra-ui/vue";
export default {
  components: {
    CBox,
    CCircularProgress,
  },
};
</script>
```

我们可以用`color`道具改变它的颜色:

```
<template>
  <c-box>
    <c-circular-progress :value="80" color="orange" size="120px" />
  </c-box>
</template><script>
import { CBox, CCircularProgress } from "@chakra-ui/vue";
export default {
  components: {
    CBox,
    CCircularProgress,
  },
};
</script>
```

同样，我们可以添加`c-circular-progress-label`组件来在循环进度组件中添加一个标签:

```
<template>
  <c-box>
    <c-circular-progress :value="40" color="green">
      <c-circular-progress-label>40%</c-circular-progress-label>
    </c-circular-progress>
  </c-box>
</template><script>
import {
  CBox,
  CCircularProgress,
  CCircularProgressLabel,
} from "@chakra-ui/vue";
export default {
  components: {
    CBox,
    CCircularProgress,
    CCircularProgressLabel,
  },
};
</script>
```

要制作动画，我们可以使用`is-determinate`道具:

```
<template>
  <c-box>
    <c-circular-progress is-indeterminate />
  </c-box>
</template><script>
import { CBox, CCircularProgress } from "@chakra-ui/vue";
export default {
  components: {
    CBox,
    CCircularProgress,
  },
};
</script>
```

# 结论

我们可以使用 Chakar UI Vue 在我们的 Vue 应用程序中添加一个循环进度显示。