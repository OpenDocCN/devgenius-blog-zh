# Vuetify —步进器定制

> 原文：<https://blog.devgenius.io/vuetify-steppers-customization-cc9c2e0dec9c?source=collection_archive---------7----------------------->

![](img/7c1e0c8999fb936892a291b726a9e21c.png)

照片由[林赛·亨伍德](https://unsplash.com/@lindsayhenwood?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 垂直步进机

我们可以让步进器垂直显示。

例如，我们可以写:

```
<template>
  <v-stepper v-model="step" vertical>
    <v-stepper-step :complete="step > 1" step="1">
      Select an app
      <small>Summarize if needed</small>
    </v-stepper-step> <v-stepper-content step="1">
      <v-card color="grey lighten-1" class="mb-12" height="200px"></v-card>
      <v-btn color="primary" @click="step = 2">Continue</v-btn>
      <v-btn text>Cancel</v-btn>
    </v-stepper-content> <v-stepper-step :complete="step > 2" step="2">Configure keywords</v-stepper-step> <v-stepper-content step="2">
      <v-card color="grey lighten-1" class="mb-12" height="200px"></v-card>
      <v-btn color="primary" @click="step = 3">Continue</v-btn>
      <v-btn text>Cancel</v-btn>
    </v-stepper-content> <v-stepper-step :complete="step > 3" step="3">Select ad unit</v-stepper-step> <v-stepper-content step="3">
      <v-card color="grey lighten-1" class="mb-12" height="200px"></v-card>
      <v-btn color="primary" @click="step = 4">Continue</v-btn>
      <v-btn text>Cancel</v-btn>
    </v-stepper-content>
  </v-stepper>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    step: 1,
  }),
};
</script>
```

在`v-stepper-content`上方添加`v-stepper-step`，在内容上方显示步骤标题。

# 线性步进机

我们可以制作一个线性步进器，让用户按顺序完成每个步骤:

```
<template>
  <div>
    <v-stepper>
      <v-stepper-header>
        <v-stepper-step step="1" complete>Select keywords</v-stepper-step> <v-divider></v-divider> <v-stepper-step step="2" complete>Create an ad group</v-stepper-step> <v-divider></v-divider> <v-stepper-step step="3">Create an ad</v-stepper-step>
      </v-stepper-header>
    </v-stepper>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({}),
};
</script>
```

我们添加了`complete`道具，这样用户就不能回到那一步了。

# 非线性步进机

为了使步骤可编辑，我们可以添加`editable`道具:

```
<template>
  <div>
    <v-stepper>
      <v-stepper-header>
        <v-stepper-step step="1" editable>Select keywords</v-stepper-step> <v-divider></v-divider> <v-stepper-step step="2" editable>Create an ad group</v-stepper-step> <v-divider></v-divider> <v-stepper-step step="3">Create an ad</v-stepper-step>
      </v-stepper-header>
    </v-stepper>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({}),
};
</script>
```

`editable`道具让用户点击这个步骤。

# 替代标签

我们可以在带有`alt-labels`道具的步骤图标下方放置文本:

```
<template>
  <div>
    <v-stepper alt-labels>
      <v-stepper-header>
        <v-stepper-step step="1">Select keywords</v-stepper-step> <v-divider></v-divider> <v-stepper-step step="2">Create an ad group</v-stepper-step> <v-divider></v-divider> <v-stepper-step step="3">Create an ad</v-stepper-step>
      </v-stepper-header>
    </v-stepper>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({}),
};
</script>
```

# 多线错误状态

我们可以用`rules`属性添加一个错误状态:

```
<template>
  <div>
    <v-stepper alt-labels>
      <v-stepper-header>
        <v-stepper-step step="1">Select keywords</v-stepper-step> <v-divider></v-divider> <v-stepper-step step="2" :rules="[() => false]">
          Create an ad group
          <small>Alert message</small>
        </v-stepper-step> <v-divider></v-divider> <v-stepper-step step="3">Create an ad</v-stepper-step>
      </v-stepper-header>
    </v-stepper>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({}),
};
</script>
```

我们有一个`rules`属性，它有一个返回`false`来显示错误状态的函数数组。

步骤文本将为红色。

# 结论

我们可以配置我们的步进器来处理规则和各种道具。