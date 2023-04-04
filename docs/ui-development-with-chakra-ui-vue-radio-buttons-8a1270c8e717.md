# 使用 Chakra UI Vue 的 UI 开发—单选按钮

> 原文：<https://blog.devgenius.io/ui-development-with-chakra-ui-vue-radio-buttons-8a1270c8e717?source=collection_archive---------9----------------------->

![](img/eb311b20d49ae12f580d23a72f2a9ee3.png)

[流苏猫](https://unsplash.com/@nittygritty_photo?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

Chakra UI Vue 是一个为 Vue.js 制作的 UI 框架，让我们可以将好看的 UI 组件添加到我们的 Vue 应用程序中。

本文将介绍如何开始使用 Chakra UI Vue 进行 UI 开发。

# 单选按钮

我们可以添加带有`c-radio-group`和`c-radio`组件的单选按钮。

例如，我们可以写:

```
<template>
  <c-box>
    <c-radio-group v-model="selectedFruit">
      <c-radio value="apple">apple</c-radio>
      <c-radio value="orange">orange </c-radio>
      <c-radio value="grape">grape</c-radio>
    </c-radio-group>
    <c-text> Favourite fruit: {{ selectedFruit }} </c-text>
  </c-box>
</template><script>
import { CBox, CRadio, CRadioGroup, CText } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CRadio,
    CRadioGroup,
    CText,
  },
  data() {
    return {
      selectedFruit: "apple",
    };
  },
};
</script>
```

我们用`v-model`将选择的值绑定到一个反应属性。

`c-radio`有单选按钮。

我们可以用`variant-color`道具改变单选按钮的颜色:

```
<template>
  <c-box>
    <c-radio-group v-model="selectedFruit">
      <c-radio variant-color="red" value="apple">apple</c-radio>
      <c-radio variant-color="orange" value="orange">orange </c-radio>
      <c-radio value="grape">grape</c-radio>
    </c-radio-group>
    <c-text> Favourite fruit: {{ selectedFruit }} </c-text>
  </c-box>
</template><script>
import { CBox, CRadio, CRadioGroup, CText } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CRadio,
    CRadioGroup,
    CText,
  },
  data() {
    return {
      selectedFruit: "apple",
    };
  },
};
</script>
```

`size`道具让我们设定尺寸:

```
<template>
  <c-box>
    <c-radio-group v-model="selectedFruit">
      <c-radio value="apple">apple</c-radio>
      <c-radio value="orange">orange </c-radio>
      <c-radio value="grape" size="lg">grape</c-radio>
    </c-radio-group>
    <c-text> Favourite fruit: {{ selectedFruit }} </c-text>
  </c-box>
</template><script>
import { CBox, CRadio, CRadioGroup, CText } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CRadio,
    CRadioGroup,
    CText,
  },
  data() {
    return {
      selectedFruit: "apple",
    };
  },
};
</script>
```

`lg`放大。我们也可以将它设置为`sm`以使其变小，或者设置为`md`以获得中等大小。

我们可以用`is-disabled`道具禁用一个按钮:

```
<template>
  <c-box>
    <c-radio-group v-model="selectedFruit">
      <c-radio value="apple">apple</c-radio>
      <c-radio value="orange">orange </c-radio>
      <c-radio value="grape" is-disabled>>grape</c-radio>
    </c-radio-group>
    <c-text> Favourite fruit: {{ selectedFruit }} </c-text>
  </c-box>
</template><script>
import { CBox, CRadio, CRadioGroup, CText } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CRadio,
    CRadioGroup,
    CText,
  },
  data() {
    return {
      selectedFruit: "apple",
    };
  },
};
</script>
```

我们可以让单选按钮与`is-inline`道具并排显示:

```
<template>
  <c-box>
    <c-radio-group v-model="selectedFruit" is-inline>
      <c-radio value="apple">apple</c-radio>
      <c-radio value="orange">orange </c-radio>
      <c-radio value="grape">grape</c-radio>
    </c-radio-group>
    <c-text> Favourite fruit: {{ selectedFruit }} </c-text>
  </c-box>
</template><script>
import { CBox, CRadio, CRadioGroup, CText } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CRadio,
    CRadioGroup,
    CText,
  },
  data() {
    return {
      selectedFruit: "apple",
    };
  },
};
</script>
```

# 结论

我们可以用 Chakra UI Vue 添加单选按钮。