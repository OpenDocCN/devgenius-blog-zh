# 使用 Chakra UI Vue 开发 UI——控制盒

> 原文：<https://blog.devgenius.io/ui-development-with-chakra-ui-vue-control-box-4309375261ff?source=collection_archive---------6----------------------->

![](img/e35ef2ce561e120915ced873105fa708.png)

Claudio Schwarz | @purzlbaum 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Chakra UI Vue 是一个为 Vue.js 制作的 UI 框架，让我们可以将好看的 UI 组件添加到我们的 Vue 应用程序中。

本文将介绍如何开始使用 Chakra UI Vue 进行 UI 开发。

# 控制箱

我们可以使用`c-control-box`组件来添加一个组件，该组件根据同级复选框或单选输入来改变样式。

例如，我们可以写:

```
<template>
  <c-box>
     <label>
      <c-visually-hidden as="input" type="checkbox" default-checked />
      <c-control-box
        border-width="1px"
        size="24px"
        rounded="sm"
        :_checked="{ bg: 'green.500', color: 'white', borderColor: 'green.500' }""
        :_focus="{ borderColor: 'green.600', boxShadow: 'outline' }""
      >
        <c-icon name="check" size="16px" />
      </c-control-box>
      <c-box as="span" vertical-align="top" ml="3">
        Checkbox Label
      </c-box>
    </label>
  </c-box>
</template><script>
import { CBox, CControlBox, CIcon, CVisuallyHidden } from "@chakra-ui/vue";
export default {
  components: {
    CBox,
    CControlBox,
    CIcon,
    CVisuallyHidden,
  },
};
</script>
```

我们用带有`_checked`道具的`c-control-box`组件来设置`c-visually-hidden`组件的`bg`和`color`道具。

当我们点击复选框时，我们有`_focus`属性来设置`c-visually-hidden`的`borderColor`和`boxShadow`样式。

我们可以用单选按钮做同样的事情:

```
<template>
  <c-box>
    <label>
      <c-visually-hidden type="radio" as="input" />
      <c-control-box
        size="24px"
        bg="white"
        border="2px"
        rounded="full"
        type="radio"
        borderColor="inherit"
        :_focus="{ boxShadow: 'outline' }"
        :_hover="{ borderColor: 'gray.300' }"
        :_disabled="{ opacity: '40%' }"
        :_checked="{ bg: 'green.500', borderColor: 'green.500' }"
      >
        <c-box w="50%" h="50%" bg="white" rounded="full" />
      </c-control-box>
      <c-box as="span" ml="2" vertical-align="center" user-select="none">
        This is a Radio
      </c-box>
    </label>
  </c-box>
</template><script>
import { CBox, CControlBox, CVisuallyHidden } from "@chakra-ui/vue";
export default {
  components: {
    CBox,
    CControlBox,
    CVisuallyHidden,
  },
};
</script>
```

我们有`_disabled`属性来设置`c-visually-hidden`被禁用时的样式。

我们有`_hover`道具来设置`c-visually-hidden`的样式。

# 结论

我们可以使用`c-control-box`组件来添加一个组件，该组件可以根据兄弟复选框或单选输入来改变 Vue 应用程序的样式。