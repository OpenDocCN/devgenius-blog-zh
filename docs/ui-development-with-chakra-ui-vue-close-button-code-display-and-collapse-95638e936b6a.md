# 使用 Chakra UI Vue 开发 UI——关闭按钮、代码显示和折叠

> 原文：<https://blog.devgenius.io/ui-development-with-chakra-ui-vue-close-button-code-display-and-collapse-95638e936b6a?source=collection_archive---------5----------------------->

![](img/be27c6f2a2a2713a7bb90a74eb6a727d.png)

卡洛斯·金特罗在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Chakra UI Vue 是一个为 Vue.js 制作的 UI 框架，让我们可以将好看的 UI 组件添加到我们的 Vue 应用程序中。

本文将介绍如何开始使用 Chakra UI Vue 进行 UI 开发。

# 关闭按钮

Chakra UI Vue 带有一个关闭按钮组件。

为了补充它，我们写道:

```
<template>
  <c-box> <c-close-button /> </c-box>
</template><script>
import { CBox, CCloseButton } from "@chakra-ui/vue";
export default {
  components: {
    CBox,
    CCloseButton,
  },
};
</script>
```

添加`c-close-button`组件。

我们可以用`size`道具改变它的大小:

```
<template>
  <c-box> <c-close-button size="lg" /> </c-box>
</template><script>
import { CBox, CCloseButton } from "@chakra-ui/vue";
export default {
  components: {
    CBox,
    CCloseButton,
  },
};
</script>
```

# 代码显示

我们可以使用`c-code`组件将代码显示添加到我们的 Vue 应用程序中。

例如，我们可以写:

```
<template>
  <c-box> <c-code>Hello world</c-code></c-box>
</template><script>
import { CBox, CCode } from "@chakra-ui/vue";
export default {
  components: {
    CBox,
    CCode,
  },
};
</script>
```

添加显示为代码的文本。

我们可以用`variant-color`道具改变背景:

```
<template>
  <c-box>
    <c-stack is-inline>
      <c-code>console.log(welcome)</c-code>
      <c-code variant-color="red">var chakra = 'awesome!'></c-code>
      <c-code variant-color="yellow">npm install chakra</c-code>
    </c-stack>
  </c-box>
</template>
<script>
import { CBox, CCode, CStack } from "@chakra-ui/vue";
export default {
  components: {
    CBox,
    CCode,
    CStack,
  },
};
</script>
```

# 倒塌

查克拉 UI Vue 带有塌陷组件。

为了补充它，我们写道:

```
<template>
  <c-box>
    <c-button mb="4" variant-color="blue" @click="show = !show">
      Toggle
    </c-button>
    <c-collapse :is-open="show">
      Anim pariatur cliche reprehenderit
    </c-collapse>
  </c-box>
</template>
<script>
import { CBox, CButton, CCollapse } from "@chakra-ui/vue";
export default {
  components: {
    CBox,
    CButton,
    CCollapse,
  },
  data() {
    return {
      show: false,
    };
  },
};
</script>
```

我们有`c-button`组件，让我们点击切换`show`反应属性。

我们使用`show`来控制是否显示`c-collapse`组件，方法是将`show`作为`is-open`属性的值。

我们可以用`starting-height`道具设置折叠组件的高度:

```
<template>
  <c-box>
    <c-button mb="4" variant-color="blue" @click="show = !show">
      Toggle
    </c-button>
    <c-collapse :is-open="show" :starting-height="20">
      Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vestibulum id
      magna in diam porttitor hendrerit quis ut quam. Aliquam et sem eget eros
      sodales pretium sit amet sit amet dolor.
    </c-collapse>
  </c-box>
</template>
<script>
import { CBox, CButton, CCollapse } from "@chakra-ui/vue";
export default {
  components: {
    CBox,
    CButton,
    CCollapse,
  },
  data() {
    return {
      show: false,
    };
  },
};
</script>
```

`20`以像素为单位。

# 结论

我们可以用 Chakra UI Vue 添加关闭按钮、代码显示和折叠组件。