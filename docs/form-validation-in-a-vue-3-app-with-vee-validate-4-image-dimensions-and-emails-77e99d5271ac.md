# 使用 Vee-Validate 4 在 Vue 3 应用程序中进行表单验证—图像尺寸和电子邮件

> 原文：<https://blog.devgenius.io/form-validation-in-a-vue-3-app-with-vee-validate-4-image-dimensions-and-emails-77e99d5271ac?source=collection_archive---------2----------------------->

![](img/51a264d69f3b0cfbdf7d2bdab29b1a1e.png)

照片由 [Niklas Schweinzer](https://unsplash.com/@nelebki?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

表单验证是任何应用程序的重要组成部分。

在本文中，我们将了解如何在我们的 Vue 3 应用程序中使用 Vee-Validate 4 进行表单验证。

# 规模

我们可以使用`dimensions`规则来验证所选图像文件具有给定的宽度和高度。

例如，我们可以这样使用它:

```
<template>
  <Form @submit="submit" v-slot="{ errors }">
    <Field name="field" type="file" rules="dimensions:120,120" />
    <span>{{ errors.field }}</span>
  </Form>
</template>
<script>
import { Form, Field, defineRule } from "vee-validate";
import * as rules from "@vee-validate/rules";
Object.keys(rules).forEach((rule) => {
  defineRule(rule, rules[rule]);
});
export default {
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

我们通过将宽度和高度放在`rules`道具中的`dimensions:`之后来设置它们。

此外，我们可以写:

```
<template>
  <Form @submit="submit" v-slot="{ errors }">
    <Field name="field" type="file" :rules="validations" />
    <span>{{ errors.field }}</span>
  </Form>
</template>
<script>
import { Form, Field, defineRule } from "vee-validate";
import * as rules from "@vee-validate/rules";
Object.keys(rules).forEach((rule) => {
  defineRule(rule, rules[rule]);
});
export default {
  components: {
    Form,
    Field,
  },
  data() {
    return {
      validations: { dimensions: [120, 120] },
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

添加带有对象的`dimensions`规则。

此外，我们可以写:

```
<template>
  <Form @submit="submit" v-slot="{ errors }">
    <Field name="field" type="file" :rules="validations" />
    <span>{{ errors.field }}</span>
  </Form>
</template>
<script>
import { Form, Field, defineRule } from "vee-validate";
import * as rules from "@vee-validate/rules";
Object.keys(rules).forEach((rule) => {
  defineRule(rule, rules[rule]);
});
export default {
  components: {
    Form,
    Field,
  },
  data() {
    return {
      validations: { dimensions: { width: 120, height: 120 } },
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

# 电子邮件

`email`规则验证输入的值是有效的电子邮件。

例如，我们可以写:

```
<template>
  <Form @submit="submit" v-slot="{ errors }">
    <Field name="field" rules="email" />
    <span>{{ errors.field }}</span>
  </Form>
</template>
<script>
import { Form, Field, defineRule } from "vee-validate";
import * as rules from "@vee-validate/rules";Object.keys(rules).forEach((rule) => {
  defineRule(rule, rules[rule]);
});
export default {
  components: {
    Form,
    Field,
  },
  methods: {
    submit(values) {
      alert(JSON.stringify(values, null, 2));
    },
  },
};
</script>
```

我们将`rules`属性设置为`'email'`来添加规则。

此外，我们可以写:

```
<template>
  <Form @submit="onSubmit" v-slot="{ errors }">
    <Field name="field" :rules="validations" />
    <span>{{ errors.field }}</span>
  </Form>
</template>
<script>
import { Form, Field, defineRule } from "vee-validate";
import * as rules from "@vee-validate/rules";Object.keys(rules).forEach((rule) => {
  defineRule(rule, rules[rule]);
});
export default {
  components: {
    Form,
    Field,
  },
  data() {
    return {
      validations: { email: true },
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

添加带有对象的`email`规则。

# 结论

我们可以使用 Vee-Validate 4 来验证图像文件尺寸和电子邮件。