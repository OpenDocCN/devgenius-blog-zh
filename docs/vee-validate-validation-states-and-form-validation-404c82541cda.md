# Vee-Validate —验证状态和表单验证

> 原文：<https://blog.devgenius.io/vee-validate-validation-states-and-form-validation-404c82541cda?source=collection_archive---------7----------------------->

![](img/0ffd6a627ebeb02f284c864722eaebe1.png)

照片由 [Paige Cody](https://unsplash.com/@paige_cody?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

表单验证是 Vue.js 没有内置的功能。

但是，我们还是非常需要这个功能。

在本文中，我们将了解如何使用检查和显示验证状态。

此外，我们看看如何验证整个表单。

# 验证状态

除了显示`errors`，我们还可以检查表单的验证状态。

# 旗帜

有许多可用的验证标志。它们包括:

*   `valid` —检查字段是否有效
*   `invalid` —检查字段是否无效
*   `changed` —检查字段是否被更改
*   `touched` —检查某个区域是否被模糊了
*   `untouched` —检查字段是否模糊
*   `pristine` —检查字段值是否未被操纵
*   `dirty` —检查字段值是否被操纵
*   `pending` —指示字段验证是否正在进行
*   `required` —检查字段是否为必填字段
*   `validated` —检查字段是否至少验证一次
*   `passed` —检查字段是否已经过验证，是否有效
*   `failed` —检查字段是否已经过验证，是否无效

它们可以作为吃角子老虎机道具。

所以我们可以通过书写来使用它们:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import { ValidationProvider, extend } from "vee-validate";
import { required } from "vee-validate/dist/rules";extend("required", required);
Vue.component("ValidationProvider", ValidationProvider);Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <ValidationProvider rules="required" v-slot="{ valid }">
      <input v-model="value" type="text">
      <span>{{ valid }}</span>
    </ValidationProvider>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: ""
    };
  }
};
</script>
```

我们只是从`v-slot`得到`valid`，并在它下面显示它的值。

# 处理表单

除了检查表单验证状态，我们还可以在提交之前检查表单的所有值是否都有效。

例如，我们可以写:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import { ValidationProvider, ValidationObserver, extend } from "vee-validate";
import { required, alpha } from "vee-validate/dist/rules";extend("required", required);
extend("alpha", alpha);Vue.component("ValidationProvider", ValidationProvider);
Vue.component("ValidationObserver", ValidationObserver);Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <ValidationObserver v-slot="{ handleSubmit }">
      <form [@submit](http://twitter.com/submit).prevent="handleSubmit(onSubmit)">
        <ValidationProvider rules="required|alpha" v-slot="{ errors }">
          <input v-model="name" name="name" type="text">
          <span>{{ errors[0] }}</span>
        </ValidationProvider>
        <input type="submit" value="submit">
      </form>
    </ValidationObserver>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      name: ""
    };
  },
  methods: {
    onSubmit() {
      alert("submitted");
    }
  }
};
</script>
```

我们导入了`required`和`alpha`规则。

此外，除了`ValidationProvider`之外，我们还导入了`ValidationObserver`组件。

我们将`ValidationProvider`嵌套在`ValidationObserver`中，以观察所有字段的验证状态。

`form`标签有提交处理程序，它是通过获取`handleSubmit`函数创建的。

然后我们将自己的`onSubmit`方法传递给它。

现在，当我们输入字母数据时，我们会看到“已提交”警报。

这意味着`onSubmit`被调用。

否则，`onSubmit`就不会被称为。

如果它没有被调用，那么至少某些表单值是无效的。

# 使用$refs 进行编程访问

我们可以通过引用来访问表单验证状态。

例如，我们可以通过编写以下代码来重写示例:

```
<template>
  <div id="app">
    <ValidationObserver ref="form">
      <form [@submit](http://twitter.com/submit).prevent="onSubmit">
        <ValidationProvider rules="required|alpha" v-slot="{ errors }">
          <input v-model="name" name="name" type="text">
          <span>{{ errors[0] }}</span>
        </ValidationProvider>
        <input type="submit" value="submit">
      </form>
    </ValidationObserver>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      name: ""
    };
  },
  methods: {
    async onSubmit() {
      const success = await this.$refs.form.validate();
      if (!success) {
        return;
      }
      alert("submitted");
      this.$nextTick(() => {
        this.$refs.form.reset();
      });
    }
  }
};
</script>
```

我们将`ref`设为`'form'`。

当我们点击表单上的提交时，我们的`onSubmit`方法。

然后在`onSubmit`中，我们通过在`this.$refs.form`上运行`validate`方法来检查表单的有效性。

它返回一个具有`success`值的承诺。如果表单有效，则解析为`true`。

否则就是`false`。

因此，是`success`是`false`，我们停止运行`onSubmit`。

如果表单有效，我们会显示“已提交”警告。

我们重置`this.name`的值。

在下一次使用`this.$nextTick`的渲染迭代中，我们使用`this.$refs.form.reset();`重置表单验证状态。

![](img/74db87d9639779ff954b95b288146d13.png)

由[马丁·莫雷诺](https://unsplash.com/@memoreno?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以使用 Vee-Validate 来检查整个表单的验证状态。

为此，我们使用`ValidationObserver`组件来观察表单的验证状态。

我们可以用内置的`handleSubmit`函数或者通过给`ValidationObserver`分配一个参考来检查。