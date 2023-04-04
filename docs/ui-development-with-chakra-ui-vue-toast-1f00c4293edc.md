# 使用 Chakra UI Vue-Toast 进行 UI 开发

> 原文：<https://blog.devgenius.io/ui-development-with-chakra-ui-vue-toast-1f00c4293edc?source=collection_archive---------4----------------------->

![](img/0e38a5247482bf1bf40df730ad79e212.png)

[Toa Heftiba](https://unsplash.com/@heftiba?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Chakra UI Vue 是一个为 Vue.js 制作的 UI 框架，让我们可以将好看的 UI 组件添加到我们的 Vue 应用程序中。

本文将介绍如何开始使用 Chakra UI Vue 进行 UI 开发。

# 烤

Chakra UI Vue 自带吐司组件。

要添加它，我们可以写:

```
<template>
  <c-box>
    <c-button @click="showToast">Show Toast</c-button>
  </c-box>
</template><script>
import { CBox, CButton } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CButton,
  },
  methods: {
    showToast() {
      this.$toast({
        title: "Account created.",
        description: "We've created your account for you.",
        status: "info",
        duration: 10000,
      });
    },
  },
};
</script>
```

我们有一个`c-button`,当我们点击它时，它调用`showToast`方法。

在`showToast`方法中，我们调用`this.$toast`方法来打开敬酒词。

`title`有此称号。

`description`有主要内容的文字。

`status`设置要显示的吐司类型。

`status`也可以是`'success'`、`'warning'`或`'error'`。

`duration`是显示 toast 的持续时间(毫秒)。

我们还可以设置一个组件来显示 toast 内容。

例如，我们可以写:

```
<template>
  <c-box>
    <c-button @click="showToast">Show Toast</c-button>
  </c-box>
</template><script>
import { CBox, CButton } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CButton,
  },
  methods: {
    showToast() {
      this.$toast({
        position: "bottom-left",
        render: () => {
          return this.$createElement(
            "c-box",
            {
              props: {
                color: "white",
                p: 3,
                m: 3,
                bg: "blue.500",
              },
            },
            "Hello World!"
          );
        },
      });
    },
  },
};
</script>
```

我们有返回`c-box`组件作为 toast 内容的`render`方法。

`props`属性有我们传入`c-box`的道具。

第三个参数是`c-box`的子内容。

所以当我们点击`c-button`，‘你好，世界！’应该显示出来。

`position`设定烤面包的位置。

`variant`属性改变背景颜色样式。

例如，如果我们有:

```
<template>
  <c-box>
    <c-button @click="showToast">Show Toast</c-button>
  </c-box>
</template><script>
import { CBox, CButton } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CButton,
  },
  methods: {
    showToast() {
      this.$toast({
        title: "Account created.",
        description: "We've created your account for you.",
        status: "info",
        duration: 10000,
        variant: "subtle",
      });
    },
  },
};
</script>
```

然后因为`variant`设置为`'subtle'`，所以吐司背景颜色变浅。

# 结论

我们可以用 Chakra UI Vue 加一个祝酒词。