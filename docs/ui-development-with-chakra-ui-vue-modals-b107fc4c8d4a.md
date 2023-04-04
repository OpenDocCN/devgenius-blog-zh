# 使用 Chakra UI Vue-Modals 开发 UI

> 原文：<https://blog.devgenius.io/ui-development-with-chakra-ui-vue-modals-b107fc4c8d4a?source=collection_archive---------5----------------------->

![](img/12da6438ecfbebb51a19fa79634cae6c.png)

[Gigi](https://unsplash.com/@ling_gigi?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Chakra UI Vue 是一个为 Vue.js 制作的 UI 框架，让我们可以将好看的 UI 组件添加到我们的 Vue 应用程序中。

本文将介绍如何开始使用 Chakra UI Vue 进行 UI 开发。

# 情态的

我们可以用查克拉 UI Vue 添加一个模态。

要添加一个，我们写:

```
<template>
  <c-box>
    <c-button
      left-icon="check"
      mb="3"
      variant-color="blue"
      @click="open"
      variant="outline"
    >
      Open Modal
    </c-button>
    <c-modal :is-open="isOpen" :on-close="close">
      <c-modal-content ref="content">
        <c-modal-header>Modal Title</c-modal-header>
        <c-modal-close-button />
        <c-modal-body>
          <Lorem add="2s" />
        </c-modal-body>
        <c-modal-footer>
          <c-button variant-color="blue" mr="3"> Save </c-button>
          <c-button @click="close">Cancel</c-button>
        </c-modal-footer>
      </c-modal-content>
      <c-modal-overlay />
    </c-modal>
  </c-box>
</template><script>
import {
  CBox,
  CButton,
  CModal,
  CModalOverlay,
  CModalContent,
  CModalHeader,
  CModalFooter,
  CModalBody,
  CModalCloseButton,
} from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CButton,
    CModal,
    CModalOverlay,
    CModalContent,
    CModalHeader,
    CModalFooter,
    CModalBody,
    CModalCloseButton,
  },
  data() {
    return {
      isOpen: false,
    };
  },
  methods: {
    open() {
      this.isOpen = true;
    },
    close() {
      this.isOpen = false;
    },
  },
};
</script>
```

我们注册创建模型所需的组件。

是为其子节点提供上下文的包装器

`CModalOverlay`是模式对话框后面的灰色覆盖图

`CModalContent`是模态对话框内容的容器

`CModalHeader`是标记模式对话框的标题

`CModalFooter`是包含模态动作的页脚

`CModalBody`是容纳模态主要内容的包装器

而`CModalCloseButton`是关闭模态的按钮。

我们将`is-open`属性设置为一个反应属性，让我们控制何时打开模态。

`on-close` prop 采用了一个方法，当我们通过在模态之外单击来关闭它时，这个方法就会运行。

我们可以将`initial-focus-ref`属性设置为一个函数，该函数返回我们打开模态时要关注的元素的 ref。

我们可以将`final-focus-ref`属性设置为一个函数，当我们关闭模态时，该函数返回要关注的元素的 ref。

例如，我们可以写:

```
<template>
  <c-box>
    <c-button mr="3" @click="open">Open Modal</c-button>
    <c-button ref="finalRef"> I'll receive focus on close </c-button>
    <c-modal
      :initial-focus-ref="() => $refs.initialRef"
      :final-focus-ref="() => $refs.finalRef"
      :is-open="isOpen"
      :on-close="close"
    >
      <c-modal-content ref="content">
        <c-modal-header>Create your account</c-modal-header>
        <c-modal-close-button />
        <c-modal-body mr="8">
          <c-form-control>
            <c-form-label>First name</c-form-label>
            <c-input ref="initialRef" placeholder="First name" />
          </c-form-control> <c-form-control mt="4">
            <c-form-label>Last name</c-form-label>
            <c-input placeholder="Last name" />
          </c-form-control>
        </c-modal-body>
        <c-modal-footer>
          <c-button variant-color="blue" mr="3"> Cancel </c-button>
          <c-button @click="close">Save</c-button>
        </c-modal-footer>
      </c-modal-content>
      <c-modal-overlay />
    </c-modal>
  </c-box>
</template><script>
import {
  CBox,
  CButton,
  CModal,
  CModalOverlay,
  CModalContent,
  CModalHeader,
  CModalFooter,
  CModalBody,
  CModalCloseButton,
  CInput,
  CFormControl,
  CFormLabel,
} from "[@chakra](http://twitter.com/chakra)-ui/vue";export default {
  components: {
    CBox,
    CButton,
    CModal,
    CModalOverlay,
    CModalContent,
    CModalHeader,
    CModalFooter,
    CModalBody,
    CModalCloseButton,
    CInput,
    CFormControl,
    CFormLabel,
  },
  data() {
    return {
      isOpen: false,
    };
  },
  methods: {
    open() {
      this.isOpen = true;
    },
    close() {
      this.isOpen = false;
    },
  },
};
</script>
```

以便当我们打开模式时名字字段被聚焦。

当模态关闭时，“我将在关闭时接收焦点”按钮将被聚焦。

# 结论

我们可以用 Chakra UI Vue 在我们的 Vue 应用程序中添加一个模态。