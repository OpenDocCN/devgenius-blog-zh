# 使用 Chakra UI Vue 开发 UI——分隔器和抽屉

> 原文：<https://blog.devgenius.io/ui-development-with-chakra-ui-vue-dividers-and-drawers-4ec5c8079b15?source=collection_archive---------8----------------------->

![](img/df569c8c6c7139a043b37eb4b9ee7ffe.png)

照片由[维多利亚·比尔斯伯勒](https://unsplash.com/@vicbils?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Chakra UI Vue 是一个为 Vue.js 制作的 UI 框架，让我们可以将好看的 UI 组件添加到我们的 Vue 应用程序中。

本文将介绍如何开始使用 Chakra UI Vue 进行 UI 开发。

# 圆规

Chakra UI Vue 带有一个分隔组件。

为了补充它，我们写道:

```
<template>
  <c-box>
    <p>Part 1</p>
    <c-divider />
    <p>Part 2</p>
  </c-box>
</template><script>
import { CBox, CDivider } from "@chakra-ui/vue";
export default {
  components: {
    CBox,
    CDivider,
  },
};
</script>
```

用`c-divider`组件添加水平分隔线。

我们还可以添加一个垂直分隔线:

```
<template>
  <c-flex>
    <span>Part 1</span>
    <c-divider orientation="vertical" />
    <span>Part 2</span>
  </c-flex>
</template><script>
import { CFlex, CDivider } from "@chakra-ui/vue";
export default {
  components: {
    CFlex,
    CDivider,
  },
};
</script>
```

我们只是将`orientation`设置为`vertical`来添加它。

我们可以用`border-color`道具改变边框颜色:

```
<template>
  <c-box>
    <p>Part 1</p>
    <c-divider border-color="red.200" />
    <p>Part 2</p>
  </c-box>
</template>
<script>
import { CBox, CDivider } from "@chakra-ui/vue";
export default {
  components: {
    CBox,
    CDivider,
  },
};
</script>
```

# 抽屉

Chakra UI Vue 附带了一些组件，可以让我们在 Vue 应用程序中添加一个抽屉。

为了补充它，我们写道:

```
<template>
  <c-box>
    <c-button ref="btnRef" @click="isOpen = true">Open Drawer</c-button> <c-drawer
      :isOpen="isOpen"
      placement="right"
      :on-close="close"
      :initialFocusRef="() => $refs.inputInsideModal"
    >
      <c-drawer-overlay />
      <c-drawer-content>
        <c-drawer-close-button />
        <c-drawer-header>Create your account</c-drawer-header> <c-drawer-body>
          <c-input ref="inputInsideModal" placeholder="Type here..." />
        </c-drawer-body> <c-drawer-footer>
          <c-button variant="outline" mr="3" @click="isOpen = false"
            >Cancel</c-button
          >
          <c-button variant-color="blue">Save</c-button>
        </c-drawer-footer>
      </c-drawer-content>
    </c-drawer>
  </c-box>
</template>
<script>
import {
  CBox,
  CInput,
  CButton,
  CDrawer,
  CDrawerBody,
  CDrawerFooter,
  CDrawerHeader,
  CDrawerOverlay,
  CDrawerContent,
  CDrawerCloseButton,
} from "@chakra-ui/vue";
export default {
  components: {
    CBox,
    CInput,
    CButton,
    CDrawer,
    CDrawerBody,
    CDrawerFooter,
    CDrawerHeader,
    CDrawerOverlay,
    CDrawerContent,
    CDrawerCloseButton,
  },
  data() {
    return {
      isOpen: false,
    };
  },
  methods: {
    close() {
      this.isOpen = false;
    },
  },
};
</script>
```

我们导入并注册`CDrawer`、`CDrawerBody`、`CDrawerFooter`、`CDrawerHeader`、`CDrawerOverlay`、`CDrawerContent`、`CDrawerCloseButton`组件来添加一个抽屉。

`CDrawer`是主抽屉容器。

`CDrawerBody`是抽屉主体的容器。

`CDrawerFooter`是抽屉页脚的容器。

`CDrawerHeader`是抽屉头的容器。

`CDrawerOverlay`是抽屉贴面。

`CDrawerContent`是抽屉内容的容器。

`CDrawerCloseButton`是抽屉关闭按钮。

我们将`isOpen`道具设置为`isOpen`反应属性，让我们控制何时打开或关闭抽屉。

当我们点击`c-button`时，因为`isOpen`被设置为`true`，所以`c-button`会打开抽屉。

`initialFocusRef`设置抽屉打开时我们最初想要关注的元素。

`placement`设置抽屉的放置位置。我们可以设置为`top`、`right`、`bottom`或者`left`。

# 结论

我们可以使用 Chakra UI Vue 轻松添加隔断和抽屉。