# 使用 Vuelidate 2 向 Vue 3 应用程序添加表单验证

> 原文：<https://blog.devgenius.io/add-form-validation-to-a-vue-3-app-with-vuelidate-2-972bd0782667?source=collection_archive---------5----------------------->

![](img/845ffcb84ef7f86d02ecece92a0eb11c.png)

由 [Heather Lo](https://unsplash.com/@llhheather?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

Vuelidate 2 是一个为 Vue 3 应用程序开发的流行的表单验证库。

在本文中，我们将了解如何使用 Vuelidate 2 向我们的 Vue 3 应用程序添加表单验证。

# 装置

要安装 Vuelidate 2，我们运行:

```
npm install @vuelidate/core @vuelidate/validators
```

和 NPM 一起。

或者我们可以运行:

```
yarn add @vuelidate/core @vuelidate/validators
```

用纱线。

# 添加表单验证

为了在我们的 Vue 3 应用中添加 Vuelidate 2 的表单验证，我们编写了:

```
<template>
  <div>
    <input v-model="name" />
    <div v-for="error of v$.name.$silentErrors" :key="error.$message">
      <div>{{ error.$message }}</div>
    </div>
  </div>
  <div>
    <input v-model="contact.email" />
    <div v-for="error of v$.contact.email.$silentErrors" :key="error.$message">
      <div>{{ error.$message }}</div>
    </div>
  </div>
</template><script>
import useVuelidate from "@vuelidate/core";
import { required, email } from "@vuelidate/validators";export default {
  name: "App",
  setup() {
    return { v$: useVuelidate() };
  },
  data() {
    return {
      name: "",
      contact: {
        email: "",
      },
    };
  },
  validations() {
    return {
      name: { required },
      contact: {
        email: { required, email },
      },
    };
  },
};
</script>
```

我们导入`useVuelidate`钩子，调用它，并将返回值赋给 thew `v$` reactive 属性。

然后我们添加`name`无功属性，并用`v-model`将其绑定到输入。

然后在`validations`方法中，我们返回一个带有字段名和有效性规则的对象。

我们将在`validations`方法中返回的对象中的每个属性设置为带有我们想要设置的验证规则的属性对象。

然后我们得到每个对象中的`$silentErrors`属性的错误。

`error.$message`有每个字段的错误信息。

# 肮脏的国家

Vuelidate 为每个字段提供了`$dirty`状态，以跟踪是否对其进行了任何操作。

例如，我们可以写:

```
<template>
  <div>
    <input v-model="name" @blur="v$.name.$touch" />
    <template v-if="v$.name.$dirty">
      <div v-for="error of v$.name.$silentErrors" :key="error.$message">
        <div>{{ error.$message }}</div>
      </div>
    </template>
  </div>
  <div>
    <input v-model="contact.email" @blur="v$.contact.email.$touch" />
    <template v-if="v$.contact.email.$dirty">
      <div
        v-for="error of v$.contact.email.$silentErrors"
        :key="error.$message"
      >
        <div>{{ error.$message }}</div>
      </div>
    </template>
  </div>
</template><script>
import useVuelidate from "@vuelidate/core";
import { required, email } from "@vuelidate/validators";export default {
  name: "App",
  setup() {
    return { v$: useVuelidate() };
  },
  data() {
    return {
      name: "",
      contact: {
        email: "",
      },
    };
  },
  validations() {
    return {
      name: { required },
      contact: {
        email: { required, email },
      },
    };
  },
};
</script>
```

在每个输入中，当我们将焦点从输入移开时，我们调用`$touch`方法。

这将使`$dirty`属性变成`true`。

因此，当我们关注输入，然后将焦点从输入移开时，我们可以用它来显示验证错误。

# 结论

我们可以用 Vuelidate 2 将基本的表单验证添加到我们的 Vue 3 应用中。

我们可以显示验证错误，并在显示时进行控制。