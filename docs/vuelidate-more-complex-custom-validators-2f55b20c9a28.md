# vue lidate——更复杂的定制验证器

> 原文：<https://blog.devgenius.io/vuelidate-more-complex-custom-validators-2f55b20c9a28?source=collection_archive---------1----------------------->

![](img/69f756cecb4bfa9ae53ab7cf64c8d5c0.png)

由 [Motoculturel](https://unsplash.com/@motoculturel?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

默认情况下，Vue.js 没有任何表单验证功能。

因此，我们需要添加自己的表单验证库或自己的代码。

在本文中，我们将看看如何用 Vuelidate 创建定制的验证器。

# 访问组件

我们可以用 Vuelidate 访问整个组件的模型。

例如，我们可以写:

```
<template>
  <div id="app">
    <div>
      <label>name</label>
      <input v-model.trim="$v.name.$model">
      <div v-if="!$v.name.mustBeCool">name is invalid.</div>
    </div><div>
      <label>field</label>
      <input v-model.trim="$v.field.$model">
    </div>
  </div>
</template>
<script>
import { helpers } from "vuelidate/lib/validators";const contains = param =>
  helpers.withParams(
    { type: "contains", value: param },
    (value, vm) => !helpers.req(value) || vm[param.field].includes(param.text)
  );
export default {
  name: "App",
  data() {
    return {
      name: "",
      field: ""
    };
  },
  validations: {
    name: {
      mustBeCool: contains({ text: "cool", field: "field" })
    },
    field: {}
  }
};
</script>
```

添加一个验证器来检查`field`字段中是否有单词`'cool'`。

如果是的话，那么如果`field`字段中包含单词`'cool'`，那么使用`contains`验证器的字段就是有效的。

`vm`有组件的视图模型，有状态。

# 基于正则表达式的验证程序

我们可以添加基于正则表达式的验证。

例如，我们可以写:

```
<template>
  <div id="app">
    <div>
      <label>name</label>
      <input v-model.trim="$v.name.$model">
      <div v-if="!$v.name.alpha">name is invalid.</div>
    </div>
  </div>
</template>
<script>
import { helpers } from "vuelidate/lib/validators";const alpha = helpers.regex("alpha", /^[a-zA-Z]*$/);export default {
  name: "App",
  data() {
    return {
      name: ""
    };
  },
  validations: {
    name: {
      alpha
    }
  }
};
</script>
```

我们添加了`alpha`验证器来检查输入的值是否都是字母。

# 基于定位器的验证器

此外，我们可以使用定位器策略进行验证。

例如，我们可以写:

```
<template>
  <div id="app">
    <div>
      <label>name</label>
      <input v-model.trim="$v.password.$model">
    </div> <div>
      <label>confirm password</label>
      <input v-model.trim="$v.confirmPassword.$model">
      <div v-if="!$v.confirmPassword.same">confirm pasword must be the same as password.</div>
    </div>
  </div>
</template>
<script>
import { helpers } from "vuelidate/lib/validators";const sameAs = equalTo =>
  helpers.withParams({ type: "sameAs", eq: equalTo }, function(
    value,
    parentVm
  ) {
    return value === helpers.ref(equalTo, this, parentVm);
  });export default {
  name: "App",
  data() {
    return {
      password: "",
      confirmPassword: ""
    };
  },
  validations: {
    password: {},
    confirmPassword: {
      same: sameAs("password")
    }
  }
};
</script>
```

`helper.ref`方法使用`equalTo`参数检查两个字段是否相等。

`equalTo`参数具有要检查的字段名，以确定它是否与该字段相同。

所以`equalTo`应该是`'password'`，因为我们想检查它是否与`'password'`相同。

我们返回条件来比较另一个字段的值是否与我们输入给定字段的值相同。

# 结论

我们可以用 Vuelidate 的各种策略创建我们自己的定制验证器。