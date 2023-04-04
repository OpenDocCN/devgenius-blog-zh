# 使用 Chakra UI Vue 的 UI 开发—统计显示

> 原文：<https://blog.devgenius.io/ui-development-with-chakra-ui-vue-statistics-display-39a660c67f4b?source=collection_archive---------5----------------------->

![](img/ff843b773642c327e4fe0aa8bcbf1a40.png)

斯蒂芬·道森在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Chakra UI Vue 是一个为 Vue.js 制作的 UI 框架，让我们可以将好看的 UI 组件添加到我们的 Vue 应用程序中。

本文将介绍如何开始使用 Chakra UI Vue 进行 UI 开发。

# 统计组件

Chakra UI Vue 带有一个显示统计数据的组件。

为了使用它，我们写:

```
<template>
  <c-box>
    <c-stat>
      <c-stat-label>Collected Fees</c-stat-label>
      <c-stat-number>$500.00</c-stat-number>
      <c-stat-helper-text>Jan 12 - Jan 28</c-stat-helper-text>
    </c-stat>
  </c-box>
</template><script>
import { CBox, CStat, CStatLabel, CStatNumber } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CStat,
    CStatLabel,
    CStatNumber,
  },
};
</script>
```

我们添加了`c-stat`组件来添加统计容器。

`c-stat-label`是统计数据的标签。

`c-stat-number`是统计数字的容器。

`c-stat-helper-text`是助手文本的容器。

我们可以添加一组带有`c-stat-group`成分的统计数据:

```
<template>
  <c-box>
    <c-stat-group>
      <c-stat>
        <c-stat-label>Sent</c-stat-label>
        <c-stat-number>676585</c-stat-number>
        <c-stat-helper-text>
          <c-stat-arrow type="increase" />
          30.60%
        </c-stat-helper-text>
      </c-stat>
      <c-stat>
        <c-stat-label>Clicked</c-stat-label>
        <c-stat-number>45</c-stat-number>
        <c-stat-helper-text>
          <c-stat-arrow type="decrease" />
          -5.20%
        </c-stat-helper-text>
      </c-stat>
    </c-stat-group>
  </c-box>
</template><script>
import {
  CBox,
  CStat,
  CStatLabel,
  CStatNumber,
  CStatHelpText,
  CStatArrow,
  CStatGroup,
} from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CStat,
    CStatLabel,
    CStatNumber,
    CStatHelpText,
    CStatArrow,
    CStatGroup,
  },
};
</script>
```

我们从多个`c-stat`组件中包装`c-stat-group`。

此外，我们添加了`c-stat-arrow`组件来添加用于增加和增加的箭头，这些箭头是由`type`道具设置的。

# 结论

我们可以用查克拉 UI Vue 进行统计显示。