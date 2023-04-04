# Vue 路由器 4–导航防护装置

> 原文：<https://blog.devgenius.io/vue-router-4-navigation-guards-7992fa57d220?source=collection_archive---------4----------------------->

![](img/3dc2ee06dff15154d6a80aac9d681b80.png)

照片由[琳达·芬金](https://unsplash.com/@lwitham?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Vue Router 4 处于测试阶段，可能会有变化。

为了轻松地构建一个单页面应用程序，我们需要添加路由，以便将 URL 映射到呈现的组件。

在这篇文章中，我们将看看如何使用 Vue 路由器 4 与 Vue 3。

# 导航防护装置

我们可以通过重定向或取消导航来添加导航防护。

守卫只有在我们导航到不同的路线时才会被触发。

URL 参数和查询更改不会触发进入或离开导航卫士。

# 警卫前的全局

我们可以通过向`router.beforeEach`方法传递一个回调来添加一个全局 before guard。

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
        props: ["id"]
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
      }); router.beforeEach((to, from, next) => {
        console.log(to, from);
        next();
      });
      const app = Vue.createApp({});
      app.use(router);
      app.mount("#app");
    </script>
  </body>
</html>
```

我们用回调来调用`router.beforeEach`方法。

回调需要一个`to`、`from`和`next`参数。

`to`是导航到的路线对象。

`from`是被导航离开的路线对象。

路由对象有`fullPath`、元数据、查询参数、URL 参数等等。

`next`是解析钩子必须调用的函数。

我们需要调用`next`来显示`to`路线。

`next`函数可以接受一个参数。

如果我们进入`false`，则当前导航被中止。

如果浏览器 URL 被更改，它将重置为`from`路线中的 URL。

我们也可以用路径字符串传入一个路径字符串或者一个带有`path`属性的对象。

不管怎样，我们都要去不同的地方。

当前导航被中止，将开始新的导航。

对象可以有额外的选项，如来自路线的`replace`和`name`属性。

任何被`router.push`接受的属性都被接受。

如果有任何错误，我们也可以传入一个`Error`实例。

然后导航将被中止，错误将被传递给用`router.onError()`注册的回调。

我们应该确保`next`在回调中只被调用一次。

否则，导航可能永远无法解决，或者可能导致错误。

所以与其写:

```
router.beforeEach((to, from, next) => {
  if (to.name !== 'login' && !isAuthenticated) {
    next({ name: 'login' })
  }
  next()
})
```

我们写道:

```
router.beforeEach((to, from, next) => {
  if (to.name !== 'login' && !isAuthenticated) {
    next({ name: 'login' });
  }
  else {
    next();
  }
})
```

# 全球解析卫士

我们可以用`router.beforteResolve`注册一个全局守卫。

它类似于`router.beforeEach`，除了它将在导航被确认之前被调用。

# 结论

我们可以添加导航卫士，用 Vue Router 4 控制导航。