# 验证—验证和输入

> 原文：<https://blog.devgenius.io/vuetify-vee-validate-and-input-26d687292bb?source=collection_archive---------0----------------------->

![](img/f188ffd912a88e410fb97c9da81458bc.png)

亚历山大·德比耶夫在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 使用 Vee-Validate 验证表单

我们可以用 Vee-Validate 库来验证 Vuetify 表单。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <ValidationObserver ref="observer" v-slot="{ validate, reset }">
          <form>
            <ValidationProvider v-slot="{ errors }" name="Name" rules="required|max:10">
              <v-text-field
                v-model="name"
                :counter="10"
                :error-messages="errors"
                label="Name"
                required
              ></v-text-field>
            </ValidationProvider>
            <ValidationProvider v-slot="{ errors }" name="email" rules="required|email">
              <v-text-field v-model="email" :error-messages="errors" label="E-mail" required></v-text-field>
            </ValidationProvider>
            <ValidationProvider v-slot="{ errors }" name="select" rules="required">
              <v-select
                v-model="select"
                :items="items"
                :error-messages="errors"
                label="Select"
                data-vv-name="select"
                required
              ></v-select>
            </ValidationProvider>
            <ValidationProvider v-slot="{ errors, valid }" rules="required" name="checkbox">
              <v-checkbox
                v-model="checkbox"
                :error-messages="errors"
                value="1"
                label="Option"
                type="checkbox"
                required
              ></v-checkbox>
            </ValidationProvider> <v-btn class="mr-4" @click="submit">submit</v-btn>
            <v-btn @click="clear">clear</v-btn>
          </form>
        </ValidationObserver>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
import { required, email, max } from "vee-validate/dist/rules";
import {
  extend,
  ValidationObserver,
  ValidationProvider,
  setInteractionMode,
} from "vee-validate";
setInteractionMode("eager");extend("required", {
  ...required,
  message: "{_field_} can not be empty",
});extend("max", {
  ...max,
  message: "{_field_} may not be greater than {length} characters",
});extend("email", {
  ...email,
  message: "Email must be valid",
});export default {
  name: "HelloWorld",
  components: {
    ValidationProvider,
    ValidationObserver,
  },
  data: () => ({
    name: "",
    email: "",
    select: null,
    items: ["Item 1", "Item 2", "Item 3", "Item 4"],
    checkbox: null,
  }), methods: {
    submit() {
      this.$refs.observer.validate();
    },
    clear() {
      this.name = "";
      this.email = "";
      this.select = null;
      this.checkbox = null;
      this.$refs.observer.reset();
    },
  },
};
</script>
```

我们用`ValidationObserver`组件包装表单，这样我们就可以使用验证特性。

此外，我们用`ValidationProvider`来包装我们的`v-text-field`，让我们用 Vee-Validate 为每个字段添加验证。

`rules`设定规则。

规则是用`extends`函数定义的。

我们将一个对象传入带有该对象的第二个参数，用错误消息定义我们的验证规则。

`message`有错误信息。

`required`、`max`和`email`是具有验证规则的对象。

在`components`属性中，我们注册了组件。

`reset`和`validate`方法让我们分别重置表单验证和 validate。

# 输入

我们可以用`v-input`组件添加表单输入。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-text-field color="success" loading disabled></v-text-field>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
};
</script>
```

用`loading`道具添加一个输入，让它显示一个进度条，表明它正在加载。

![](img/e9206bea64a1071fd696ae2af2a79c17.png)

照片由 [Jonas Leupe](https://unsplash.com/@jonasleupe?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以用 VeeValidate 将表单验证添加到 Vuetify 表单中。

此外，我们可以用`v-input`组件添加一个输入。