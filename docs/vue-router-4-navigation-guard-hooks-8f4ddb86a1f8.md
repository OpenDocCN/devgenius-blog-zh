# Vue 路由器 4–导航保护钩

> 原文：<https://blog.devgenius.io/vue-router-4-navigation-guard-hooks-8f4ddb86a1f8?source=collection_archive---------8----------------------->

![](img/29269a352480e424d4199a9fcd81416c.png)

照片由[马克·利什曼](https://unsplash.com/@mark_leishman2?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Vue Router 4 处于测试阶段，可能会有变化。

为了轻松地构建一个单页面应用程序，我们需要添加路由，以便将 URL 映射到呈现的组件。

在这篇文章中，我们将看看如何使用 Vue 路由器 4 与 Vue 3。

# 全局后挂钩

我们可以在导航挂钩后创建全局。

为此，我们用回调来调用`router.afterEach`方法。

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
      }); router.afterEach((to, from) => {
        console.log(to, from);
      });
      const app = Vue.createApp({});
      app.use(router);
      app.mount("#app");
    </script>
  </body>
</html>
```

添加我们的后导航挂钩。

它在我们导航到路线后被调用。

`to`是被导航到的路线对象。

`from`是被导航离开的路线对象。

路由对象有`fullPath`、元数据、查询参数、URL 参数等等。

没有`next`函数让我们进行重定向或抛出错误。

# 每条路线的警卫

我们可以在每条路线上增加导航守卫。

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
          beforeEnter: (to, from, next) => {
            console.log(to, from);
            next();
          }
        }
      ];
      const router = VueRouter.createRouter({
        history: VueRouter.createWebHistory(),
        routes
      }); const app = Vue.createApp({});
      app.use(router);
      app.mount("#app");
    </script>
  </body>
</html>
```

我们在`/bar` route 对象中有`beforeEnter`方法。

`next`功能可以让我们解析导航，因为它是在我们输入路线之前运行的。

# 组件内防护装置

我们可以在组件内部添加导航保护。

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
        template: "<div>bar</div>",
        beforeEnter(to, from, next) {
          console.log(to, from);
          next();
        },
        beforeRouteUpdate(to, from, next) {
          console.log(to, from);
          next();
        },
        beforeRouteLeave(to, from, next) {
          console.log(to, from);
          next();
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
      }); const app = Vue.createApp({});
      app.use(router);
      app.mount("#app");
    </script>
  </body>
</html>
```

为我们的`Bar`组件添加导航防护装置。

在确认渲染组件之前调用`beforeRouteEnter`方法。

它不能访问`this`组件实例，因为它还没有被创建。

当呈现组件的路径改变时，调用`beforeRouterUpdate`方法。

例如，当具有动态参数的路线改变时，它将运行。

它可以访问`this`组件实例。

`beforeRouterLeave`在呈现组件的路径将要被导航时被调用。

它还可以访问`this`组件实例。

# 结论

我们可以用 Vue Router 4 在各个位置添加导航护钩。