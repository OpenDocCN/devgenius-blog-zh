# Vue 路由器 4–组件内防护、路由元字段和转换

> 原文：<https://blog.devgenius.io/vue-router-4-in-component-guards-route-meta-fields-and-transitions-6c32c3b01c8c?source=collection_archive---------2----------------------->

![](img/e1dcb6e5ef1f433a66886823a38ec916.png)

米切尔·奥尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Vue Router 4 处于测试阶段，可能会有变化。

为了轻松地构建一个单页面应用程序，我们需要添加路由，以便将 URL 映射到呈现的组件。

在这篇文章中，我们将看看如何使用 Vue 路由器 4 与 Vue 3。

# 组件内防护装置

在渲染组件的路径被确认之前，运行`beforeRouteEnter`保护

因此，它没有访问`this`的权限。

然而，我们可以在`next`回调中访问它。

为此，我们写道:

```
beforeRouteEnter(to, from, next) {
  next(vm => {
    // ...
  })
}
```

`vm`有路由组件的实例。

这是唯一支持为`next`传入回调的路由。

我们可以在`beforeRouterUpdate`和`beforeRouterLeave`方法中直接使用`this`，所以不支持向`next`传递回调。

所以我们可以写:

```
beforeRouteUpdate (to, from, next) {
  this.name = to.params.name
  next()
}
```

或者:

```
beforeRouteLeave (to, from, next) {
  const answer = window.confirm('Do you really want to leave?')
  if (answer) {
    next()
  } else {
    next(false)
  }
}
```

`beforeRouteLeave`通常用于防止用户意外离开路线。

# 导航解析流程

导航的步骤如下:

1.  导航触发。
2.  调用`beforeRouteLeave`停用部件中的防护装置。
3.  呼叫全局`beforeEach`警卫。
4.  调用重复使用组件中的`beforeRouteUpdate`防护装置。
5.  在路线配置中调用`beforeEnter`。
6.  解析异步路由组件。
7.  调用激活组件中的`beforeRouteEnter`。
8.  呼叫全局`beforeResolve`守卫。
9.  导航确认。
10.  调用全局`afterEach`钩子。
11.  DOM 更新被触发。
12.  运行传递给实例化实例的`beforeRouteEnter`守卫中的`next`的回调。

# 路由元字段

我们可以用`meta`属性添加路由元字段。

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
          meta: { requiresAuth: true }
        }
      ];
      const router = VueRouter.createRouter({
        history: VueRouter.createWebHistory(),
        routes
      }); router.beforeEach((to, from, next) => {
        console.log(to.meta.requiresAuth);
        next();
      }); const app = Vue.createApp({});
      app.use(router);
      app.mount("#app");
    </script>
  </body>
</html>
```

向我们的`routes`数组条目添加一个`meta`属性。

我们有一个属性为`requiresAuth`的对象。

然后在`beforeEach`回调中，我们可以用`to.meta.requiresAuth`属性访问它。

# 过渡

我们可以使用 Vue 路由器为路线变更添加转换。

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
        transition: opacity 0.5s;
      }
      .fade-enter,
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
      <router-view v-slot="{ Component }">
        <transition name="fade">
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
      }); const app = Vue.createApp({});
      app.use(router);
      app.mount("#app");
    </script>
  </body>
</html>
```

向我们的应用程序添加过渡。

我们添加`router-view`并访问`Component` slot prop 来获得通过`router-view`显示的组件。

然后我们可以将它传递给`router-view`中的`component`组件。

我们设置了`name`属性，这样我们就可以设置为动画添加的 CSS 类的前缀。

在`style`标签之间，我们有动画不同阶段的样式。

过渡的阶段在[https://v3 . vuejs . org/guide/transitions-overview . html # class-based-animations-transitions](https://v3.vuejs.org/guide/transitions-overview.html#class-based-animations-transitions)中列出。

# 结论

我们可以使用 Vue Router 4 添加组件内防护和路由转换。

我们添加路由转换的方式不同于 Vue Router 3。