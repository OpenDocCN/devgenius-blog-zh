# 使用 Chakra UI Vue 进行 UI 开发—选择下拉菜单

> 原文：<https://blog.devgenius.io/ui-development-with-chakra-ui-vue-select-dropdowns-1ee917c880d1?source=collection_archive---------6----------------------->

![](img/70d805b042b7604837fa9443a67f594e.png)

由[Julian bck](https://unsplash.com/@julian_bck?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

Chakra UI Vue 是一个为 Vue.js 制作的 UI 框架，让我们可以将好看的 UI 组件添加到我们的 Vue 应用程序中。

本文将介绍如何开始使用 Chakra UI Vue 进行 UI 开发。

# 挑选

Chakra UI Vue 带有一个样式化的下拉选择组件。

为了补充它，我们写道:

```
<template>
  <c-box>
    <c-select v-model="fruit" placeholder="Select Fruit">
      <option value="apple">apple</option>
      <option value="orange">orange</option>
      <option value="grape">grape</option>
    </c-select>
  </c-box>
</template><script>
import { CBox, CSelect } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CSelect,
  },
  data() {
    return {
      fruit: "",
    };
  },
};
</script>
```

我们将`c-select`组件与`v-model`相加，将选择的值绑定到`fruit`反应属性。

所选选项的`value`属性值将被设置为`fruit`的值。

`placeholder`设置占位符。

我们添加了带有`option`标签的选项。

我们可以设置下拉菜单的样式变量。

为此，我们只需设置`variant`道具:

```
<template>
  <c-box>
    <c-select variant="filled" v-model="fruit" placeholder="Select Fruit">
      <option value="apple">apple</option>
      <option value="orange">orange</option>
      <option value="grape">grape</option>
    </c-select>
  </c-box>
</template><script>
import { CBox, CSelect } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CSelect,
  },
  data() {
    return {
      fruit: "",
    };
  },
};
</script>
```

`filled`将背景填充一种颜色。

我们也可以将其设置为`outline`、`flushed`或`unstyled`。

`outline`在下拉菜单周围添加轮廓。

`flushed`删除轮廓。

`unstyled`表示没有样式添加到下拉列表中。

我们也可以自己设置下拉菜单的样式。

为此，我们写道:

```
<template>
  <c-box>
    <c-select
      backgroundColor="tomato"
      borderColor="tomato"
      color="white"
      v-model="fruit"
      placeholder="Select Fruit"
    >
      <option value="apple">apple</option>
      <option value="orange">orange</option>
      <option value="grape">grape</option>
    </c-select>
  </c-box>
</template><script>
import { CBox, CSelect } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CSelect,
  },
  data() {
    return {
      fruit: "",
    };
  },
};
</script>
```

`backgroundColor`设置背景颜色。`borderColor`设置边框颜色。`color`设置文本颜色。

# 结论

我们可以用 Chakra UI Vue 轻松添加下拉菜单。