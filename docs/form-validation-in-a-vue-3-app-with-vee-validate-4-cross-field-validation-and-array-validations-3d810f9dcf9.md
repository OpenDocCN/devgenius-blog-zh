# 使用 Vee-Validate 4 在 Vue 3 应用中进行表单验证——跨字段验证和数组验证

> 原文：<https://blog.devgenius.io/form-validation-in-a-vue-3-app-with-vee-validate-4-cross-field-validation-and-array-validations-3d810f9dcf9?source=collection_archive---------5----------------------->

![](img/67760b2c539f799b5f8012321fdcefcd.png)

Joshua J. Cotten 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

表单验证是任何应用程序的重要组成部分。

在本文中，我们将了解如何在我们的 Vue 3 应用程序中使用 Vee-Validate 4 进行表单验证。

# 使用定义规则的跨字段验证

我们可以在没有 Yup 库的情况下添加跨字段验证。

为此，我们写道:

```
<template>
  <div>
    <Form @submit="onSubmit">
      <div>
        <label for="password">Password</label>
        <Field name="password" type="password" rules="required|min:5" />
        <ErrorMessage name="password" />
      </div> <div>
        <label for="passwordConfirmation">Confirm Password </label>
        <Field
          name="passwordConfirmation"
          type="password"
          rules="required|confirmed:@password"
        />
        <ErrorMessage name="passwordConfirmation" />
      </div>
      <button type="submit">Submit</button>
    </Form>
  </div>
</template><script>
import { Field, Form, ErrorMessage, defineRule } from "vee-validate";defineRule("required", (value) => {
  if (!value) {
    return "This is required";
  }
  return true;
});defineRule("min", (value, [min]) => {
  if (value && value.length < min) {
    return `Should be at least ${min} characters`;
  }
  return true;
});defineRule("confirmed", (value, [other]) => {
  if (value !== other) {
    return `Passwords do not match`;
  }
  return true;
});export default {
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
  },
};
</script>
```

我们调用了`defineRule`来定义`required`规则，以检查字段是否被填充。

`min`检查字段是否有最小字符数。

`confirmed`检查另一个字段的值是否与当前字段的值匹配。

`value`有田地的价值。并且`other`具有另一个字段的值。

# 数组字段

我们可以验证从数组中呈现的字段。

为此，我们写道:

```
<template>
  <div>
    <Form @submit="onSubmit" :validation-schema="schema">
      <fieldset v-for="(user, idx) in users" :key="user.id">
        <legend>User {{ idx }}</legend>
        <label :for="`name_${idx}`">Name</label>
        <Field :id="`name_${idx}`" :name="`users[${idx}].name`" />
        <ErrorMessage :name="`users[${idx}].name`" as="p" /> <label :for="`email_${idx}`">Email</label>
        <Field
          :id="`email_${idx}`"
          :name="`users[${idx}].email`"
          type="email"
        />
        <ErrorMessage :name="`users[${idx}].email`" as="p" />
        <button type="button" @click="remove(user)">X</button>
      </fieldset> <button type="button" @click="add">Add User +</button>
      <button type="submit">Submit</button>
    </Form>
  </div>
</template><script>
import { Field, Form, ErrorMessage } from "vee-validate";
import * as yup from "yup";export default {
  name: "App",
  components: {
    Field,
    Form,
    ErrorMessage,
  },
  data: () => {
    const schema = yup.object().shape({
      users: yup
        .array()
        .of(
          yup.object().shape({
            name: yup.string().required().label("Name"),
            email: yup.string().email().required().label("Email"),
          })
        )
        .strict(),
    }); return {
      schema,
      users: [
        {
          id: Date.now(),
        },
      ],
    };
  },
  methods: {
    onSubmit(values) {
      alert(JSON.stringify(values, null, 2));
    },
    add() {
      this.users.push({
        id: Date.now(),
      });
    },
    remove(item) {
      const index = this.users.indexOf(item);
      if (index === -1) {
        return;
      } this.users.splice(index, 1);
    },
  },
};
</script>
```

我们用 Yup 创建了`schema`对象来调用`yup.array`来验证数组。

然后我们调用`of`来指定内部对象的验证模式。

每个字段的`name`属性应该是一个字符串，采用数组路径的形式。

现在，当我们在任何条目中输入无效内容时，我们会看到显示错误。

# 结论

我们可以使用 Vee-Validate 4 在我们的 Vue 3 应用中进行跨字段验证和数组字段验证。