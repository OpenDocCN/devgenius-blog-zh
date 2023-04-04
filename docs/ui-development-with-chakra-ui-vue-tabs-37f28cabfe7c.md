# 使用 Chakra UI Vue 的 UI 开发—选项卡

> 原文：<https://blog.devgenius.io/ui-development-with-chakra-ui-vue-tabs-37f28cabfe7c?source=collection_archive---------11----------------------->

![](img/acf511f580d519fe74250c307af93221.png)

照片由[鲁斯沃德](https://unsplash.com/@rssemfam?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Chakra UI Vue 是一个为 Vue.js 制作的 UI 框架，让我们可以将好看的 UI 组件添加到我们的 Vue 应用程序中。

本文将介绍如何开始使用 Chakra UI Vue 进行 UI 开发。

# 制表符

我们可以用查克拉 UI Vue 添加标签。

为了添加它们，我们写:

```
<template>
  <c-box>
    <c-tabs>
      <c-tab-list>
        <c-tab>One</c-tab>
        <c-tab>Two</c-tab>
        <c-tab>Three</c-tab>
      </c-tab-list> <c-tab-panels>
        <c-tab-panel>
          <p>one</p>
        </c-tab-panel>
        <c-tab-panel>
          <p>two</p>
        </c-tab-panel>
        <c-tab-panel>
          <p>three</p>
        </c-tab-panel>
      </c-tab-panels>
    </c-tabs>
  </c-box>
</template><script>
import {
  CBox,
  CTab,
  CTabs,
  CTabList,
  CTabPanel,
  CTabPanels,
} from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CTab,
    CTabs,
    CTabList,
    CTabPanel,
    CTabPanels,
  },
};
</script>
```

我们添加`c-tabs`组件来添加选项卡和选项卡面板容器。

`c-tab-list`是选项卡的容器。

`c-tab`包装每个标签的内容。

`c-tab-panels`有选项卡面板。

`c-tab-panel`有选项卡面板。

我们可以用`variant`道具改变标签的样式:

```
<template>
  <c-box>
    <c-tabs variant="enclosed">
      <c-tab-list>
        <c-tab>One</c-tab>
        <c-tab>Two</c-tab>
        <c-tab>Three</c-tab>
      </c-tab-list> <c-tab-panels>
        <c-tab-panel>
          <p>one</p>
        </c-tab-panel>
        <c-tab-panel>
          <p>two</p>
        </c-tab-panel>
        <c-tab-panel>
          <p>three</p>
        </c-tab-panel>
      </c-tab-panels>
    </c-tabs>
  </c-box>
</template><script>
import {
  CBox,
  CTab,
  CTabs,
  CTabList,
  CTabPanel,
  CTabPanels,
} from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CTab,
    CTabs,
    CTabList,
    CTabPanel,
    CTabPanels,
  },
};
</script>
```

`variant`的可能值为:

*   `line`
*   `enclosed`
*   `enclosed-colored`
*   `soft-rounded`
*   `solid-rounded`
*   `unstyled`

我们可以用以下方式设置`variant-color`道具:

```
<template>
  <c-box>
    <c-tabs variant="soft-rounded" variant-color="green">
      <c-tab-list>
        <c-tab>One</c-tab>
        <c-tab>Two</c-tab>
        <c-tab>Three</c-tab>
      </c-tab-list> <c-tab-panels>
        <c-tab-panel>
          <p>one</p>
        </c-tab-panel>
        <c-tab-panel>
          <p>two</p>
        </c-tab-panel>
        <c-tab-panel>
          <p>three</p>
        </c-tab-panel>
      </c-tab-panels>
    </c-tabs>
  </c-box>
</template><script>
import {
  CBox,
  CTab,
  CTabs,
  CTabList,
  CTabPanel,
  CTabPanels,
} from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CTab,
    CTabs,
    CTabList,
    CTabPanel,
    CTabPanels,
  },
};
</script>
```

设置选项卡被选中时的背景颜色。

我们可以用`size`道具设置标签尺寸:

```
<template>
  <c-box>
    <c-tabs variant="soft-rounded" size="sm" variant-color="green">
      <c-tab-list>
        <c-tab>One</c-tab>
        <c-tab>Two</c-tab>
        <c-tab>Three</c-tab>
      </c-tab-list><c-tab-panels>
        <c-tab-panel>
          <p>one</p>
        </c-tab-panel>
        <c-tab-panel>
          <p>two</p>
        </c-tab-panel>
        <c-tab-panel>
          <p>three</p>
        </c-tab-panel>
      </c-tab-panels>
    </c-tabs>
  </c-box>
</template><script>
import {
  CBox,
  CTab,
  CTabs,
  CTabList,
  CTabPanel,
  CTabPanels,
} from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CTab,
    CTabs,
    CTabList,
    CTabPanel,
    CTabPanels,
  },
};
</script>
```

`size`可以是`sm`、`md`或`lg`，从最小到最大排序。

我们可以改变标签与`align`支柱的对齐方式:

```
<template>
  <c-box>
    <c-tabs align="end">
      <c-tab-list>
        <c-tab>One</c-tab>
        <c-tab>Two</c-tab>
        <c-tab>Three</c-tab>
      </c-tab-list> <c-tab-panels>
        <c-tab-panel>
          <p>one</p>
        </c-tab-panel>
        <c-tab-panel>
          <p>two</p>
        </c-tab-panel>
        <c-tab-panel>
          <p>three</p>
        </c-tab-panel>
      </c-tab-panels>
    </c-tabs>
  </c-box>
</template><script>
import {
  CBox,
  CTab,
  CTabs,
  CTabList,
  CTabPanel,
  CTabPanels,
} from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CTab,
    CTabs,
    CTabList,
    CTabPanel,
    CTabPanels,
  },
};
</script>
```

我们可以用`is-fitted`支撑物将拉环拉伸到面板的宽度:

```
<template>
  <c-box>
    <c-tabs is-fitted>
      <c-tab-list>
        <c-tab>One</c-tab>
        <c-tab>Two</c-tab>
        <c-tab>Three</c-tab>
      </c-tab-list> <c-tab-panels>
        <c-tab-panel>
          <p>one</p>
        </c-tab-panel>
        <c-tab-panel>
          <p>two</p>
        </c-tab-panel>
        <c-tab-panel>
          <p>three</p>
        </c-tab-panel>
      </c-tab-panels>
    </c-tabs>
  </c-box>
</template><script>
import {
  CBox,
  CTab,
  CTabs,
  CTabList,
  CTabPanel,
  CTabPanels,
} from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CTab,
    CTabs,
    CTabList,
    CTabPanel,
    CTabPanels,
  },
};
</script>
```

并且我们可以用`default-index`道具设置默认选中的标签页:

```
<template>
  <c-box>
    <c-tabs :default-index="2">
      <c-tab-list>
        <c-tab>One</c-tab>
        <c-tab>Two</c-tab>
        <c-tab>Three</c-tab>
      </c-tab-list> <c-tab-panels>
        <c-tab-panel>
          <p>one</p>
        </c-tab-panel>
        <c-tab-panel>
          <p>two</p>
        </c-tab-panel>
        <c-tab-panel>
          <p>three</p>
        </c-tab-panel>
      </c-tab-panels>
    </c-tabs>
  </c-box>
</template><script>
import {
  CBox,
  CTab,
  CTabs,
  CTabList,
  CTabPanel,
  CTabPanels,
} from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CTab,
    CTabs,
    CTabList,
    CTabPanel,
    CTabPanels,
  },
};
</script>
```

我们可以用`is-disabled`道具禁用一个标签:

```
<template>
  <c-box>
    <c-tabs>
      <c-tab-list>
        <c-tab>One</c-tab>
        <c-tab is-disabled>>Two</c-tab>
        <c-tab>Three</c-tab>
      </c-tab-list> <c-tab-panels>
        <c-tab-panel>
          <p>one</p>
        </c-tab-panel>
        <c-tab-panel>
          <p>two</p>
        </c-tab-panel>
        <c-tab-panel>
          <p>three</p>
        </c-tab-panel>
      </c-tab-panels>
    </c-tabs>
  </c-box>
</template><script>
import {
  CBox,
  CTab,
  CTabs,
  CTabList,
  CTabPanel,
  CTabPanels,
} from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CTab,
    CTabs,
    CTabList,
    CTabPanel,
    CTabPanels,
  },
};
</script>
```

# 结论

我们可以通过 Chakra UI Vue 轻松添加标签。