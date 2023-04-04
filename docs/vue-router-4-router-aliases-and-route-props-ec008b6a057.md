# Vue 路由器 4–路由器别名和路由属性

> 原文：<https://blog.devgenius.io/vue-router-4-router-aliases-and-route-props-ec008b6a057?source=collection_archive---------5----------------------->

![](img/a1c5266cca0910b2b8ada914a5c8f6bd.png)

马特·邓肯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Vue Router 4 处于测试阶段，可能会有变化。

为了轻松地构建一个单页面应用程序，我们需要添加路由，以便将 URL 映射到呈现的组件。

在这篇文章中，我们将看看如何使用 Vue 路由器 4 与 Vue 3。

# 别名

重定向意味着当用户访问`/a`时，URL 被替换为`/b`并匹配为`/b`。

`/a`的别名为`/b`意味着当用户访问`/b`时，URL 保持为`/b`，但会像用户访问`/a`一样进行匹配。

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
        <router-link to="/foo">foo</router-link>
        <router-link to="/bar">bar</router-link>
        <router-link to="/baz">baz</router-link>
      </p>
      <router-view></router-view>
    </div>
    <script>
      const Foo = {
        template: "<div>foo</div>"
      };
      const Bar = {
        template: "<div>bar</div>"
      };
      const routes = [
        {
          path: "/foo",
          component: Foo
        },
        {
          path: "/bar",
          component: Bar,
          alias: "/baz"
        }
      ];
      const router = VueRouter.createRouter({
        history: VueRouter.createWebHistory(),
        routes
      });
      const app = Vue.createApp({});
      app.use(router);
      app.mount("#app");
    </script>
  </body>
</html>
```

我们有了`alias`属性以及我们想要映射到`Bar`组件的路径。

然后，当我们单击`baz`链接时，我们会看到显示的`bar`。

# 传递属性以路由组件

我们可以通过道具来路由组件。

这样，我们就不必将路由参数与组件耦合起来。

我们不使用`this.$router.currentRoute.value.params`属性，而是使用 props 来获取路线参数。

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
        <router-link to="/foo">foo</router-link>
        <router-link to="/bar/1">bar</router-link>
      </p>
      <router-view></router-view>
    </div>
    <script>
      const Foo = {
        template: "<div>foo</div>"
      };
      const Bar = {
        template: "<div>bar {{id}}</div>",
        props: ["id"]
      };
      const routes = [
        {
          path: "/foo",
          component: Foo
        },
        {
          path: "/bar/:id",
          component: Bar,
          props: true
        }
      ];
      const router = VueRouter.createRouter({
        history: VueRouter.createWebHistory(),
        routes
      });
      const app = Vue.createApp({});
      app.use(router);
      app.mount("#app");
    </script>
  </body>
</html>
```

我们将`props`属性添加到`routes`条目中，并将其设置为`true`。

我们在路由中有了`:id` URL 参数占位符。

然后我们添加`Bar`的`props`属性来取`id`道具。

`id`道具显示在`Bar`的模板中。

这样我们就可以从 route 参数中得到`id`道具。

此外，我们将`router-link`的`to`道具设置为`'/bar/1'`。

这样，我们将 1 看作是`Bar`组件中`id`的值。

# 结论

我们可以添加别名并传递路由 URL 参数作为道具，而不是从 Vue Router 4 中的`$router`对象访问值。