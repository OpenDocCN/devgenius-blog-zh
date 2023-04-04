# Vuetify —输入提示和消息

> 原文：<https://blog.devgenius.io/vuetify-input-hints-and-messages-6420ef19ba34?source=collection_archive---------1----------------------->

![](img/38dfe995c3040cf7a0725a600bad8b74.png)

哈维尔·阿莱格·巴罗斯[在](https://unsplash.com/@soymeraki?utm_source=medium&utm_medium=referral) [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 输入提示

一个`v-input`组件使用一个`hint`道具让我们添加一个输入提示。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-input hint="a hint" persistent-hint :messages="messages">Input</v-input>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    messages: [],
  }),
};
</script>
```

我们添加了`persistent-hint`道具来使提示持久。

此外，我们还有带有提示文本的`hint`道具。

# 成功消息

我们可以用`success-messages`道具添加一条成功消息:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-input :success-messages="['Success']" success disabled>Input</v-input>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({}),
};
</script>
```

我们给`success-messages`道具设置了一个数组。

# 错误

我们可以用`error-messages`道具显示错误:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-input :error-messages="['an error occurred']" error disabled>Input</v-input>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({}),
};
</script>
```

# 多重错误

`error-count` prop 允许我们在一个输入中添加多个错误:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-input
          error-count="2"
          :error-messages="['an error', 'another error']"
          error
          disabled
        >Input</v-input>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({}),
};
</script>
```

我们用`error-count`属性来设置错误计数，用`error-messages`属性来设置错误消息数组。

# 规则

可以使用`rules`属性设置表单的验证规则:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-text-field :rules="rules"></v-text-field>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    rules: [
      (value) => !!value || "Required.",
      (value) => (value || "").length <= 10 || "Max 10 characters",
    ],
  }),
};
</script>
```

属性是一个带有函数的数组，如果需要的话会返回一个错误消息。

`value`具有我们输入的值。

# 自动隐藏详细信息

道具可以让我们自动隐藏消息。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-text-field label="Main input" :rules="rules" hide-details="auto"></v-text-field>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    rules: [
      (value) => !!value || "Required.",
      (value) => (value || "").length <= 10 || "Max 10 characters",
    ],
  }),
};
</script>
```

我们有带有验证规则的`rules`数组。

`hide-details`设置为`auto`如果没有要显示的验证错误，则隐藏验证错误。

![](img/b5ed3fdfe19c69c859dede07ba65b2b3.png)

照片由[你好，我是尼克🎞](https://unsplash.com/@helloimnik?utm_source=medium&utm_medium=referral)上[下](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用 Vuetify 文本输入添加输入提示和验证消息。