# 如何用 Vue Router 4 去除 URL 中的哈希？

> 原文：<https://blog.devgenius.io/how-to-remove-the-hashbang-from-the-url-with-vue-router-4-6640ac6bd7c6?source=collection_archive---------3----------------------->

![](img/a081394efb971261043b76bbf060abf5.png)

由 [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

有时，我们希望在使用 Vue Router 4 的 Vue 3 应用程序中将组件映射到前面没有哈希的路径。

在本文中，我们将看看如何使用 Vue Router 4 从 URL 路径中删除散列。

# 使用 Vue Router 4 从 URL 中删除哈希

我们可以通过调用`createWebHistory`方法，用 Vue Router 4 从 URL 中删除散列。

例如，我们可以写:

`main.js`

```
import { createApp } from "vue";
import { createRouter, createWebHistory } from "vue-router";
import App from "./App.vue";
import Page1 from "./views/Page1";
import Page2 from "./views/Page2";
import Page3 from "./views/Page3";const routes = [
  { path: "/", component: Page1 },
  { path: "/page2", component: Page2 },
  { path: "/page3", component: Page3 }
];const router = createRouter({
  history: createWebHistory(),
  routes
});const app = createApp(App);
app.use(router);
app.mount("#app");
```

`App.vue`

```
<template>
  <router-link to="/">page 1</router-link>
  <router-link to="/page2">page 2</router-link>
  <router-link to="/page3">page 3</router-link>
  <router-view></router-view>
</template><script>
export default {
  name: "App"
};
</script>
```

`views/Page1.vue`

```
<template>
  <div>page 1</div>
</template>
```

`views/Page2.vue`

```
<template>
  <div>page 2</div>
</template>
```

`views/Page3.vue`

```
<template>
  <div>page 3</div>
</template>
```

在`main.js`中，我们有了带有路线定义的`routes`数组。

我们将 URL 路径映射到组件。

然后我们用一个对象调用`createRouter`，该对象的`history`属性被设置为由`createWebHistory`返回的值。

`createWebHistory`将路由器设置为 HTML5 历史模式，删除哈希。

这将从 Vue 路由器 4 映射的路径中删除哈希。

我们还将`routes`属性设置为`routes`数组，以添加我们创建的路线。

接下来，我们用`router`调用`app.use`来添加路由器对象。

在`App.vue`中，我们有`router-link`来呈现链接。

`router-view`渲染路线页面组件。

`Page1`、`Page2`和`Page3`是路线组件。

现在，当我们单击路由器链接时，我们不应该看到主机名和路由路径之间的散列。

# 结论

我们可以用 Vue Router 4 的`createWebHistory`函数从 URL 中删除散列。