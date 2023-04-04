# 使用基于类的组件开发 Vue 应用——类型注释

> 原文：<https://blog.devgenius.io/developing-vue-apps-with-class-based-components-type-annotations-dd6c28491385?source=collection_archive---------3----------------------->

![](img/7ddd5b0d190be12395ecd92dd0af0428.png)

照片由[艾米莉亚纳·霍尔](https://unsplash.com/@emilianatmbg?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

如果我们更喜欢使用类，我们可以使用 Vue 自带的基于类的组件 API。

在本文中，我们将看看如何用基于类的组件开发 Vue 应用。

# 外部挂钩的类型脚本自动完成

我们可以在用 TypeScript 编写的基于类的组件中为外部挂钩添加自动完成功能。

例如，我们可以写:

`vue-router-hook-types.ts`

```
import Vue from "vue";
import { Route, RawLocation } from "vue-router";declare module "vue/types/vue" {
  interface Vue {
    beforeRouteEnter?(
      to: Route,
      from: Route,
      next: (to?: RawLocation | false | ((vm: Vue) => void)) => void
    ): void; beforeRouteLeave?(
      to: Route,
      from: Route,
      next: (to?: RawLocation | false | ((vm: Vue) => void)) => void
    ): void; beforeRouteUpdate?(
      to: Route,
      from: Route,
      next: (to?: RawLocation | false | ((vm: Vue) => void)) => void
    ): void;
  }
}
```

`main.ts`

```
import Vue from "vue";
import App from "./App.vue";
import Foo from "./views/Foo.vue";
import Bar from "./views/Bar.vue";
import VueRouter from "vue-router";
import "./vue-router-hook-types";
Vue.config.productionTip = false;
const routes = [
  { path: "/foo", component: Foo },
  { path: "/bar", component: Bar }
];
const router = new VueRouter({
  routes
});
Vue.use(VueRouter);
new Vue({
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
</template>
<script>
export default {
  name: "App",
};
</script>
```

`views/Bar.vue`

```
<template>
  <div>bar</div>
</template>
<script>
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
  }
  beforeRouteUpdate(to, from, next) {
    console.log("beforeRouteUpdate");
    next();
  }
  beforeRouteLeave(to, from, next) {
    console.log("beforeRouteLeave");
    next();
  }
}
</script>
```

`views/Foo.vue`

```
<template>
  <div>foo</div>
</template>
<script>
import Vue from "vue";
import Component from "vue-class-component";
import "vue-class-component/hooks";Component.registerHooks([
  "beforeRouteEnter",
  "beforeRouteLeave",
  "beforeRouteUpdate",
]);
@Component
export default class Foo extends Vue {
  beforeRouteEnter(to, from, next) {
    console.log("beforeRouteEnter");
    next();
  }
  beforeRouteUpdate(to, from, next) {
    console.log("beforeRouteUpdate");
    next();
  }
  beforeRouteLeave(to, from, next) {
    console.log("beforeRouteLeave");
    next();
  }
}
</script>
```

我们有带 Vue 路由器钩子类型定义的`vue-router-hook-types.ts`。

在`main.ts`中，我们有:

```
import "./vue-router-hook-types";
```

导入类型定义。

然后在`Foo.vue`和`Bar.vue`组件中，我们可以在自动完成菜单中看到`beforeRouteEnter`、`beforeRouteUpdate`和`beforeRouteLeave`钩子，当我们将它们键入组件类代码时。

我们还可以在代码中标注方法的类型。

例如，我们可以写:

```
<template>
  <div>
    <button @click="increment">increment</button>
    <p>{{ count }}</p>
  </div>
</template><script lang="ts">
import Vue from "vue";
import Component from "vue-class-component";@Component(({
  watch: {
    count(val: number) {
      this.log(val) 
    }
  }
})
export default class HelloWorld extends Vue {
  count: number = 0 log(val: number): void {
    console.log(val)
  } increment(){
    this.count++
  }
}
</script>
```

我们有返回`void`并接受类型为`number`的`val`参数的`log`方法，

我们在传递给`Component`的参数中定义的`count`观察器中使用它。

这样，我们就不必担心用我们不期望的数据类型传递参数。

这同样适用于方法的返回类型。

# 结论

我们可以用 TypeScript 轻松地注释数据类型，以避免在基于 Vue 类的组件中出错。