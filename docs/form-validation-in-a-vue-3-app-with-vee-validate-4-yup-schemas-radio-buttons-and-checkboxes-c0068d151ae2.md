# 使用 Vee-Validate 4 在 Vue 3 应用程序中进行表单验证——包括模式、单选按钮和复选框

> 原文：<https://blog.devgenius.io/form-validation-in-a-vue-3-app-with-vee-validate-4-yup-schemas-radio-buttons-and-checkboxes-c0068d151ae2?source=collection_archive---------3----------------------->

![](img/ff702b55257f93ce7e9bca341553c25d.png)

由[苏珊·霍尔特·辛普森](https://unsplash.com/@shs521?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

表单验证是任何应用程序的重要组成部分。

在本文中，我们将了解如何在我们的 Vue 3 应用程序中使用 Vee-Validate 4 进行表单验证。

# 是数据中的模式

让我们的验证模式反应是一个不必要的开销，我们可以消除它。

要做到这一点，我们可以用`setUp`方法添加模式，或者用我们的模式调用`markRaw`,使其成为非反应性的。

例如，我们可以写:

```
<template>
  <Form v-slot="{ errors }" :validation-schema="schema">
    <Field as="input" name="email" />
    <span>{{ errors.email }}</span>
    <br /> <Field as="input" name="password" type="password" />
    <span>{{ errors.password }}</span>
    <br /> <button>Submit</button>
  </Form>
</template><script>
import { Form, Field } from "vee-validate";
import * as yup from "yup";export default {
  components: {
    Form,
    Field,
  },
  setup() {
    const schema = yup.object().shape({
      email: yup.string().required().email(),
      password: yup.string().required().min(8),
    }); return {
      schema,
    };
  },
};
</script>
```

在`setUp`方法中创建我们的模式。

我们只返回由 Yup 返回的对象，因此`schema`属性不会是被动的。

为了给`markRaw`打电话，我们写:

```
<template>
  <Form v-slot="{ errors }" :validation-schema="schema">
    <Field as="input" name="email" />
    <span>{{ errors.email }}</span>
    <br /> <Field as="input" name="password" type="password" />
    <span>{{ errors.password }}</span>
    <br /> <button>Submit</button>
  </Form>
</template><script>
import { markRaw } from "vue";
import { Form, Field } from "vee-validate";
import * as yup from "yup";export default {
  components: {
    Form,
    Field,
  },
  data() {
    const schema = markRaw(
      yup.object().shape({
        email: yup.string().required().email(),
        password: yup.string().required().min(8),
      })
    ); return {
      schema,
    };
  },
};
</script>
```

我们用我们的 Yup 模式调用`markRaw`,使其成为非反应性的。

# 单选按钮

我们可以使用 Vee-Validate 4 来验证单选按钮。

例如，我们可以写:

```
<template>
  <Form :validation-schema="schema" @submit="onSubmit">
    <Field name="drink" type="radio" value=""></Field> None
    <Field name="drink" type="radio" value="Tea"></Field> Tea
    <Field name="drink" type="radio" value="Coffee"></Field> Coffee
    <ErrorMessage name="drink" />
  </Form>
</template><script>
import { Form, Field, ErrorMessage } from "vee-validate";export default {
  components: {
    Form,
    Field,
    ErrorMessage,
  },
  data() {
    return {
      schema: {
        drink: (value) => {
          if (value) {
            return true;
          } return "You must choose a drink";
        },
      },
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

我们将`type`设置为`radio`的`Field`组件用于创建单选按钮。

然后在`data`方法中，我们用`drink`方法返回`schema`反应属性来验证我们的单选按钮。

`value`具有由`value`属性指定的检查值。

如果选择有效，我们返回`true`，否则返回错误消息。

然后我们显示`ErrorMessage`组件来显示错误消息。

# 复选框

我们可以使用 Vee-Validate 4 添加复选框验证。

例如，我们可以写:

```
<template>
  <Form :validation-schema="schema" @submit="onSubmit">
    <Field name="drink" type="checkbox" value="Milk"></Field> Milk
    <Field name="drink" type="checkbox" value="Tea"></Field> Tea
    <Field name="drink" type="checkbox" value="Coffee"></Field> Coffee
    <ErrorMessage name="drink" />
  </Form>
</template><script>
import { Form, Field, ErrorMessage } from "vee-validate";export default {
  components: {
    Form,
    Field,
    ErrorMessage,
  },
  data() {
    return {
      schema: {
        drink: (value) => {
          if (Array.isArray(value) && value.length) {
            return true;
          }
          return "You must choose a drink";
        },
      },
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

为复选框添加验证。我们将`type`属性设置为`checkbox`来呈现一个复选框。

如果选中了任何复选框，`drink`方法将所有选中的值放在一个数组中。

如果选择有效，我们返回`true`，否则返回错误消息。

然后我们显示`ErrorMessage`组件来显示错误消息。

# 结论

我们可以在 Vue 3 应用程序中使用 Vee-Validate 4 将 Yup 模式设置为非反应模式，以减少开销并验证单选按钮和复选框。