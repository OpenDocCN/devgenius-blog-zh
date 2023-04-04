# 使用 Chakra UI Vue 的 UI 开发—输入中的元素和输入焦点和错误

> 原文：<https://blog.devgenius.io/ui-development-with-chakra-ui-vue-elements-in-inputs-and-input-focus-and-error-e6a762519827?source=collection_archive---------5----------------------->

![](img/1b808ccdd8d8e19c6c0b5e09928be96d.png)

叶卡捷琳娜·格里希娜在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Chakra UI Vue 是一个为 Vue.js 制作的 UI 框架，让我们可以将好看的 UI 组件添加到我们的 Vue 应用程序中。

本文将介绍如何开始使用 Chakra UI Vue 进行 UI 开发。

# 投入要素

我们可以用`c-input-left-element`或`c-input-right-element`组件在输入中添加元素。

`c-input-left-element`在输入左侧添加一个元素。

而`c-input-right-element`在输入右侧增加一个元素。

例如，我们可以写:

```
<template>
  <c-box>
    <c-input-group size="md">
      <c-input
        pr="4.5rem"
        :type="show ? 'text' : 'password'"
        placeholder="Enter password"
        v-model="password"
      />
      <c-input-right-element width="4.5rem">
        <c-button h="1.75rem" size="sm" @click="show = !show">
          {{ show ? "Hide" : "Show" }}
        </c-button>
      </c-input-right-element>
    </c-input-group>
  </c-box>
</template><script>
import {
  CBox,
  CInputGroup,
  CInput,
  CInputRightElement,
  CButton,
} from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CInputGroup,
    CInput,
    CInputRightElement,
    CButton,
  },
  data() {
    return {
      password: "",
      show: false,
    };
  },
};
</script>
```

在输入的右侧添加一个显示按钮，让用户看到他们输入了什么密码。

# 输入焦点和错误

我们可以设置`focus-border-color`道具来设置我们关注输入时的边框颜色。

例如，我们可以写:

```
<template>
  <c-box>
    <c-input
      focus-border-color="lime"
      placeholder="Here is a sample placeholder"
    />
  </c-box>
</template><script>
import { CBox, CInput } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CInput,
  }
};
</script>
```

当我们关注它时，将边框颜色设置为石灰色。

我们可以添加`is-invalid`道具向用户显示输入无效。

并且我们可以设置`error-border-color`来设置输入无效时输入的边框颜色。

例如，我们可以写:

```
<template>
  <c-box>
    <c-input
      is-invalid
      error-border-color="red.300"
      placeholder="Here is a sample placeholder"
    />
  </c-box>
</template><script>
import { CBox, CInput } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CInput,
  },
};
</script>
```

来添加道具。

# 结论

我们可以用 Chakra UI Vue 添加各种风格的输入。