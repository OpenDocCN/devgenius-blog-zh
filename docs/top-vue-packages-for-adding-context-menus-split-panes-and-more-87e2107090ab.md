# 用于添加上下文菜单、分割窗格等的顶级 Vue 包

> 原文：<https://blog.devgenius.io/top-vue-packages-for-adding-context-menus-split-panes-and-more-87e2107090ab?source=collection_archive---------11----------------------->

![](img/3603c2b126b4baf775bdc75f58c0f0be.png)

照片由[斯潘塞·戴维斯](https://unsplash.com/@spencerdavis?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将看看检测远离元素的点击、添加上下文菜单、更改元数据和添加分割窗格的最佳包。

# vue 元信息

vue-meta-info 是一个 vue 插件，让我们为我们的 Vue 应用程序设置元数据。

为了使用它，我们运行:

```
npm install vue-meta-info --save
```

安装软件包。

然后我们写道:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import MetaInfo from "vue-meta-info";Vue.use(MetaInfo);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div></div>
</template>

<script>
export default {
  metaInfo: {
    title: "my app",
    meta: [
      {
        name: "keyWords",
        content: "my app"
      }
    ],
    link: [
      {
        rel: "assets",
        href: "https://assets-cdn.github.com/"
      }
    ]
  }
};
</script>
```

我们导入插件，然后我们可以传入`metaInfo`属性来设置所有的元数据。

`title`是标题。

`meta`是元属性。

`name`是属性名，`content`是值。

`link`是链接标签。`rel`是 rel 属性，`href`是 href 属性。

# vue-linkify

vue-linkify 是一个插件，可以自动将任何看起来像 URL 的文本变成链接。

为了使用它，我们运行:

```
npm i vue-linkify
```

来安装它。

然后我们写道:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import linkify from "vue-linkify";Vue.directive("linkified", linkify);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <div v-html="raw" v-linkified/>
  </div>
</template>
<script>
export default {
  data() {
    return {
      raw: "Hello from example.com"
    };
  }
};
</script>
```

我们有了`v-linkified`指令，我们将原始文本设置为`v-html`指令的值，这样我们就可以呈现链接。

此外，我们可以更改链接的类名，以便我们可以对其进行样式化。

我们可以写:

```
<template>
  <div id="app">
    <div v-html="raw" v-linkified:options="{ className: 'example' }"/>
  </div>
</template>
<script>
export default {
  data() {
    return {
      raw: "Hello from example.com"
    };
  }
};
</script>
```

并且该链接将具有类`example`。

# 拆分窗格

Splitpanes 是 Vue 应用程序的布局组件。

为了使用它，我们运行:

```
npm i splitpanes
```

来安装它。

然后我们写道:

```
<template>
  <div id="app">
    <splitpanes style="height: 400px">
      <pane min-size="20">1</pane>
      <pane>
        <splitpanes horizontal>
          <pane>2</pane>
          <pane>3</pane>
        </splitpanes>
      </pane>
      <pane>4</pane>
    </splitpanes>
  </div>
</template><script>
import { Splitpanes, Pane } from "splitpanes";
import "splitpanes/dist/splitpanes.css";
export default {
  components: { Splitpanes, Pane }
};
</script>
```

去使用它。

我们定义窗格来放置我们的列，并且我们可以嵌套它们。

我们所要做的就是使用`splitpanes`和`pane`组件。

# vue-点击方式 2

我们可以使用 vue-clickaway2 包在元素外部添加检测点击。

为了使用它，我们运行:

```
npm i vue-clickaway2
```

来安装它。

然后我们写道:

```
<template>
  <div id="app">
    <p v-on-clickaway="away">Click away</p>
  </div>
</template><script>
import { mixin as clickaway } from "vue-clickaway2";export default {
  mixins: [clickaway],
  methods: {
    away() {
      console.log("clicked away");
    }
  }
};
</script>
```

去使用它。

当我们在 p 元素外单击时，我们使用`v-on-clickaway`指令来运行一个方法。

此外，我们可以添加修饰符来检测各种鼠标事件。

例如，我们可以添加`mousedown`来检测`mousedown` 事件，而不是点击事件。

# v-上下文菜单

v-contextmenu 是一个上下文菜单组件，我们可以在我们的 Vue 应用程序中使用。

为了使用它，我们运行:

```
npm i v-contextmenu vue-runtime-helpers
```

来安装它。

它有`vue-runtime-helpers`作为依赖项。

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import contentmenu from "v-contextmenu";
import "v-contextmenu/dist/index.css";Vue.use(contentmenu);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <v-contextmenu ref="contextmenu">
      <v-contextmenu-item>foo</v-contextmenu-item>
      <v-contextmenu-item>bar</v-contextmenu-item>
      <v-contextmenu-item>baz</v-contextmenu-item>
    </v-contextmenu> <div v-contextmenu:contextmenu>right click here</div>
  </div>
</template><script>
export default {};
</script>
```

我们注册插件。

然后我们使用`v-contextmenu`组件让我们在右键单击它时显示一个上下文菜单。

修饰符是上下文菜单名称的引用。

然后我们使用`v-contextmenu`来添加我们的上下文菜单。

该组件可以嵌套以创建嵌套菜单。

![](img/3a2d5c5cacdae3764a50804327ca6c5d.png)

照片由[萨曼莎·亨托什](https://unsplash.com/@samanthahentosh?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

vue-meta-info 允许我们更改 vue 应用程序的标题和元数据。

Splitpanes 允许我们添加拆分窗格。

vue-clickaway2 检测元素外部的点击。

v-contextmenu 是我们应用程序的上下文菜单组件。