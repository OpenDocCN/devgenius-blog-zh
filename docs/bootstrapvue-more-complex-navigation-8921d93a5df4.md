# BootstrapVue —更复杂的导航

> 原文：<https://blog.devgenius.io/bootstrapvue-more-complex-navigation-8921d93a5df4?source=collection_archive---------14----------------------->

![](img/a1d48fe660ae7680988a2def00da09fc.png)

照片由[克里斯蒂娜·戈塔迪](https://unsplash.com/@cristina_gottardi?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

为了制作好看的 Vue 应用，我们需要设计组件的样式。

为了使我们的生活更容易，我们可以使用内置样式的组件。

在本文中，我们将看看如何添加导航组件。

# 嵌入式表单

`b-nav-form`让我们将表单添加到导航栏中。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-nav>
      <b-nav-item active>foo</b-nav-item>
      <b-nav-item>bar</b-nav-item>
      <b-nav-form @submit.stop.prevent="submit">
        <b-form-input></b-form-input>
        <b-button type="submit">Ok</b-button>
      </b-nav-form>
    </b-nav>
  </div>
</template>
<script>
export default {
  name: "App",
  methods: {
    submit(){
      alert('success')
    }
  }
};
</script>
```

我们在`b-nav-form`里面有`b-form-input`。

我们可以通过在`b-nav-form`上设置`submit`事件处理程序来调用提交处理程序。

# 卡片集成

我们可以在卡片里添加一个导航条。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-card title="Card Title" no-body>
      <b-card-header header-tag="nav">
        <b-nav card-header tabs>
          <b-nav-item active>foo</b-nav-item>
          <b-nav-item>bar</b-nav-item>
          <b-nav-item disabled>baz</b-nav-item>
        </b-nav>
      </b-card-header> <b-card-body class="text-center">
        <b-card-text>Content.</b-card-text> <b-button variant="primary">Do Something</b-button>
      </b-card-body>
    </b-card>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script>
```

我们只是把`b-nav`放在`b-card-header`里面。

然后我们添加`card-header`道具，使其适合卡头。

`tabs`使其显示为选项卡。

或者，我们可以将其更改为药丸样式:

```
<template>
  <div id="app">
    <b-card title="Card Title" no-body>
      <b-card-header header-tag="nav">
        <b-nav card-header pills>
          <b-nav-item active>foo</b-nav-item>
          <b-nav-item>bar</b-nav-item>
          <b-nav-item disabled>baz</b-nav-item>
        </b-nav>
      </b-card-header> <b-card-body class="text-center">
        <b-card-text>Content.</b-card-text> <b-button variant="primary">Do Something</b-button>
      </b-card-body>
    </b-card>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script>
```

如果我们想要一个简单风格的导航条，我们也可以去掉`card-header`道具:

```
<template>
  <div id="app">
    <b-card title="Card Title" no-body>
      <b-card-header header-tag="nav">
        <b-nav>
          <b-nav-item active>foo</b-nav-item>
          <b-nav-item>bar</b-nav-item>
          <b-nav-item disabled>baz</b-nav-item>
        </b-nav>
      </b-card-header> <b-card-body class="text-center">
        <b-card-text>Content.</b-card-text> <b-button variant="primary">Do Something</b-button>
      </b-card-body>
    </b-card>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script>
```

# 与 Vue 路由器一起使用

我们可以在 Vue 路由器上使用 nav。为此，我们必须定义一条父路由和一组子路由。

例如，我们可以写:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import { BootstrapVue } from "bootstrap-vue";
import "bootstrap/dist/css/bootstrap.css";
import "bootstrap-vue/dist/bootstrap-vue.css";
import VueRouter from "vue-router";
import Foo from "./components/Foo";
import Bar from "./components/Bar";
import Root from "./components/Root";const routes = [
  {
    path: "/",
    component: Root,
    children: [
      {
        path: "foo",
        component: Foo
      },
      {
        path: "bar",
        component: Bar
      }
    ]
  }
];const router = new VueRouter({
  routes
});Vue.use(VueRouter);
Vue.use(BootstrapVue);
Vue.config.productionTip = false;new Vue({
  router,
  render: h => h(App)
}).$mount("#app");
```

我们添加了`VueRouter`插件和路线。

我们必须有一个导航的父路由和显示路由内容的子路由。

子路线在`children`道具中。

然后我们加上:

`App.vue`

```
<template>
  <div id="app">
    <router-view></router-view>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script>
```

这是父`router-view`。

然后我们添加我们的 nav 和子节点`router-view`:

`components/Root.vue`

```
<template>
  <div id="app">
    <b-card title="Card Title" no-body>
      <b-card-header header-tag="nav">
        <b-nav>
          <b-nav-item to="/foo" exact exact-active-class="active">foo</b-nav-item>
          <b-nav-item to="/bar" exact exact-active-class="active">bar</b-nav-item>
        </b-nav>
      </b-card-header> <b-card-body class="text-center">
        <router-view></router-view>
      </b-card-body>
    </b-card>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script>
```

我们有带`to`道具的`b-nav-item`。我们有`exact`道具来精确匹配路线。

`exact-active-class`是导航到路线时应用的 CSS 类。

然后我们添加我们的子路由:

`component/Foo.vue`:

```
<template>
  <p>foo</p>
</template><script>
export default {};
</script>
```

`component/Bar.vue`:

```
<template>
  <p>bar</p>
</template><script>
export default {};
</script>
```

![](img/93fb7225fc3fe6201df44cf1886e6058.png)

安布尔·基普在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以在导航中添加各种东西，比如内嵌表格。

另外，我们可以把导航放在卡片里。

它还与 Vue 路由器集成，因此我们可以通过 nav 导航到路线。