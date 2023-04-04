# 使用 Vee-Validate 4 在 Vue 3 应用程序中进行表单验证—表单重置

> 原文：<https://blog.devgenius.io/form-validation-in-a-vue-3-app-with-vee-validate-4-form-reset-1d36680c161e?source=collection_archive---------0----------------------->

![](img/4725bd8641ef579ace26222cef45bc33.png)

照片由[克里斯蒂安·凯恩德尔](https://unsplash.com/@christiankaindl?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

表单验证是任何应用程序的重要组成部分。

在本文中，我们将了解如何在我们的 Vue 3 应用程序中使用 Vee-Validate 4 进行表单验证。

# 处理复位

我们可以通过添加一个属性设置为`reset`的按钮来重置表单。

例如，我们可以写:

```
<template>
  <Form :validation-schema="schema">
    <Field name="email" as="input" />
    <Field name="name" as="input" type="text" />
    <Field name="password" as="input" type="password" /> <button type="Submit">Submit</button>
    <button type="reset">Reset</button>
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

当我们单击重置按钮时，表单字段将被清除。

同样，我们可以从插槽道具中调用`handleReset`方法来做同样的事情:

```
<template>
  <Form v-slot="{ handleReset }" :validation-schema="schema">
    <Field name="email" as="input" />
    <Field name="name" as="input" type="text" />
    <Field name="password" as="input" type="password" /> <button type="button" @click="handleReset">Reset</button>
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

# 提交后重置表单

我们也可以在提交后重置表单。

例如，我们可以写:

```
<template>
  <Form @submit="onSubmit" :validation-schema="schema">
    <Field name="email" as="input" />
    <Field name="name" as="input" type="text" />
    <Field name="password" as="input" type="password" /> <button type="submit">Submit</button>
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
    onSubmit(values, { resetForm }) {
      console.log(values);
      resetForm();
    },
  },
};
</script>
```

我们调用`submit`处理程序中的`resetForm`方法来清除表单字段。

`resetForm`方法可以选择接受一个带有要重置的值的对象。

例如，我们可以写:

```
<template>
  <Form @submit="onSubmit" :validation-schema="schema">
    <Field name="email" as="input" />
    <Field name="name" as="input" type="text" />
    <Field name="password" as="input" type="password" /> <button type="submit">Submit</button>
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
    onSubmit(values, { resetForm }) {
      console.log(values);
      resetForm({
        values: {
          email: "example@example.com",
          password: "",
        },
      });
    },
  },
};
</script>
```

用对象调用`resetForm`。属性有一个对象，其中设置了表单字段名称和相应的值。

我们也可以从表单的 ref:

```
<template>
  <Form @submit="onSubmit" :validation-schema="schema" ref="form">
    <Field name="email" as="input" />
    <Field name="name" as="input" type="text" />
    <Field name="password" as="input" type="password" /><button type="submit">Submit</button>
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
    onSubmit(values, { resetForm }) {
      console.log(values);
      this.$refs.form.resetForm();
    },
  },
};
</script>
```

# 结论

我们可以用 Vee-Validate 4 的 Vue 3 应用程序中的`resetForm`方法重置表单。可以通过多种方式访问它。