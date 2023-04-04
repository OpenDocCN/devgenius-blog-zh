# 使用 Vee-Validate 4 在 Vue 3 应用中进行表单验证——异步和跨字段验证

> 原文：<https://blog.devgenius.io/form-validation-in-a-vue-3-app-with-vee-validate-4-async-and-cross-field-validation-3ea4bb49294c?source=collection_archive---------2----------------------->

![](img/ab09e70bca85ae3b019ecb0bb9456cda.png)

奥尔加·巴斯特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

表单验证是任何应用程序的重要组成部分。

在本文中，我们将了解如何在我们的 Vue 3 应用程序中使用 Vee-Validate 4 进行表单验证。

# 异步验证

我们可以用 Vee-Validate 4 异步验证表单字段。

例如，我们可以写:

```
<template>
  <div>
    <Form @submit="onSubmit">
      <label for="email">Email</label>
      <Field id="email" name="email" :rules="validateEmail" type="email" />
      <ErrorMessage name="email" /> <button type="submit">Submit</button>
    </Form>
  </div>
</template><script>
import { Field, Form, ErrorMessage } from "vee-validate";const mockApiRequest = (value) => {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve(value === "abc@abc.com");
    }, 1000);
  });
};export default {
  name: "App",
  components: {
    Field,
    Form,
    ErrorMessage,
  },
  methods: {
    onSubmit(values) {
      alert(JSON.stringify(values, null, 2));
    },
    async validateEmail(value) {
      const result = await mockApiRequest(value);
      return result ? true : "This email is already taken";
    },
  },
};
</script>
```

我们有`mockApiRequest`函数返回模拟异步验证的承诺。

我们在`validateEmail`方法中使用它。

如果解析为`true`，则该字段有效。

否则无效。

然后我们将该方法传递到`rules` prop 中进行验证。

当我们点击提交时，将会运行验证。

如果有效，那么将运行`onSubmit`方法。

# 跨字段验证

我们可以使用 Vee-Validate 4 添加跨字段验证。

例如，要检查确认密码字段是否与密码字段匹配，我们可以编写:

```
<template>
  <div>
    <Form @submit="onSubmit" :validation-schema="schema">
      <div>
        <label for="password">Password</label>
        <Field id="password" name="password" type="password" />
        <ErrorMessage name="password" />
      </div> <div>
        <label for="passwordConfirmation">Confirm Password </label>
        <Field
          id="passwordConfirmation"
          name="passwordConfirmation"
          type="password"
        />
        <ErrorMessage name="passwordConfirmation" />
      </div> <button type="submit">Submit</button>
    </Form>
  </div>
</template><script>
import { Field, Form, ErrorMessage, defineRule } from "vee-validate";
import * as yup from "yup";defineRule("required", (value) => {
  if (!value) {
    return "This is required";
  } return true;
});defineRule("min", (value, [min]) => {
  if (value && value.length < min) {
    return `Should be at least ${min} characters`;
  } return true;
});defineRule("confirmed", (value, [other]) => {
  if (value !== other) {
    return `Passwords do not match`;
  } return true;
});export default {
  name: "App",
  components: {
    Field,
    Form,
    ErrorMessage,
  },
  data: () => {
    const schema = yup.object().shape({
      password: yup.string().min(5).required(),
      passwordConfirmation: yup
        .string()
        .required()
        .oneOf([yup.ref("password")], "Passwords do not match"),
    }); return {
      schema,
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

我们调用了`defineRule`来定义`required`和`min`规则，以检查字段是否有值被填充，以及它是否具有所需的最小长度。

使用`other`变量定义了`confirmed`规则，以检查其他字段的值。

我们在`data`方法中定义了 Yup 模式。

`passwordConfirmation`规则的验证规则使用`yup.ref("password")`调用`oneOf`来检查它的值是否与`password`字段的值匹配。

然后我们将`schema`传入`validation-schema`道具。

# 结论

我们可以异步验证表单字段，并使用 Vee-Validate 4 在我们的 Vue 3 应用程序中进行跨字段验证。