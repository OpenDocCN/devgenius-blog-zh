# Vue 路由器 4–404 和嵌套路由

> 原文：<https://blog.devgenius.io/vue-router-4-404-and-nested-routes-ce1d23c9a8b0?source=collection_archive---------2----------------------->

![](img/2646c581ed01b486849adb07b40db93a.png)

乔恩·赛勒在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

Vue Router 4 处于测试阶段，可能会有变化。

为了轻松地构建一个单页面应用程序，我们需要添加路由，以便将 URL 映射到呈现的组件。

在这篇文章中，我们将看看如何使用 Vue 路由器 4 与 Vue 3。

# 无所不包/ 404 未找到路线

我们可以通过使用星号模式作为路径来创建一个总括或 404 路由。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <script src="https://unpkg.com/vue@next"></script>
    <script src="https://unpkg.com/vue-router@4.0.0-beta.7/dist/vue-router.global.js"></script>
    <title>App</title>
  </head>
  <body>
    <div id="app">
      <p>
        <router-link to="/foo">Foo</router-link>
      </p>
      <router-view></router-view>
    </div>
    <script>
      const Foo = {
        template: "<div>foo</div>"
      }; const NotFound = {
        template: "<div>not found</div>"
      }; const routes = [
        { path: "/foo", component: Foo },
        { path: "/:catchAll(.*)", component: NotFound }
      ]; const router = VueRouter.createRouter({
        history: VueRouter.createWebHistory(),
        routes
      }); const app = Vue.createApp({});
      app.use(router);
      app.mount("#app");
    </script>
  </body>
</html>
```

我们用`/:catchAll(.*)`捕获组而不是 Vue Router 3 中的`*`定义了一个全包路由。

现在，当我们转到任何不是`foo`的路径时，我们会看到“未找到”的消息。

# 嵌套路由

我们可以用`children`属性创建嵌套路由。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <script src="https://unpkg.com/vue@next"></script>
    <script src="https://unpkg.com/vue-router@4.0.0-beta.7/dist/vue-router.global.js"></script>
    <title>App</title>
  </head>
  <body>
    <div id="app">
      <p>
        <router-link to="/parent/home">parent</router-link>
      </p>
      <router-view></router-view>
    </div>
    <script>
      const Parent = {
        template: `<div>
          parent
          <router-view></router-view>
        </div>`
      }; const Home = {
        template: `<div>home</div>`
      }; const routes = [
        {
          path: "/parent",
          component: Parent,
          children: [{ path: "home", component: Home }]
        }
      ]; const router = VueRouter.createRouter({
        history: VueRouter.createWebHistory(),
        routes
      }); const app = Vue.createApp({
        watch: {
          $route() {
            console.log(this.$route.resolve);
          }
        }
      });
      app.use(router);
      app.mount("#app");
    </script>
  </body>
</html>
```

我们有一个`routes`数组，其中的一个对象具有`children`属性。

该属性有一个带有`path`和`component`属性的对象，就像常规路线一样。

在`router-link`中，`to`道具有到嵌套路线的路径。

`Parent`组件有`router-view`组件，这样我们可以查看子路由的内容。

因此，当我们单击父链接时，我们会看到:

```
parent
home
```

显示文本。

我们可以在父路由中包含 URL 参数。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <script src="[https://unpkg.com/vue@next](https://unpkg.com/vue@next)"></script>
    <script src="[https://unpkg.com/vue-router@4.0.0-beta.7/dist/vue-router.global.js](https://unpkg.com/vue-router@4.0.0-beta.7/dist/vue-router.global.js)"></script>
    <title>App</title>
  </head>
  <body>
    <div id="app">
      <p>
        <router-link to="/parent/1/home">parent</router-link>
      </p>
      <router-view></router-view>
    </div>
    <script>
      const Parent = {
        template: `<div>
          <div>parent {{ $router.currentRoute.value.params.id }}</div>
          <div><router-view></router-view></div>
        </div>`
      }; const Home = {
        template: `<div>home</div>`
      }; const routes = [
        {
          path: "/parent/:id",
          component: Parent,
          children: [{ path: "home", component: Home }]
        }
      ]; const router = VueRouter.createRouter({
        history: VueRouter.createWebHistory(),
        routes
      }); const app = Vue.createApp({
        watch: {
          $route() {
            console.log(this.$route.resolve);
          }
        }
      });
      app.use(router);
      app.mount("#app");
    </script>
  </body>
</html>
```

添加`:id` URL 参数占位符。

由于`router-link`的`to`道具现在是`/parent/1/home`并且我们在`Parent`的模板中有了`$router.currentRoute.value.params.id`，我们将看到数字 1 显示出来。

父级的 URL 参数占位符保留在父级中。

# 结论

我们在 Vue 路由器 4 中定义 404 和嵌套路由的方式和 Vue 路由器 3 略有不同。