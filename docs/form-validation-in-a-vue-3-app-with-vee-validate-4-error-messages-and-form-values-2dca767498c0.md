# 使用 Vee-Validate 4 在 Vue 3 应用程序中进行表单验证—错误消息和表单值

> 原文：<https://blog.devgenius.io/form-validation-in-a-vue-3-app-with-vee-validate-4-error-messages-and-form-values-2dca767498c0?source=collection_archive---------3----------------------->

![](img/fc53f58cba958d65fd009454971136c7.png)

[Julia Craice](https://unsplash.com/@jcraice?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

表单验证是任何应用程序的重要组成部分。

在本文中，我们将了解如何在我们的 Vue 3 应用程序中使用 Vee-Validate 4 进行表单验证。

# 错误消息组件

我们可以使用`ErrorMessage`组件来显示错误消息。

例如，我们可以写:

```
<template>
  <Form>
    <Field name="field" as="input" :rules="rules" />
    <ErrorMessage name="field" />
  </Form>
</template><script>
import { Field, Form, ErrorMessage } from "vee-validate";
import * as yup from "yup";export default {
  components: {
    Field,
    Form,
    ErrorMessage,
  },
  data() {
    const rules = yup.string().required(); return {
      rules,
    };
  },
};
</script>
```

我们添加了`ErrorMessage`组件，并将`name`属性设置为`Field`组件的`name`属性的值，以显示该字段的验证错误消息。

# 带“是”的自定义标签

我们可以更改字段的标签。

例如，我们可以写:

```
<template>
  <Form :validation-schema="schema">
    <Field name="emailAddr" as="input" />
    <ErrorMessage name="emailAddr" /> <Field name="accPassword" as="input" />
    <ErrorMessage name="accPassword" />
  </Form>
</template><script>
import { Field, Form, ErrorMessage } from "vee-validate";
import * as yup from "yup";export default {
  components: {
    Field,
    Form,
    ErrorMessage,
  },
  data() {
    const schema = yup.object().shape({
      emailAddr: yup.string().email().required().label("Email Address"),
      accPassword: yup.string().min(5).required().label("Your Password"),
    });
    return {
      schema,
    };
  },
};
</script>
```

我们调用`label`方法来改变字段标签。

当我们看到验证错误消息时，我们会看到字段标签显示在错误消息中。

# 表单值

我们可以从`Form`组件的 slot props 中获取表单值。

例如，我们可以写:

```
<template>
  <Form v-slot="{ values }" :validation-schema="schema">
    <Field name="email" as="input" />
    <Field name="name" as="input" type="email" />
    <Field name="password" as="input" type="password" /> <pre>
      {{ values }}
    </pre>
  </Form>
</template><script>
import { Form, Field } from "vee-validate";
import * as yup from "yup";export default {
  components: {
    Form,
    Field,
  },
  data() {
    const schema = yup.object().shape({
      email: yup.string().required().email(),
      name: yup.string().required(),
      password: yup.string().required().min(8),
    }); return {
      schema,
    };
  },
};
</script>
```

slot props 中的`values`属性具有表单值。

所以当我们输入时，我们会看到显示的值。

要访问组件对象中的表单值，我们可以从提交处理程序的参数中访问它们:

```
<template>
  <Form @submit="onSubmit" :validation-schema="schema">
    <Field name="email" as="input" />
    <Field name="name" as="input" type="email" />
    <Field name="password" as="input" type="password" />
    <input type="submit" />
  </Form>
</template><script>
import { Form, Field } from "vee-validate";
import * as yup from "yup";export default {
  components: {
    Form,
    Field,
  },
  data() {
    const schema = yup.object().shape({
      email: yup.string().required().email(),
      name: yup.string().required(),
      password: yup.string().required().min(8),
    }); return {
      schema,
    };
  },
  methods: {
    onSubmit(values) {
      console.log(values);
    },
  },
};
</script>
```

我们添加了`submit`事件处理程序。

然后在提交事件处理程序中，我们可以从`values`参数中访问表单值。

当我们单击 submit 并且输入的值都有效时，就会运行`onSubmit`方法。

# 结论

我们可以用`ErrorMessage`组件显示错误消息。

此外，我们可以显示来自 slot props 的表单值，并在提交处理程序中获取表单值。