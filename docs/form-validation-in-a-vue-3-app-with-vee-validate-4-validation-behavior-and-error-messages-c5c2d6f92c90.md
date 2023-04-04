# 使用 Vee-Validate 4 在 Vue 3 应用程序中进行表单验证—验证行为和错误消息

> 原文：<https://blog.devgenius.io/form-validation-in-a-vue-3-app-with-vee-validate-4-validation-behavior-and-error-messages-c5c2d6f92c90?source=collection_archive---------3----------------------->

![](img/0527777e2fa4a03f357bb0c083cfa530.png)

照片由[δξIξδUDIO](https://unsplash.com/@aessieaudio?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

表单验证是任何应用程序的重要组成部分。

在本文中，我们将了解如何在我们的 Vue 3 应用程序中使用 Vee-Validate 4 进行表单验证。

# 验证行为

Vee-Validate 在发出`change`事件或外部更改值时运行验证。

# 自定义验证触发器

当字段被验证时，我们可以改变。

为此，我们使用`configure`函数来改变这种行为:

```
<template>
  <Form @submit="submit" :validation-schema="schema" v-slot="{ errors }">
    <Field name="email" as="input" />
    <span>{{ errors.email }}</span>
    <br />
    <Field name="password" as="input" type="password" />
    <span>{{ errors.password }}</span>
    <br />
    <button>Submit</button>
  </Form>
</template>
<script>
import { configure, Field, Form } from "vee-validate";
import * as yup from "yup";
configure({
  validateOnBlur: true,
  validateOnChange: true,
  validateOnInput: false,
  validateOnModelUpdate: true,
});
export default {
  components: {
    Field,
    Form,
  },
  data() {
    const schema = yup.object().shape({
      email: yup.string().required().email(),
      password: yup.string().required().min(8),
    });
    return {
      schema,
    };
  },
  methods: {
    submit() {},
  },
};
</script>
```

`validateOnBlur`表示验证将在`blur`事件发出时运行。

`validateOnChange`表示当发出`change`事件时将运行验证。

`validateOnInput`表示发出`input`事件时验证运行。

`validateInModelUpdate`表示当模型反应特性更新时验证运行。

# 显示错误消息

我们可以创建自己的字段，并通过使用 slot props 显示错误消息。

例如，我们可以写:

```
<template>
  <Form @submit="submit" :validation-schema="schema">
    <Field :rules="rules" v-slot="{ field, errors, errorMessage }" name="email">
      <input v-bind="field" type="text" />
      <p>{{ errors[0] }}</p>
      <p>{{ errorMessage }}</p>
    </Field>
    <br />
    <button>Submit</button>
  </Form>
</template>
<script>
import { Field, Form } from "vee-validate";
import * as yup from "yup";export default {
  components: {
    Form,
    Field,
  },
  data() {
    const schema = yup.object().shape({
      email: yup.string().required().email(),
    }); return {
      schema,
    };
  },
  methods: {
    submit() {},
  },
};
</script>
```

我们有带有验证模式的`validation-schema` prop。

在`Field`组件中，我们有`input`元素。

我们将`field`属性作为道具传入，以在输入上生成验证触发器。

我们显示了`errors[0]`或`errorMessage`表达式的错误。

此外，我们可以通过迭代`errors`对象来显示字段的所有错误消息:

```
<template>
  <Form v-slot="{ errors }" :validation-schema="schema">
    <template v-if="Object.keys(errors).length">
      <p>Please correct the following errors</p>
      <ul>
        <li v-for="(message, field) in errors" :key="field">
          {{ message }}
        </li>
      </ul>
    </template> <Field name="name" as="input" :rules="rules" />
    <Field name="email" as="input" :rules="rules" />
    <Field name="password" as="input" :rules="rules" />
  </Form>
</template>
<script>
import { Field, Form } from "vee-validate";
import * as yup from "yup";export default {
  components: {
    Field,
    Form,
  },
  data() {
    const schema = yup.object().shape({
      email: yup.string().required().email(),
      password: yup.string().required().min(8),
      name: yup.string().required(),
    });
    return {
      schema,
    };
  },
  methods: {
    submit() {},
  },
};
</script>
```

我们从`errors`对象获取密钥，并用`v-for`指令遍历它们。

`message`是值，我们在`li`中显示它们。

# 结论

我们可以通过 Vee-Validate 4 更改验证运行的时间，并在 Vue 3 应用中显示错误消息。