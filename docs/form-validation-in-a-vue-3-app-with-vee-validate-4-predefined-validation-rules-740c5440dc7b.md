# 使用 Vee-Validate 4 在 Vue 3 应用程序中进行表单验证—预定义的验证规则

> 原文：<https://blog.devgenius.io/form-validation-in-a-vue-3-app-with-vee-validate-4-predefined-validation-rules-740c5440dc7b?source=collection_archive---------2----------------------->

![](img/6c34c99d9a7bb274cccb6d6887b567f8.png)

Nicolas perolja 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

表单验证是任何应用程序的重要组成部分。

在本文中，我们将了解如何在我们的 Vue 3 应用程序中使用 Vee-Validate 4 进行表单验证。

# 使用预定义的规则

我们可以使用来自`@vee-validate/rules`包的预定义验证规则。

为了安装这个包，我们运行:

```
yarn add @vee-validate/rules
```

或者:

```
npm install @vee-validate/rules
```

然后，我们可以通过编写以下代码，从带有`defineRule`函数的包中添加规则:

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
import { Form, Field, defineRule } from "vee-validate";
import { required, email, min } from "@vee-validate/rules";
defineRule("required", required);
defineRule("email", email);
defineRule("min", min);export default {
  components: {
    Form,
    Field,
  },
  data() {
    const schema = {
      email: "required|email",
      password: "required|min:8",
    };
    return {
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

我们从`@vee-validate/rules`包中导入规则，然后将它们传递给`defineRule`函数来使用。

然后我们可以在`schema`验证模式中使用它们。

在模板中，我们显示了来自`errors`对象的错误消息。

如果我们输入了无效的内容，我们会看到一条一般性的错误消息。

此外，我们可以通过编写以下内容从包中导入所有规则:

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
import { Form, Field, defineRule } from "vee-validate";
import * as rules from "@vee-validate/rules";Object.keys(rules).forEach((rule) => {
  defineRule(rule, rules[rule]);
});export default {
  components: {
    Form,
    Field,
  },
  data() {
    const schema = {
      email: "required|email",
      password: "required|min:8",
    };
    return {
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

# 可用规则

`@vee-validate/rules`包提供了许多内置的验证。

# 希腊字母的第一个字母

`alpha`规则验证输入的值只有字母字符。

例如，我们可以这样使用它:

```
<template>
  <Form @submit="submit" v-slot="{ errors }">
    <Field name="field" rules="alpha" />
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

我们也可以用对象格式设置`rules`属性值:

```
<template>
  <Form @submit="submit" v-slot="{ errors }">
    <Field name="field" :rules="validations" />
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
      validations: { alpha: true },
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

添加相同的验证规则。

# 结论

我们可以使用 Vee-Validate 4 将预定义的验证规则添加到我们的 Vue 3 应用程序中。