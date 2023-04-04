# Vue 3 — Mixin 合并和指令

> 原文：<https://blog.devgenius.io/vue-3-mixin-merging-and-directives-886d09e403f7?source=collection_archive---------5----------------------->

![](img/516897b4c6ffb4319e4efbbc6afcf88b.png)

艾伦·尼格伦在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

**Vue 3 处于测试阶段，可能会有变化。**

Vue 3 是 Vue 前端框架的最新版本。

它建立在 Vue 2 的普及性和易用性之上。

在本文中，我们将研究如何改变 mixin 合并策略和定制指令。

# 自定义选项合并策略

我们可以改变 Vue 实例选项合并策略。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
  </head>
  <body>
    <div id="app">
      <p>app</p>
    </div> <script>
      const app = Vue.createApp({
        foo: "bar"
      }); app.config.optionMergeStrategies.foo = (toVal, fromVal) => {
        return toVal || fromVal;
      }; app.mixin({
        foo: "baz",
        created() {
          console.log(this.$options.foo);
        }
      }); app.mount("#app");
    </script>
  </body>
</html>
```

更改`foo`属性的选项合并属性。

如果存在的话，我们返回`toVal`。`toVal`是 mixin 的值。

否则，我们返回`fromVal`。`fromVal`是组件的值。

因此，`created`方法中的控制台日志会显示`'baz'`。

`this.$options.foo`有 Vue 组件选项。

Mixins 是 Vue 2 中提取应用程序可重用部分的主要工具。

但是它们容易发生冲突，可重用性有限。

我们不能向 mixin 传递任何参数来改变逻辑，这会降低它们的灵活性。

# 指令

Directives 是让我们直接操作 DOM 的 Vue 代码。

Vue 有很多内置的指令，比如`v-model`和`v-show`，让我们可以做很多事情。

我们可以创建自己的指令，让我们做我们想做的事情。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
  </head>
  <body>
    <div id="app">
      <input v-focus />
    </div> <script>
      const app = Vue.createApp({}); app.directive("focus", {
        mounted(el) {
          el.focus();
        }
      }); app.mount("#app");
    </script>
  </body>
</html>
```

创建一个`focus`指令，让我们在安装组件时关注它所应用的元素。

`el`是指令应用到的元素的 DOM element 对象。

我们必须在模板中添加前缀`v-`，这样我们就可以使用它了。

这避免了与其他属性的冲突。

# 钩子函数

Vue 指令有各种钩子函数。

`beforeMount`当指令第一次绑定到元素时，在装载父组件之前调用。

这是我们进行初始化的地方。

当绑定元素的父组件被挂载时，调用`mounted`。

在包含组件的 VNode 更新之前调用`beforeUpdate`。

当包含组件的 VNode 及其子组件的 VNode 被更新时，调用`updated`。

在卸载绑定元素的父组件之前调用`beforeUnmount`。

当指令与元素解除绑定并且父元素的组件被卸载时，调用`unmounted`。

指令可以接受参数。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
  </head>
  <body>
    <div id="app">
      <p v-size="50">foo</p>
    </div> <script>
      const app = Vue.createApp({}); app.directive("size", {
        mounted(el, binding) {
          el.style.fontSize = `${binding.value}px`;
        }
      }); app.mount("#app");
    </script>
  </body>
</html>
```

创建`size`指令来设置内容的字体大小。

`binding.value`具有我们传递到指令中的值。

![](img/731e5d87ac2db5118c03124441800670.png)

照片由[马希尔·云萨尔](https://unsplash.com/@mahiruysal?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 结论

Mixin 合并行为可以改变。

此外，我们可以创建新的指令来直接操作 DOM 元素。