# 用于处理触摸事件、砖石网格等的顶级 Vue 包

> 原文：<https://blog.devgenius.io/top-vue-packages-for-handling-touch-events-masonry-grid-and-more-5bcba5774936?source=collection_archive---------17----------------------->

![](img/46bf4fb5c7565ce96ffa261a0cf7ca72.png)

[戈达弗](https://unsplash.com/@gettagottaflow?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将看看处理触摸事件的最佳包，添加砖石网格，添加 Github 按钮和 HTML5 编辑器。

# 真空锤

vue-hammer 是一个让我们处理触摸事件的组件。

要使用它，我们通过运行以下命令来安装它:

```
npm i vue2-hammer
```

然后，我们可以通过运行以下命令来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import { VueHammer } from "vue2-hammer";
Vue.use(VueHammer);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <a v-hammer:tap="onTap">Tap me!</a>
  </div>
</template><script>
export default {
  methods: {
    onTap() {
      console.log('tapped');
    }
  }
};
</script>
```

我们注册了这个插件，并使用`v-hammer:tap`指令来检测对它的点击。

然后`onTap`就会运行。

它可以观察其他事件，如平移、印刷等。

# vue-砖石工程-css

我们可以使用 vue-masonry-css 在网格中显示项目。

要安装它，我们运行:

```
npm i vue-masonry-css
```

来安装它。

然后我们可以写:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueMasonry from "vue-masonry-css";Vue.use(VueMasonry);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <masonry :cols="3" :gutter="30">
      <div v-for="item in 100" :key="item">Item: {{item}}</div>
    </masonry>
  </div>
</template><script>
export default {};
</script>
```

我们使用`masonry`组件将元素放入网格中。

我们可以在里面放任何东西。

`cols`是要显示的列数。

`gutter`是各列之间的间隙。

# Vue GitHub 按钮

Vue GitHub 按钮让我们将 GitHub 按钮添加到我们的 Vue 应用程序中来使用它，我们运行:

```
npm i vue-github-buttons
```

来安装它。

然后我们可以写:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueGitHubButtons from "vue-github-buttons";import "vue-github-buttons/dist/vue-github-buttons.css";Vue.use(VueGitHubButtons);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <gh-btns-watch slug="mertJF/tailblocks" show-count></gh-btns-watch>
    <gh-btns-star slug="mertJF/tailblocks" show-count></gh-btns-star>
    <gh-btns-fork slug="mertJF/tailblocks" show-count></gh-btns-fork>
  </div>
</template><script>
export default {};
</script>
```

`gh-btns-watch`让我们得到一个回购的观察者的数量。

`slug`是回购名称。

`show-count`显示计数。

`gh-btns-star`显示回购上的星数。

`gh-btns-fork`显示回购上的分叉数量。

计数是自动获得的。

我们可以将`useCache`添加到设置中，使用缓存的值进行计数，而不是每次都获取值。

# vue-html 5-编辑器

vue-html5-editor 是一个用于 vue 应用程序的 html 编辑器。

为了使用它，我们运行:

```
npm i vue-html5-editor
```

来安装它。

然后我们写下以下内容:

`index.html`

```
<link
  rel="stylesheet"
  href="https://stackpath.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css"
/>
```

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueHtml5Editor from "vue-html5-editor";
Vue.use(VueHtml5Editor, {
  name: "vue-html5-editor",
  showModuleName: false,
  icons: {
    text: "fa fa-pencil",
    color: "fa fa-paint-brush",
    font: "fa fa-font",
    align: "fa fa-align-justify",
    list: "fa fa-list",
    link: "fa fa-chain",
    unlink: "fa fa-chain-broken",
    tabulation: "fa fa-table",
    image: "fa fa-file-image-o",
    hr: "fa fa-minus",
    eraser: "fa fa-eraser",
    undo: "fa-undo fa",
    "full-screen": "fa fa-arrows-alt",
    info: "fa fa-info"
  }
});
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <vue-html5-editor :content="content" :height="500"></vue-html5-editor>
  </div>
</template><script>
export default {
  data(){
    return {
      content: ''
    }
  }
};
</script>
```

我们添加了`vue-html-editor`组件以在编辑器中使用它。

此外，我们还添加了字体 Awesome CSS 来添加图标。

然后我们将内容传递给编辑器。

我们还可以设置一个上传处理器来处理图片上传。

![](img/4272d048a671e8d9b9fbb9004564a4a9.png)

由 [T.R 摄影](https://unsplash.com/@_redo_?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

vue-html5-editor 是一个专为 vue 设计的 html5 编辑器。

Vue GitHub Buttons 是 Vue 的一个 GitHub 按钮组件。

vue-hammer 是一个处理触摸事件的指令。

css 让我们创建一个砖石网格。