# 常见的 Vue 问题—状态更新、Vuex 和事件处理程序

> 原文：<https://blog.devgenius.io/common-vue-problems-state-updates-vuex-and-event-handlers-8a570c6cac8a?source=collection_archive---------14----------------------->

![](img/570b4eaa4ac824b1d55bdc20cecd4e07.png)

莎伦·麦卡琴在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Vue.js 让开发前端应用变得简单。然而，我们仍有可能遇到问题。

在这篇文章中，我们将看看一些常见的问题，并看看如何解决它们。

# 将事件和参数传递给 Vue.js 中的 v-on

我们可以将参数传递给我们在 Vue 的`v-on`指令中调用的方法。

例如，我们可以写:

```
<input type="number" v-on:input="addToCart($event, item.id)" min="0" placeholder="0">
```

在我们的模板中。

然后在我们的组件中，我们可以写:

```
methods: {
  addToCart(event, id){
    console.log(id)
  }
}
```

`$event`拥有从`input`事件中发出的对象。

`id`拥有我们在模板代码中传递的`item.id`。

# nextTick 的目的

`nextTick`让我们在更改数据后做些什么。

这就像普通 JavaScript 中的`setTimeout`。

`nextTick`从数据变化中检测变化，所以它总是在数据更新后运行。

没有意识到。

例如，如果我们有:

```
<template>
  <div>
    {{ val }}
  </div>
</template><script>
export default {
  data() {
    return {
      val: 1
    }
  },
  mounted() {
    this.val = 2; this.$nextTick(() => {
      this.val = 3;
    });
  }
}
</script>
```

`this.val`的初始值为 1。

然后调用`mounted`钩子，更新`this.val`为 2。

一旦更新完成，Vue 就将`this.val`更新为 3。

# $mount()和$el 之间的区别

`$mount`让我们在需要时显式挂载 Vue 实例。

有了它，我们可以延迟`Vue`实例，直到一个特定的元素存在。

这对于将 Vue 添加到通过操作 DOM 添加元素的遗留应用程序非常有用。

`el`将`Vue`实例直接挂载到 DOM 中，而不先检查它是否存在。

因此，将实例安装到静态元素或我们知道总是在`Vue`实例之前加载的东西是很有用的。

# [Vue 警告]:找不到元素

这个错误意味着 Vue 不能选择给定的元素来安装组件。

例如，如果我们有:

```
const main = new Vue({
  el: '#main',
  data: {
    hello: 'home'
  }
});
```

那么没有找到 ID 为`main`的元素。

为了解决这个问题，我们应该确保在创建`Vue`实例之前加载元素。

例如，我们可以写:

```
window.onload = () => {
  const main = new Vue({
    el: '#main',
    data: {
      hello: 'home'
    }
  });
}
```

或者我们可以写:

```
window.addEventListener('load', () => {
  // add the same Vue instance as the previous example
})
```

它们都允许我们在给定元素出现在 DOM 中之后挂载`Vue`实例。

# 在初始 Vue.js 加载时加载所有服务器端数据

我们可以在创建 Vue 实例之前，通过编写自己的函数来加载数据，从而将数据加载到 Vuex 存储中。

例如，我们可以写:

```
const store = (data) => {
  return new Vuex.Store({
    state: {
      exams: data,
    },
    actions,
    getters,
    mutations,
  });
}fetch('/some/url')
  .then(response => response.json())
  .then((data) => {
    new Vue({
      el: '#app',
      router,
      store: store(data),
      template: '<App/>',
      components: { App },
    });
  });
```

我们有一个用`store`创建商店的函数。

然后我们调用传递给`Vue`构造函数的对象中的`store`函数。

我们传递给`store`的`data`来自于`fetch`函数调用。

这样，数据将在进入商店之前被填充到商店中。

# 使用 Vue CLI 项目导入 SASS 或 SCSS

我们可以通过添加`node-sass`和`sass-loaer`包，用 Vue CLI 创建的 Vue app 导入 SASS 或 SCSS。

然后我们可以在代码中加载 SCSS 文件。

为了安装软件包，我们运行:

```
npm install -D node-sass sass-loader
```

然后我们可以写:

```
import './styles/my-styles.scss'
```

在`main.js`中全局导入样式。

如果我们想在文件中导入一个 SASS 或 SCSS 文件，那么我们写:

```
<style lang="scss">
  ...
</style>
```

添加让 Vue 知道 SCSS 代码。

![](img/c0cfdc499fa2c3838bc57561b2b8a7c6.png)

照片由 [Melody Jacob](https://unsplash.com/@melodyjacob1?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

为了让我们在现有项目中使用 SASS 或 SCSS 代码，我们可以使用 SASS loader 包。

如果我们想在加载 Vue 应用程序之前将数据加载到 Vuex 商店，我们可以加载数据，然后用数据创建 Vuex 商店。

然后我们可以用商店创建我们的 Vue 实例。

我们可以将任何东西传递给事件处理程序。

`nextTick`便于在其他更新后更新我们的状态。