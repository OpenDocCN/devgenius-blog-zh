# 使用 Vee-Validate 4 在 Vue 3 应用中进行表单验证——全局验证器

> 原文：<https://blog.devgenius.io/form-validation-in-a-vue-3-app-with-vee-validate-4-global-validators-a69b7d4592?source=collection_archive---------0----------------------->

![](img/7a51049792ad94d267cb2876e7172051.png)

Nicolas perolja 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

表单验证是任何应用程序的重要组成部分。

在本文中，我们将了解如何在我们的 Vue 3 应用程序中使用 Vee-Validate 4 进行表单验证。

# 全局验证器

我们可以用`defineRule`方法定义自己的验证规则。

例如，我们可以这样使用它:

```
<template>
  <Form @submit="onSubmit">
    <Field name="name" as="input" rules="required" />
    <ErrorMessage name="name" />
    <br /> <Field name="email" as="input" rules="required|email" />
    <ErrorMessage name="email" />
    <br /> <button>Submit</button>
  </Form>
</template><script>
import { Form, Field, ErrorMessage, defineRule } from "vee-validate";defineRule("required", (value) => {
  if (!value || !value.length) {
    return "This field is required";
  } return true;
});defineRule("email", (value) => {
  if (!value || !value.length) {
    return true;
  } if (!/[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}/.test(value)) {
    return "This field must be a valid email";
  } return true;
});export default {
  components: {
    Form,
    Field,
    ErrorMessage,
  },
  data() {},
  methods: {
    onSubmit(values) {
      alert(JSON.stringify(values, null, 2));
    },
  },
};
</script>
```

我们用规则名调用`defineRule`方法。

第二个参数是我们用来验证表单域的函数。

`value`有输入值。

如果值有效，我们返回`true`。

否则，我们返回一个错误消息字符串。

`Field`组件的`rules`道具有验证规则。

我们将多个规则与管道结合起来。

# 配置全局验证器

我们还可以向规则传递参数来配置它。

例如，我们可以写:

```
<template>
  <Form @submit="onSubmit">
    <Field name="name" as="input" rules="minLength:8" />
    <ErrorMessage name="name" />
    <br /> <button>Submit</button>
  </Form>
</template><script>
import { Form, Field, ErrorMessage, defineRule } from "vee-validate";defineRule("minLength", (value, [limit]) => {
  if (!value || !value.length) {
    return true;
  } if (value.length < limit) {
    return `This field must be at least ${limit} characters`;
  } return true;
});export default {
  components: {
    Form,
    Field,
    ErrorMessage,
  },
  data() {},
  methods: {
    onSubmit(values) {
      alert(JSON.stringify(values, null, 2));
    },
  },
};
</script>
```

我们用从数组中析构的`limit`变量定义了`minLength`规则。

然后我们可以对照它进行验证。

我们通过在`rules`属性的冒号后添加值来设置`limit`变量。

可以通过编写以下内容来组合多个带参数的规则:

```
<template>
  <Form @submit="onSubmit">
    <Field name="name" as="input" rules="required|minLength:8" />
    <ErrorMessage name="name" />
    <br /> <button>Submit</button>
  </Form>
</template><script>
import { Form, Field, ErrorMessage, defineRule } from "vee-validate";defineRule("required", (value) => {
  if (!value || !value.length) {
    return "This field is required";
  }
  return true;
});defineRule("minLength", (value, [limit]) => {
  if (!value || !value.length) {
    return true;
  } if (value.length < limit) {
    return `This field must be at least ${limit} characters`;
  } return true;
});export default {
  components: {
    Form,
    Field,
    ErrorMessage,
  },
  data() {},
  methods: {
    onSubmit(values) {
      alert(JSON.stringify(values, null, 2));
    },
  },
};
</script>
```

# 结论

我们可以为表单字段创建全局验证器，并在我们的 Vue 3 应用程序中使用 Vee-Validate 4 来应用它们。