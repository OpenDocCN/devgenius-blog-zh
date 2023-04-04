# 使用 Chakra UI Vue 开发 UI——图像和输入

> 原文：<https://blog.devgenius.io/ui-development-with-chakra-ui-vue-images-and-inputs-17fb440fd217?source=collection_archive---------0----------------------->

![](img/5ff5d274900a89272df2d10bf6355e55.png)

莎伦·麦卡琴在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

Chakra UI Vue 是一个为 Vue.js 制作的 UI 框架，让我们可以将好看的 UI 组件添加到我们的 Vue 应用程序中。

本文将介绍如何开始使用 Chakra UI Vue 进行 UI 开发。

# 形象

我们可以用`c-image`组件添加图像。

例如，我们可以写:

```
<template>
  <c-box>
    <c-image src="https://picsum.photos/id/237/200/300" alt="dog" />
  </c-box>
</template><script>
import { CBox, CImage } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CImage,
  },
};
</script>
```

我们将`src`属性设置为图像的 URL。

`alt`有图像的文字描述。

`size`道具设定大小:

```
<template>
  <c-box>
    <c-image
      size="100px"
      src="https://picsum.photos/id/237/200/300"
      alt="dog"
    />
  </c-box>
</template><script>
import { CBox, CImage } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CImage,
  },
};
</script>
```

我们可以设置一个回退图像的 URL，当实际图像无法用`fallback-src`属性加载时，它会加载:

```
<template>
  <c-box>
    <c-image
      size="100px"
      src="abc"
      alt="dog"
      fallback-src="https://via.placeholder.com/150"
    />
  </c-box>
</template><script>
import { CBox, CImage } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CImage,
  },
};
</script>
```

# 投入

我们可以用`c-input`组件添加一个输入:

```
<template>
  <c-box>
    <c-input placeholder="Basic Input" />
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

`size`道具设定大小:

```
<template>
  <c-box>
    <c-input placeholder="Basic Input" size="lg" />
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

`lg`大。

`size`值也可以设置为`md`表示中，设置为`sm`表示小。

`variant`属性设置输入的样式变量。

例如，我们可以写:

```
<template>
  <c-box>
    <c-stack spacing="3">
      <c-input variant="outline" placeholder="Outline" />
      <c-input variant="filled" placeholder="Filled" />
      <c-input variant="flushed" placeholder="Flushed" />
      <c-input variant="unstyled" placeholder="Unstyled" />
    </c-stack>
  </c-box>
</template><script>
import { CBox, CStack, CInput } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CStack,
    CInput,
  },
};
</script>
```

设置`variant`道具来设置样式。

`outline`添加一个轮廓。

`filled`添加背景色。

`flushed`删除占位符的填充。

此外，我们可以添加插件到输入。

例如，我们可以写:

```
<template>
  <c-box>
    <c-input-group>
      <c-input-left-addon>+234</c-input-left-addon>
      <c-input type="tel" roundedLeft="0" placeholder="phone number" />
    </c-input-group>
  </c-box>
</template><script>
import { CBox, CInputGroup, CInput, CInputLeftAddon } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CInputGroup,
    CInput,
    CInputLeftAddon,
  },
};
</script>
```

使用`c-input-left-addon`组件将插件添加到输入的左侧。

`c-input-right-addon`将输入插件添加到右侧:

```
<template>
  <c-box>
    <c-input-group>
      <c-input rounded="0" placeholder="mysite" />
      <c-input-right-addon>.com</c-input-right-addon>
    </c-input-group>
  </c-box>
</template><script>
import { CBox, CInputGroup, CInput, CInputRightAddon } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CInputGroup,
    CInput,
    CInputRightAddon,
  },
};
</script>
```

# 结论

我们可以用 Chakra UI Vue 添加图像和输入。