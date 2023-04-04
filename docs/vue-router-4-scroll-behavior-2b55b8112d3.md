# Vue 路由器 4–滚动行为

> 原文：<https://blog.devgenius.io/vue-router-4-scroll-behavior-2b55b8112d3?source=collection_archive---------6----------------------->

![](img/e1f02784913c59f0abda47b763fc1303.png)

[伊莱恩·豪林](https://unsplash.com/@elaineh?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Vue Router 4 处于测试阶段，可能会有变化。

为了轻松地构建一个单页面应用程序，我们需要添加路由，以便将 URL 映射到呈现的组件。

在这篇文章中，我们将看看如何使用 Vue 路由器 4 与 Vue 3。

# 滚动行为

我们可以用 Vue 路由器改变滚动行为。

为此，我们将`scrollBehavior`方法添加到传递给`createRouter`方法的对象中。

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
      <router-view></router-view>
      <p>
        <router-link to="/foo">foo</router-link>
        <router-link to="/bar">bar</router-link>
      </p>
    </div>
    <script>
      const Foo = {
        template: `<div>
          <p v-for='n in 100'>{{n}}</p>
        </div>`
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
        routes,
        scrollBehavior(to, from, savedPosition) {
          return { left: 0, top: 500 };
        }
      });
      const app = Vue.createApp({});
      app.use(router);
      app.mount("#app");
    </script>
  </body>
</html>
```

我们返回一个具有`left`和`top`属性的对象。

`left`是 x 坐标，`top`是路线改变时我们想要滚动到的`y`坐标。

`to`拥有我们要移动到的路线的路线对象。

而`from`有我们要移动的路线对象。

现在，当我们单击路由器链接时，我们移动到页面顶部附近的某个位置。

`savedPosition`具有我们在之前的路线中滚动的位置。

我们可以如下使用`savedPosition`对象:

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
      <router-view></router-view>
      <p>
        <router-link to="/foo">foo</router-link>
        <router-link to="/bar">bar</router-link>
      </p>
    </div>
    <script>
      const Foo = {
        template: `<div>
          <p v-for='n in 100'>{{n}}</p>
        </div>`
      };
      const Bar = {
        template: `<div>
          <p v-for='n in 150'>{{n}}</p>
        </div>`
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
        routes,
        scrollBehavior(to, from, savedPosition) {
          if (savedPosition) {
            return savedPosition;
          } else {
            return { left: 0, top: 0 };
          }
        }
      });
      const app = Vue.createApp({});
      app.use(router);
      app.mount("#app");
    </script>
  </body>
</html>
```

当定义了`savedPosition`对象时，我们返回它。

否则，当我们单击路由器链接时，我们会滚动到顶部。

现在，当我们滚动到链接时，我们将停留在页面的底部。

# 结论

我们可以使用 Vue Router 4 通过`scrollBehavior`方法滚动到页面上的给定位置。