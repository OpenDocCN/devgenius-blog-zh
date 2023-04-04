# 使用 Chakra UI Vue 开发 UI——数字输入风格和弹出框

> 原文：<https://blog.devgenius.io/ui-development-with-chakra-ui-vue-number-input-styles-and-popovers-bbb3eb621174?source=collection_archive---------6----------------------->

![](img/a73981ff5c29d2a6167a6c4bfa578923.png)

萨姆·穆卡达姆在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Chakra UI Vue 是一个为 Vue.js 制作的 UI 框架，让我们可以将好看的 UI 组件添加到我们的 Vue 应用程序中。

本文将介绍如何开始使用 Chakra UI Vue 进行 UI 开发。

# 数字输入样式

我们可以改变数字输入的按钮样式。

为此，我们写道:

```
<template>
  <c-box>
    <c-number-input size="sm" :default-value="15" clamp-value-on-blur :max="30">
      <c-number-input-field focus-border-color="red.200" />
      <c-number-input-stepper>
        <c-number-increment-stepper
          bg="green.200"
          :_active="{ bg: 'green.300' }"
        >
          +
        </c-number-increment-stepper>
        <c-number-decrement-stepper bg="pink.200" :_active="{ bg: 'pink.300' }">
          -
        </c-number-decrement-stepper>
      </c-number-input-stepper>
    </c-number-input>
  </c-box>
</template><script>
import {
  CBox,
  CNumberInput,
  CNumberInputField,
  CNumberInputStepper,
  CNumberIncrementStepper,
  CNumberDecrementStepper,
} from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CNumberInput,
    CNumberInputField,
    CNumberInputStepper,
    CNumberIncrementStepper,
    CNumberDecrementStepper,
  },
  data() {
    return {
      value: 15,
    };
  },
};
</script>
```

我们设置`bg`道具来设置步进按钮的背景颜色。

`_active` prop 设置按钮激活时应用的样式。

# 波普沃

我们可以添加一个带有`c-popover`组件的 popover。

例如，我们可以写:

```
<template>
  <c-box>
    <c-popover placement="top">
      <c-popover-trigger>
        <c-button>Trigger</c-button>
      </c-popover-trigger>
      <c-popover-content z-index="4">
        <c-popover-arrow />
        <c-popover-close-button />
        <c-popover-header>Confirmation!</c-popover-header>
        <c-popover-body>
          Are you sure you want to have that milkshake?
        </c-popover-body>
      </c-popover-content>
    </c-popover>
  </c-box>
</template><script>
import {
  CBox,
  CButton,
  CPopover,
  CPopoverTrigger,
  CPopoverContent,
  CPopoverHeader,
  CPopoverBody,
  CPopoverArrow,
  CPopoverCloseButton,
} from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CButton,
    CPopover,
    CPopoverTrigger,
    CPopoverContent,
    CPopoverHeader,
    CPopoverBody,
    CPopoverArrow,
    CPopoverCloseButton,
  },
};
</script>
```

我们添加了一些组件的 popover。

`c-popover`是主容器。

`c-popover-trigger`有触发弹出菜单的按钮。

`c-popover-content`是 popover 内容的容器。

`c-popover-arrow`是 popover 箭头。

`c-popover-close-button`是弹出关闭按钮。

`c-popover-header`是 popover 头。

是主要 popover 内容的容器。

我们可以用一些老虎机道具来获得 popover 的状态:

```
<template>
  <c-box>
    <c-popover
      initialFocusRef="#closeButton"
      placement="right"
      v-slot="{ isOpen, onClose }"
    >
      <c-popover-trigger>
        <c-button>Click to {{ isOpen ? "close" : "open" }}</c-button>
      </c-popover-trigger>
      <c-popover-content z-index="4">
        <c-popover-arrow />
        <c-popover-close-button />
        <c-popover-header>Confirmation!</c-popover-header>
        <c-popover-body>
          <c-box>
            Hello. Nice to meet you! This is the body of the popover
          </c-box>
          <c-button
            mt="4"
            variantColor="blue"
            @click="onClose"
            id="closeButton"
          >
            Close
          </c-button>
        </c-popover-body>
      </c-popover-content>
    </c-popover>
  </c-box>
</template><script>
import {
  CBox,
  CButton,
  CPopover,
  CPopoverTrigger,
  CPopoverContent,
  CPopoverHeader,
  CPopoverBody,
  CPopoverArrow,
  CPopoverCloseButton,
} from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CButton,
    CPopover,
    CPopoverTrigger,
    CPopoverContent,
    CPopoverHeader,
    CPopoverBody,
    CPopoverArrow,
    CPopoverCloseButton,
  },
};
</script>
```

`isOpen`是弹出器打开时的`true`。

`onClose`是一个在我们运行时关闭弹出窗口的函数。

# 结论

我们可以用 Chakra Vue UI 添加数字输入和弹出。