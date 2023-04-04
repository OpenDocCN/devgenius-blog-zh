# 使用基于类的组件开发 Vue 应用——挂钩和计算属性

> 原文：<https://blog.devgenius.io/developing-vue-apps-with-class-based-components-hooks-and-computed-properties-5b0e2870b478?source=collection_archive---------4----------------------->

![](img/ca7922888d8f6ffbc517f01f08bc9082.png)

照片由[丹妮尔·塞鲁洛](https://unsplash.com/@dncerullo?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

如果我们更喜欢使用类，我们可以使用 Vue 自带的基于类的组件 API。

在本文中，我们将看看如何用基于类的组件开发 Vue 应用。

# 基于类的组件中的计算属性

要将计算属性添加到基于类的组件中，我们可以将 getters 和 setters 添加到我们的类中。

例如，我们可以写:

```
<template>
  <div>
    <input v-model="name" />
  </div>
</template><script>
import Vue from "vue";
import Component from "vue-class-component";@Component
export default class HelloWorld extends Vue {
  firstName = "jane";
  lastName = "smith"; get name() {
    return `${this.firstName} ${this.lastName}`;
  } set name(value) {
    const [firstName, lastName] = value.split(" ");
    this.firstName = firstName;
    this.lastName = lastName || "";
  }
}
</script>
```

我们有一个用`v-model`绑定到`name`计算属性的输入。

在组件类中，我们有`firstName`和`lastName`反应属性。

在那下面，我们有返回组合的`firstName`和`lastName`的`name` getter 方法。

然后我们添加一个`name` setter 来获取`value`并用`split`分割它。

然后，我们将分割值分配回`this.firstName`和`this.lastName`。

# 钩住

当组件被挂载时,`mounted`钩子允许我们运行代码。

而`render`钩子让我们在类组件中呈现模板内容:

```
<script>
import Vue from "vue";
import Component from "vue-class-component";@Component
export default class HelloWorld extends Vue {
  mounted() {
    console.log("mounted");
  } render() {
    return <div>Hello World</div>;
  }
}
</script>
```

我们用`render`方法替换了`template`标签。

# 注册组件

我们可以在传递给`Component`装饰器的对象中用`components`属性注册组件。

例如，我们可以写:

```
<template>
  <Greeting />
</template><script>
import Vue from "vue";
import Component from "vue-class-component";const Greeting = {
  template: `<div>hello</div>`,
};@Component({
  components: {
    Greeting,
  },
})
export default class HelloWorld extends Vue {}
</script>
```

注册我们创建的`Component`。

# Vue 路由器挂钩

如果我们想在基于 Vue 类的组件中添加其他挂钩，我们必须注册它们。

例如，我们可以写:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import Foo from "./views/Foo";
import Bar from "./views/Bar";
import VueRouter from "vue-router";
Vue.config.productionTip = false;const routes = [
  { path: "/foo", component: Foo },
  { path: "/bar", component: Bar }
];const router = new VueRouter({
  routes
});Vue.use(VueRouter);new Vue({
  router,
  render: (h) => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <router-link to="/foo">Foo</router-link>
    <router-link to="/bar">Bar</router-link>
    <router-view></router-view>
  </div>
</template><script>
export default {
  name: "App",
};
</script>
```

`views/Foo.vue`

```
<template>
  <div>foo</div>
</template><script>
import Vue from "vue";
import Component from "vue-class-component";
Component.registerHooks([
  "beforeRouteEnter",
  "beforeRouteLeave",
  "beforeRouteUpdate",
]);
@Component
export default class Foo extends Vue {
  beforeRouteEnter(to, from, next) {
    console.log("beforeRouteEnter");
    next();
  } beforeRouteUpdate(to, from, next) {
    console.log("beforeRouteUpdate");
    next();
  } beforeRouteLeave(to, from, next) {
    console.log("beforeRouteLeave");
    next();
  }
}
</script>
```

`views/Bar.vue`

```
<template>
  <div>bar</div>
</template><script>
import Vue from "vue";
import Component from "vue-class-component";
Component.registerHooks([
  "beforeRouteEnter",
  "beforeRouteLeave",
  "beforeRouteUpdate",
]);@Component
export default class Bar extends Vue {
  beforeRouteEnter(to, from, next) {
    console.log("beforeRouteEnter");
    next();
  } beforeRouteUpdate(to, from, next) {
    console.log("beforeRouteUpdate");
    next();
  } beforeRouteLeave(to, from, next) {
    console.log("beforeRouteLeave");
    next();
  }
}
</script>
```

我们用`main.js`中的`routes`数组将 Vue 路由器添加到我们的 Vue 应用程序中。

然后我们用`routes`数组创建`VueRouter`实例。

我们调用`Vue.use(VueRouter);`来注册组件和组件方法。

我们将`router`传递给`Vue`实例。

在`App.vue`中，我们添加了`router-link`和`router-view`来呈现路线和链接，我们可以点击这些链接来浏览路线。

然后我们用`Component.registerHooks`注册`Foo.vue`和`Bar.vue`中的 Vue 路由器钩子。

我们将带有钩子名称的字符串传入数组。

最后，我们在组件类中添加同名的方法。

现在，当我们浏览页面时，钩子将会运行。

# 结论

我们可以在基于 Vue 类的组件中添加计算属性和挂钩。