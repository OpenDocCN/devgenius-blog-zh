# 使用 Vee-Validate 4 在 Vue 3 应用程序中开始表单验证

> 原文：<https://blog.devgenius.io/getting-started-with-form-validation-in-a-vue-3-app-with-vee-validate-4-30bfde1dbb9b?source=collection_archive---------3----------------------->

![](img/7e4056b7e4c25bbb72660db043b9357a.png)

[摄于](https://unsplash.com/@morais?utm_source=medium&utm_medium=referral) [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的

表单验证是任何应用程序的重要组成部分。

在本文中，我们将了解如何在我们的 Vue 3 应用程序中使用 Vee-Validate 4 进行表单验证。

# 脚本标记入门

我们可以从脚本标记开始。

我们使用`v-form`组件作为表单容器。

`v-field`组件是我们的输入字段。

`error-message`组件是我们的错误消息。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
    <script src="https://unpkg.com/vee-validate@next"></script>
  </head>
  <body>
    <div id="app">
      <v-form @submit="onSubmit">
        <v-field
          name="name"
          as="input"
          type="text"
          placeholder="What's your name?"
          :rules="isRequired"
        ></v-field>
        <br />
        <error-message name="name"></error-message>
        <br />
        <button>Submit</button>
      </v-form>
    </div>
    <script>
      const app = Vue.createApp({
        components: {
          VForm: VeeValidate.Form,
          VField: VeeValidate.Field,
          ErrorMessage: VeeValidate.ErrorMessage
        },
        methods: {
          isRequired(value) {
            if (!value) {
              return "this field is required";
            }
            return true;
          },
          onSubmit(values) {
            alert(JSON.stringify(values, null, 2));
          }
        }
      });
      app.mount("#app");
    </script>
  </body>
</html>
```

首先，我们添加脚本标记:

```
<script src="https://unpkg.com/vee-validate@next"></script>
```

然后我们注册`VForm`、`VField`和`ErrorMessage`组件，这样我们就可以在模板中使用它们。

我们有`isRequired`方法，通过将它传递到`rules`属性中，我们将它用于表单的规则。

如果表单字段有效，它返回`true`。

否则，它会返回我们想要显示的错误消息。

`value`具有我们输入的值。

`v-field`组件具有`as`属性，该属性被设置为`input`以将其呈现为输入框。

`error-message`的`name`将匹配我们正在验证的字段的`name`属性值。

# 开始使用 NPM 软件包

我们可以通过运行以下命令来安装 Vee-Validate NPM 软件包:

```
yarn add vee-validate@next
```

或者

```
npm i vee-validate@next --save
```

然后我们可以通过写来使用它:

```
<template>
  <Form v-slot="{ errors }">
    <Field name="field" as="input" :rules="isRequired" />
    <br />
    <span>{{ errors.field }}</span>
  </Form>
</template><script>
import { Field, Form } from "vee-validate";export default {
  components: {
    Field,
    Form,
  },
  methods: {
    isRequired(value) {
      return value ? true : "This field is required";
    },
  },
};
</script>
```

我们注册了`Field`和`Form`组件，这样我们就可以在模板中使用它们。

`Form`是表单包装器。它具有来自带有错误消息的槽道具的`errors`属性。

这个`Field`组件类似于我们在前面的例子中的`v-field`组件。

`errors.field`有`'field'`字段的验证表单验证错误信息。

`isRequired`函数的验证逻辑与上例相同。

如果`value`有效，则返回`true`，否则返回错误验证消息。

# 结论

我们可以使用 Vee-Validate 4 将简单的表单验证添加到 Vue 3 应用程序中。