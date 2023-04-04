# Vue 路由器 4–导航期间的每条路线转换和获取数据

> 原文：<https://blog.devgenius.io/vue-router-4-per-route-transition-and-fetch-data-during-navigation-cfb953d9997f?source=collection_archive---------3----------------------->

![](img/419ca2d94a29e3d51845b4a879a17333.png)

照片由 [Anastasia Petrova](https://unsplash.com/@anastasia_p?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Vue Router 4 处于测试阶段，可能会有变化。

为了轻松地构建一个单页面应用程序，我们需要添加路由，以便将 URL 映射到呈现的组件。

在这篇文章中，我们将看看如何使用 Vue 路由器 4 与 Vue 3。

# 每条路线转换

我们可以为个别路线添加过渡效果。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <script src="https://unpkg.com/vue@next"></script>
    <script src="https://unpkg.com/vue-router@4.0.0-beta.7/dist/vue-router.global.js"></script>
    <title>App</title>
    <style>
      .fade-enter-active,
      .fade-leave-active {
        transition: opacity 0.15s ease;
      }
      .fade-enter-from,
      .fade-leave-to {
        opacity: 0;
      }
    </style>
  </head>
  <body>
    <div id="app">
      <p>
        <router-link to="/foo">foo</router-link>
        <router-link to="/bar">bar</router-link>
      </p>
      <router-view v-slot="{ Component, route }">
        <transition :name='route.path.includes("foo") ? `fade`:``'>
          <component :is="Component" />
        </transition>
      </router-view>
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

我们通过检查`route`对象来动态设置`name`属性。

`path`地产有该路线的路径。

因此，我们可以检查路径以将`name`的值设置为我们想要的过渡。

# 数据提取

当路由被激活时，我们有时想从服务器获取数据。

这可以通过各种挂钩来完成。

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
        template: "<div>{{bar}}</div>",
        data() {
          return {
            bar: ""
          };
        },
        created() {
          this.fetchData();
        },
        watch: {
          $route: "fetchData"
        },
        methods: {
          async fetchData() {
            this.bar = "bar";
          }
        }
      };
      const routes = [
        {
          path: "/foo",
          component: Foo
        },
        {
          path: "/bar",
          component: Bar
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

我们观察`$route`对象，并在`$route`改变时调用`fetchData`方法。

# 导航前提取

我们还可以运行方法来获取 before `before`导航钩子中的数据。

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
        <router-link to="/bar/1">bar 1</router-link>
        <router-link to="/bar/2">bar 2</router-link>
      </p>
      <router-view></router-view>
    </div>
    <script>
      const Foo = {
        template: "<div>foo</div>"
      };
      const Bar = {
        template: "<div>{{id}}</div>",
        data() {
          return {
            id: ""
          };
        },
        beforeRouteEnter(to, from, next) {
          next((vm) => vm.fetchData(to.params.id));
        },
        beforeRouteUpdate(to, from, next) {
          this.fetchData(to.params.id);
          next();
        },
        methods: {
          async fetchData(id) {
            this.id = id;
          }
        }
      };
      const routes = [
        {
          path: "/foo",
          component: Foo
        },
        {
          path: "/bar/:id",
          component: Bar
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

我们有`beforeRouterEnter`和`beforeRouteUpdate`钩子，它们分别在组件渲染之前和路径参数改变时运行。

在回调中运行`fetchData`方法来获取数据。

我们有 2 个`router-link`用自己的参数连接到`/bar`路线。

我们获取导航保护方法中的参数`id`并调用`fetchData`将其值设置为`this.id`状态。

因此，我们将在模板中看到它们。

# 结论

我们有一种新的方式来添加每路由转换与 Vue 路由器 4。

此外，我们可以用 Vue Router 4 在 Vue Router 挂钩中获取数据。