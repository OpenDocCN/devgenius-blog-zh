# 使用 Vee-Validate 4 在 Vue 3 应用程序中进行表单验证—文件和整数

> 原文：<https://blog.devgenius.io/form-validation-in-a-vue-3-app-with-vee-validate-4-files-and-integers-d0ac62253fb?source=collection_archive---------5----------------------->

![](img/d23730b4cebdfbdbbd8b6ecd34f78997.png)

miosz klin owski 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

表单验证是任何应用程序的重要组成部分。

在本文中，我们将了解如何在我们的 Vue 3 应用程序中使用 Vee-Validate 4 进行表单验证。

# 外面的（exterior 的简写）

`ext` 验证规则检查所选文件是否具有列出的扩展名之一。

例如，我们可以写:

```
<template>
  <Form @submit="onSubmit" v-slot="{ errors }">
    <Field name="field" type="file" rules="ext:jpg,png" />
    <span>{{ errors.field }}</span>
  </Form>
</template>
<script>
import { Form, Field, defineRule } from "vee-validate";
import * as rules from "[@vee](http://twitter.com/vee)-validate/rules";Object.keys(rules).forEach((rule) => {
  defineRule(rule, rules[rule]);
});export default {
  components: {
    Form,
    Field,
  },
  methods: {
    onSubmit(values) {
      alert(JSON.stringify(values, null, 2));
    },
  },
};
</script>
```

那么所选择的文件必须具有`jpg`或`png`扩展名。

我们可以在列表中添加任意数量的扩展名。

此外，我们可以写:

```
<template>
  <Form @submit="onSubmit" v-slot="{ errors }">
    <Field name="field" type="file" :rules="validations" />
    <span>{{ errors.field }}</span>
  </Form>
</template>
<script>
import { Form, Field, defineRule } from "vee-validate";
import * as rules from "@vee-validate/rules";Object.keys(rules).forEach((rule) => {
  defineRule(rule, rules[rule]);
});export default {
  components: {
    Form,
    Field,
  },
  data() {
    return {
      validations: { ext: ["jpg", "png"] },
    };
  },
  methods: {
    onSubmit(values) {
      alert(JSON.stringify(values, null, 2));
    },
  },
};
</script>
```

我们在一个数组中有扩展。

# 图像

`image`规则允许验证所选文件具有以`image/`开头的 MIME 类型。

例如，我们可以写:

```
<template>
  <Form @submit="onSubmit" v-slot="{ errors }">
    <Field name="field" type="file" rules="image" />
    <span>{{ errors.field }}</span>
  </Form>
</template>
<script>
import { Form, Field, defineRule } from "vee-validate";
import * as rules from "@vee-validate/rules";Object.keys(rules).forEach((rule) => {
  defineRule(rule, rules[rule]);
});export default {
  components: {
    Form,
    Field,
  },
  methods: {
    onSubmit(values) {
      alert(JSON.stringify(values, null, 2));
    },
  },
};
</script>
```

我们将`rules`属性设置为`'image'`来限制选择的文件为图像。

此外，我们可以写:

```
<template>
  <Form @submit="onSubmit" v-slot="{ errors }">
    <Field name="field" type="file" :rules="validations" />
    <span>{{ errors.field }}</span>
  </Form>
</template><script>
import { Form, Field, defineRule } from "vee-validate";
import * as rules from "@vee-validate/rules";Object.keys(rules).forEach((rule) => {
  defineRule(rule, rules[rule]);
});export default {
  components: {
    Form,
    Field,
  },
  data() {
    return {
      validations: { image: true },
    };
  },
  methods: {
    onSubmit(values) {
      alert(JSON.stringify(values, null, 2));
    },
  },
};
</script>
```

做同样的事情。

# 整数

`integer`规则让我们验证输入的值必须是整数。

例如，我们可以写:

```
<template>
  <Form @submit="onSubmit" v-slot="{ errors }">
    <Field name="field" rules="integer" />
    <span>{{ errors.field }}</span>
  </Form>
</template><script>
import { Form, Field, defineRule } from "vee-validate";
import * as rules from "@vee-validate/rules";Object.keys(rules).forEach((rule) => {
  defineRule(rule, rules[rule]);
});export default {
  components: {
    Form,
    Field,
  },
  methods: {
    onSubmit(values) {
      alert(JSON.stringify(values, null, 2));
    },
  },
};
</script>
```

将`integer`规则添加到`rules`道具中。

此外，我们可以写:

```
<template>
  <Form @submit="onSubmit" v-slot="{ errors }">
    <Field name="field" :rules="validations" />
    <span>{{ errors.field }}</span>
  </Form>
</template><script>
import { Form, Field, defineRule } from "vee-validate";
import * as rules from "@vee-validate/rules";Object.keys(rules).forEach((rule) => {
  defineRule(rule, rules[rule]);
});export default {
  components: {
    Form,
    Field,
  },
  data() {
    return {
      validations: { integer: true },
    };
  },
  methods: {
    onSubmit(values) {
      alert(JSON.stringify(values, null, 2));
    },
  },
};
</script>
```

做同样的事情。

# 结论

我们可以在 Vue 3 应用程序中使用 Vee-Validate 4 来验证所选文件是否具有给定的扩展名，或者它是否是一个图像。

此外，我们可以验证用户输入的是一个整数。