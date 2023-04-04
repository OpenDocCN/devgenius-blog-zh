# 常见的 Vue 问题——命名空间 Vuex 操作、下载二进制文件等等

> 原文：<https://blog.devgenius.io/common-vue-problems-namespaced-vuex-actions-downloading-binaries-and-more-6428cd78b79f?source=collection_archive---------22----------------------->

![](img/4a4ac0af19a9f266e09123a6cceb9470.png)

照片由[丹尼尔·赫雷斯](https://unsplash.com/@danieljerez?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Vue.js 让开发前端应用变得简单。然而，我们仍有可能遇到问题。

在这篇文章中，我们将看看一些常见的问题，并看看如何解决它们。

# 在两个命名空间 Vuex 操作之间调度操作

假设我们有 2 个或更多的名称空间 Vuex 操作:

`game.js`

```
const actions = {
  myAction({dispatch}) {
    ...
    dispatch('notification/triggerSelfDismissingNotifcation', {...})
  }
}
```

`notification.js`

```
const actions = {
  dismiss(context, payload) {
    ...
  }
}
```

我们可以通过在动作名称前添加名称空间来调度它们。

例如，我们可以写:

```
dispatch('notification/dismiss', {...}, { root: true })
```

第一个参数是动作的名称，前面带有名称空间名称。

第二个参数像往常一样具有有效载荷。

第三个参数中的`root`属性用于访问命名空间操作。

没有它就找不到动作。

我们还必须记住在 Vuex 商店模块中将`namespaced`设置为`true`。

# 从 URL 获取查询参数

我们可以从带有`this.$route.query`属性的 URL 中获取查询参数。

例如，如果我们有:

```
/url?foo=bar
```

在我们的 URL 中，我们可以通过编写`this.$route.query.foo`来获得带有关键字`foo`的查询参数。

# Vue 路由器路由器链接活动样式

我们可以向`router-link-active`类或`router-link-exact-active`类添加样式，向活动链接添加一些样式，比如高亮。

`router-link-active`匹配目标路线时应用。

`router-link-exct-active`当找到一条路线的精确匹配时，匹配。

对于样式，我们可以这样写:

```
.router-link-active,
.router-link-exact-active {
  background-color: green;
  cursor: pointer;
}
```

然后，我们将为活动的`router-link`获取绿色背景。

类名可以更改。

为此，我们可以设置`linkActiveClass`和`linkExactActiveClass`属性。

例如，我们可以写:

```
const router = new VueRouter({
  routes,
  linkActiveClass: "active",
  linkExactActiveClass: "exact-active",
})
```

我们分别将活动类从`router-link-active`和`router-link-exact-active`更改为`active`和`exact-active`。

# 用 Vuex 给一个动作传递多个参数

Vuex 动作接受一个`state`和`pyaload`参数。

因此，如果我们需要传入多条数据，我们可以向第二个参数`commit`传入一个对象。

例如，我们可以写:

```
store.commit('authenticate', {
  token,
  expiration,
});
```

在第二个参数中，我们有一个具有`token`和`expiration`属性的对象。

那么为了定义`authenticate`突变，我们可以写:

```
mutations: {
  authenticate(state, { token, expiration }) {
    localStorage.setItem('token', token);
    localStorage.setItem('expiration', expiration);
  }
}
```

我们从第二个参数，即`payload`参数中获取属性。

然后我们可以用它做我们想做的事情，比如将值保存在本地存储中。

# 用 Axios 保存 Blob 数据

我们可以通过创建一个新的`Blob`实例来用 Axios 保存 blob 数据。

然后我们可以用它调用`window.URL.ceateObjectURL`。

例如，我们可以写:

```
axios
  .get(`url/with/pdf`, {
    responseType: 'arraybuffer'
  })
  .then(response => {
     const blob = new Blob([response.data], { type: 'application/pdf' }),
     const url = window.URL.createObjectURL(blob);
     window.open(url);
  })
```

我们将`responseType`设置为`'arraybuffer'`来告诉 Axios 我们正在下载一个二进制文件。

然后我们创建一个`Blob`实例，在第一个参数的数组中有二进制数据的`response.data`。

第二个参数将文件 mime 类型设置为`'application/pdf'`。

然后我们创建 URL 让我们用`window.URL.createObjectURL`下载文件。

最后，我们用返回的 URL 调用`window.open`将文件下载到用户的计算机上。

# 每次部署后清除缓存

我们可以通过几种方式在部署 Vue 应用程序后清除缓存。

一种方法是使用`webpack-assets-manifest`在静态文件的文件名后添加一个随机散列名。

此外，我们可以将它上传到 CDN 中的版本化文件夹。

`no-cache`标题也适用于除 IE 之外的大多数浏览器，所以我们可以将它们设置为禁用缓存。

![](img/e4fb7d02f3a45b6bec88acaea35d5721.png)

[Joshua Hoehne](https://unsplash.com/@mrthetrain?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以用一些像`webpack-assets-manifest`这样的包来清空缓存，给我们的静态文件添加一个散列。

Vuex 动作可以有名称空间，这样我们就可以把它们分成不同的名称空间。

Vuex 变异的有效载荷是它的第二个参数，所以如果我们想传入一个数据，我们可以传递值；如果我们想传入更多数据，我们可以传递一个对象。