# Vue 路由器 4–路由属性和历史模式

> 原文：<https://blog.devgenius.io/vue-router-4-route-props-and-history-mode-d58700255fdf?source=collection_archive---------3----------------------->

![](img/b381493c612d1acc643f572d3e5f206e.png)

凯尔·格伦在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Vue Router 4 处于测试阶段，可能会有变化。

为了轻松地构建一个单页面应用程序，我们需要添加路由，以便将 URL 映射到呈现的组件。

在这篇文章中，我们将看看如何使用 Vue 路由器 4 与 Vue 3。

# 路由属性对象模式

我们可以设置路线的`props`属性来接受道具。

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
          path: "/bar",
          component: Bar,
          props: { id: 1 }
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

我们在带有对象的`routes`条目中有`props`属性:

```
{ id: 1 }
```

如果我们导航到`/bar`，那么`id`道具会自动传入。

`router-link`将`/bar`作为`to`属性的值。

当我们点击`bar`链接时，我们会看到`bar 1`显示出来。

1 来自道具。

这种传递路线道具的方式对于静态路线道具很有用。

# 路线道具功能模式

`props`属性也可以有一个函数作为它的值。

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
        <router-link to="/bar?id=1">bar</router-link>
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
          path: "/bar",
          component: Bar,
          props: (route) => ({ id: route.query.id })
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

我们有一个将`route`对象作为参数的函数。

我们从`query`属性中获取查询参数。

我们只是返回一个对象，它的键是查询参数的键。

该值是查询参数值。

`/bar`路线的`router-link`有`id`查询参数。

现在，当我们点击`bar`链接时，我们会看到`bar 1`显示出来。

# HTML5 历史模式

哈希模式是映射路由的默认方式。

它使用 URL 来模拟一个完整的 URL，这样当 URL 改变时就不会重新加载页面。

在历史模式下，我们去掉了散列符号，这样 URL 看起来就像我们看到的任何其他 URL。

为了使 HTML5 模式适用于各种服务器，我们可能需要对它进行配置，以便当我们转到不同的 URL 时，浏览器仍保留在应用程序中。

例如，在 Apache 中，我们写道:

```
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteBase /
  RewriteRule ^index\.html$ - [L]
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteRule . /index.html [L]
</IfModule>
```

使所有的 URL 映射到`index.html`，以便在发出任何请求时加载我们的 Vue 应用程序的`index.html`。

# 结论

我们可以使用 Vue Router 4 以各种方式传递路由属性。

如果我们对 Vue 3 应用程序使用历史模式，那么我们可能必须配置我们的 web 服务器，以便在发出任何请求时转到我们的应用程序。