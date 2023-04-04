# 使用 Chakra UI Vue 的 UI 开发—数字输入

> 原文：<https://blog.devgenius.io/ui-development-with-chakra-ui-vue-number-input-a6e32a72a048?source=collection_archive---------7----------------------->

![](img/b30cf9e1d2420375ff6caab511de450f.png)

Erik Mclean 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Chakra UI Vue 是一个为 Vue.js 制作的 UI 框架，让我们可以将好看的 UI 组件添加到我们的 Vue 应用程序中。

本文将介绍如何开始使用 Chakra UI Vue 进行 UI 开发。

# 数字输入

我们可以用 Chakra UI Vue 添加一个数字输入。

为此，我们写道:

```
<template>
  <c-box>
    <c-number-input>
      <c-number-input-field />
      <c-number-input-stepper>
        <c-number-increment-stepper />
        <c-number-decrement-stepper />
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
};
</script>
```

`c-number-input`是输入的容器。

`c-number-input-field`是数值输入本身。

`c-number-input-stepper`是增量和减量按钮的容器。

`c-number-increment-stepper`是增量按钮。

`c-number-decrement-stepper`是减量按钮。

我们可以用`v-model`将输入的值绑定到一个反应属性:

```
<template>
  <c-box>
    <c-number-input v-model="value">
      <c-number-input-field />
      <c-number-input-stepper>
        <c-number-increment-stepper />
        <c-number-decrement-stepper />
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

我们可以分别设置`max`和`min`道具允许的最大值和最小值:

```
<template>
  <c-box>
    <c-number-input v-model="value" :max="20" :min="10">
      <c-number-input-field />
      <c-number-input-stepper>
        <c-number-increment-stepper />
        <c-number-decrement-stepper />
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

数字的小数位数可以用`precision`道具设定:

```
<template>
  <c-box>
    <c-number-input :precision="2" v-model="value" :max="20" :min="10">
      <c-number-input-field type="number" />
      <c-number-input-stepper>
        <c-number-increment-stepper />
        <c-number-decrement-stepper />
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

当我们从输入中移除焦点时,`clamp-value-on-blur` prop 将值重置为最接近的最小值或最大值:

```
<template>
  <c-box>
    <c-number-input clamp-value-on-blur v-model="value" :max="20" :min="10">
      <c-number-input-field type="number" />
      <c-number-input-stepper>
        <c-number-increment-stepper />
        <c-number-decrement-stepper />
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

# 结论

我们可以用 Chakra UI Vue 添加一个数字输入。