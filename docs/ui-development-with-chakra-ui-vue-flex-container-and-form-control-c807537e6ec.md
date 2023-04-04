# 使用 Chakra UI Vue 开发 UI——Flex 容器和表单控件

> 原文：<https://blog.devgenius.io/ui-development-with-chakra-ui-vue-flex-container-and-form-control-c807537e6ec?source=collection_archive---------6----------------------->

![](img/700472e1027f0f3bcb9accf7747e1aa6.png)

由[阿洛拉·格里菲斯](https://unsplash.com/@aloragriffiths?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Chakra UI Vue 是一个为 Vue.js 制作的 UI 框架，让我们可以将好看的 UI 组件添加到我们的 Vue 应用程序中。

本文将介绍如何开始使用 Chakra UI Vue 进行 UI 开发。

# 柔性容器

我们可以添加一个带有`c-flex`组件的 flex 容器。

例如，我们可以写:

```
<template>
  <c-box>
    <c-flex align="center">
      <c-flex bg="green.50" align="flex-end">
        <c-text>Box 1</c-text>
      </c-flex>
      <c-flex bg="blue.50" size="150px" align="center" justify="center">
        <c-text text-align="center" bg="orange.50"> Box 2 </c-text>
      </c-flex>
      <c-box>
        <c-text bg="tomato" color="white"> Box 3 </c-text>
      </c-box>
    </c-flex>
  </c-box>
</template><script>
import { CBox, CFlex, CText } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CFlex,
    CText,
  },
};
</script>
```

我们添加`c-flex`组件来添加一个容器，该容器的 CSS `display`属性设置为`flex`。

`bg`道具设置背景颜色。

`align`设置`justify-items` CSS 属性。

`text-align`设置`text-align` CSS 属性。

`size`设置宽度和高度。

# 表单控件

我们可以用`c-form-control`组件添加一个表单控件容器。

例如，我们可以写:

```
<template>
  <c-box>
    <c-form-control>
      <c-form-label for="email">Email address</c-form-label>
      <c-input type="email" id="email" />
      <c-form-helper-text> We'll never share your email. </c-form-helper-text>
    </c-form-control>
  </c-box>
</template><script>
import {
  CBox,
  CFormControl,
  CFormLabel,
  CFormHelperText,
  CInput,
} from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CFormControl,
    CFormLabel,
    CFormHelperText,
    CInput,
  },
};
</script>
```

我们添加了`c-form-label`组件来为输入添加标签。

`c-input`是文本输入。

`c-form-helper-text`是帮助器文本的容器。

# 单选按钮

我们可以把单选按钮放在一个`c-form-control`里面。

为此，我们写道:

```
<template>
  <c-box>
    <c-form-control>
      <c-form-label as="legend">Favorite fruit</c-form-label>
      <c-radio-group default-value="orange">
        <c-radio value="apple">apple</c-radio>
        <c-radio value="orange">orange</c-radio>
        <c-radio value="grape">grape</c-radio>
      </c-radio-group>
      <c-form-helper-text> Select only if you're a fan. </c-form-helper-text>
    </c-form-control>
  </c-box>
</template><script>
import {
  CBox,
  CFormControl,
  CFormLabel,
  CFormHelperText,
  CRadio,
  CRadioGroup,
} from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CFormControl,
    CFormLabel,
    CFormHelperText,
    CRadio,
    CRadioGroup,
  },
};
</script>
```

我们添加`c-radio-group`组件来分组`c-radio`组件。

`as`设置标签以将组件呈现为。

`default-value`设置默认值，从组中选择。

每个`c-radio`组件都有一个`value`道具来设置数值。

# 结论

我们可以用 Chakra UI Vue 添加 flex 容器和表单控件。