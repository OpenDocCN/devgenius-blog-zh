# Vue 路由器 4–重定向

> 原文：<https://blog.devgenius.io/vue-router-4-redirects-b6019f6580d1?source=collection_archive---------3----------------------->

![](img/2a8d7d643be940b36f0b9ff6d1fce617.png)

[水晶钻](https://unsplash.com/@crystalsjo?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

Vue Router 4 处于测试阶段，可能会有变化。

为了轻松地构建一个单页面应用程序，我们需要添加路由，以便将 URL 映射到呈现的组件。

在这篇文章中，我们将看看如何使用 Vue 路由器 4 与 Vue 3。

# 再直接的

我们可以在路线中添加重定向。

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
          component: Bar
        },
        {
          path: "/baz",
          redirect: "/bar"
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

在`routes`阵列中我们有 3 条路线。

在第三个条目中，我们使用了`redirect`属性将`/baz`路由重定向到`/bar`路由。

我们有一条`router-link`指向`/baz`路线。

所以当我们点击它的时候，我们仍然可以看到`bar`显示。

重定向也适用于命名路由。

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
          name: "bar"
        },
        {
          path: "/baz",
          redirect: { name: "bar" }
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

我们在`/bar`路线中有`name`属性，并且在`/baz`路线中有`redirect`属性被设置为:

```
{ name: "bar" }
```

因此，我们会看到，当我们单击“baz”时，会看到`bar`。

我们也可以用回调方法重定向。

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
          name: "bar"
        },
        {
          path: "/baz",
          redirect: (to) => "bar"
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

我们在`redirect`回调中返回`'bar'`，这样当我们点击`/baz`时，它就会转到`/bar`。

我们得到了同样的结果。

# 结论

我们可以使用 Vue Router 4 以各种方式重定向我们的路由。