# 使用 Vee-Validate 4 在 Vue 3 应用程序中进行表单验证—验证自定义输入

> 原文：<https://blog.devgenius.io/form-validation-in-a-vue-3-app-with-vee-validate-4-validating-custom-inputs-3764e08791bd?source=collection_archive---------3----------------------->

![](img/c7d7ba36d07fa7b64cc640bb34070f82.png)

由 [Lars Millberg](https://unsplash.com/@millberg?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

表单验证是任何应用程序的重要组成部分。

在本文中，我们将了解如何在我们的 Vue 3 应用程序中使用 Vee-Validate 4 进行表单验证。

# 验证自定义输入

我们可以使用 Vee-Validate 4 来验证定制的输入组件。

例如，我们可以写:

`components/TextInput.vue`

```
<template>
  <div
    class="TextInput"
    :class="{ 'has-error': !!errorMessage, success: meta.valid }"
  >
    <label :for="name">{{ label }}</label>
    <input
      :name="name"
      :id="name"
      :type="type"
      :value="inputValue"
      @input="handleChange"
      @blur="handleBlur"
    /> <p class="help-message" v-show="errorMessage || meta.valid">
      {{ errorMessage || successMessage }}
    </p>
  </div>
</template><script>
import { useField } from "vee-validate";export default {
  props: {
    type: {
      type: String,
      default: "text",
    },
    value: {
      type: String,
      default: "",
    },
    name: {
      type: String,
      required: true,
    },
    label: {
      type: String,
      required: true,
    },
    successMessage: {
      type: String,
      default: "",
    },
  },
  setup(props) {
    const {
      value: inputValue,
      errorMessage,
      handleBlur,
      handleChange,
      meta,
    } = useField(props.name, undefined, {
      initialValue: props.value,
    }); return {
      handleChange,
      handleBlur,
      errorMessage,
      inputValue,
      meta,
    };
  },
};
</script>
```

`App.vue`

```
<template>
  <Form :validation-schema="schema" @submit="onSubmit">
    <TextInput
      name="name"
      type="text"
      label="Full Name"
      placeholder="Your Name"
      success-message="Nice to meet you!"
    />
  </Form>
</template><script>
import { Form } from "vee-validate";
import TextInput from "@/components/TextInput.vue";
import * as yup from 'yup'export default {
  components: {
    Form,
    TextInput,
  },
  data() {
    const schema = yup.object().shape({
      name: yup.string().required(),
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

我们创建了需要一些道具的`TextInput`组件。

`name`是我们组件的`name`属性值。

`type`有`type`属性值。

`label`有标签的文本。

`placeholder`已经输入占位符值。

`success-message`有验证成功消息。

然后，我们在`data`方法中定义了模式，并将其传递给`validation-schema`属性。

在`TextInput.vue`中，我们拿到道具，放在我们想要的地方。

在`setup`方法中，我们调用`useField`函数为我们的字段添加验证。

我们将`name`属性值作为第一个参数传入。

然后我们用第三个参数中的`props.value`值设置初始值。

它返回一个带有`value`属性的对象，我们可以用它来设置`value`属性。

`handleChange`传入`input`道具设置输入值。

`handleBlur`用于处理模糊事件。

`meta` has 是一个带有有效性状态字段的对象。

`errorMessage`有表单域错误信息。

现在，当我们在字段中键入一些值时，我们应该会看到不同的消息，这取决于该值是否有效。

# 结论

我们可以使用 Vee-Validate 4 来验证 Vue 3 自定义输入组件。