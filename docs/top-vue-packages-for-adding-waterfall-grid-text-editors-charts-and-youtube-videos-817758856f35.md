# 用于添加瀑布网格、文本编辑器、图表和 YouTube 视频的顶级 Vue 包

> 原文：<https://blog.devgenius.io/top-vue-packages-for-adding-waterfall-grid-text-editors-charts-and-youtube-videos-817758856f35?source=collection_archive---------26----------------------->

![](img/6238c35d0439e74c7e564a41041006f2.png)

照片由[迈克·本纳](https://unsplash.com/@mbenna?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将看看添加瀑布网格、添加所见即所得编辑器、图表和嵌入 YouTube 视频的最佳包。

# 瀑布

vue-瀑布允许我们在我们的 Vue 应用程序中添加一个瀑布组件。

为了使用它，我们运行:

```
npm i vue-waterfall
```

然后我们可以通过写来使用它:

```
<template>
  <div id="app">
    <waterfall :line-gap="200" :watch="items">
      <waterfall-slot v-for="n in 100" :width="50" :height="200" :key="n">{{n}}</waterfall-slot>
    </waterfall>
  </div>
</template><script>
import Waterfall from "vue-waterfall/lib/waterfall";
import WaterfallSlot from "vue-waterfall/lib/waterfall-slot";
export default {
  components: {
    Waterfall,
    WaterfallSlot
  }
};
</script>
```

我们使用`waterfall`组件来添加组件。

这些项目由`waterfall-slot`组件填充。

项目的宽度和高度已设置。

# Vue JS Froala 所见即所得编辑器

Vue JS Froala WYSIWYG 编辑器是一个为 Vue 应用程序设计的富文本编辑器。

为了使用它，我们运行:

```
npm i vue-froala-wysiwyg
```

来安装它。

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import "froala-editor/js/plugins.pkgd.min.js";
import "froala-editor/js/third_party/embedly.min";
import "froala-editor/js/third_party/font_awesome.min";
import "froala-editor/js/third_party/spell_checker.min";
import "froala-editor/js/third_party/image_tui.min";
import "froala-editor/css/froala_editor.pkgd.min.css";import VueFroala from "vue-froala-wysiwyg";
Vue.use(VueFroala);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <froala :tag="'textarea'" :config="config" v-model="model"></froala>
  </div>
</template>

<script>
import VueFroala from "vue-froala-wysiwyg";export default {
  name: "app",
  data() {
    return {
      config: {
        events: {
          initialized() {
            console.log("initialized");
          }
        }
      },
      model: "hello!"
    };
  }
};
</script>
```

我们使用`froala`组件来呈现文本区域。

将`tag`设置为`'textarea'`来进行渲染。

然后，我们应该看到一个文本编辑器，它具有一些基本的功能，如粗体、斜体和下划线。

# Vue YouTube 嵌入

Vue YouTube Embed 允许我们在应用程序中嵌入 YouTube 视频。

为了使用它，我们运行:

```
npm i vue-youtube-embed
```

来安装它。

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueYouTubeEmbed from "vue-youtube-embed";
Vue.use(VueYouTubeEmbed);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <youtube video-id="NNfGvwqfrBY"></youtube>
  </div>
</template>

<script>
export default {};
</script>
```

我们使用`youtube`组件并设置`video-id`道具来嵌入视频。

我们还可以从完整的 URL 中获取视频 ID 和开始时间。

它发出各种我们可以听到的事件。

# Vue Chartkick

Vue Chartkick 是一个基于 chart.js 的图表库

我们通过运行以下命令来安装它:

```
npm i vue-chartkick chart.js
```

去使用它。

Chart.js 是必需的依赖项。

然后我们写道:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import Chartkick from "vue-chartkick";
import Chart from "chart.js";Vue.use(Chartkick.use(Chart));
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <line-chart :data="{'2020-01-01': 11, '2020-01-02': 6}"></line-chart>
  </div>
</template>

<script>
export default {};
</script>
```

我们只需注册插件并使用`line-chart`组件。

为了填充数据，我们将一个带有图表数据的对象传递给`data` prop。

关键是 x 轴值，值是 y 轴值。

它还支持其他图表。

我们可以用`pie-chart`组件添加一个饼图:

```
<template>
  <div id="app">
    <pie-chart :data="[['apple', 43], ['orange', 23]]"></pie-chart>
  </div>
</template>

<script>
export default {};
</script>
```

它支持许多其他图表。

# 所见即所得

vue-wysiwyg 是一个易于使用的 vue 应用程序富文本编辑器。

要安装它，我们运行:

```
npm i vue-wysiwyg
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import wysiwyg from "vue-wysiwyg";
Vue.use(wysiwyg, {});
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <wysiwyg v-model="content"/>
    <p>{{content}}</p>
  </div>
</template>

<script>
import "vue-wysiwyg/dist/vueWysiwyg.css";export default {
  data() {
    return {
      content: ""
    };
  }
};
</script>
```

我们使用`wysiwyg`组件添加富文本编辑器。

`v-model`将输入值绑定到`content`道具。

我们还必须记住导入 CSS，使它看起来正常。

![](img/0d67704032b6c74b69e218be0ddcb050.png)

照片由 [niko 照片](https://unsplash.com/@niko_photos?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

vue-fault 让我们在 Vue 应用程序中添加一个瀑布网格。

Vue JS Froala WYSIWYG Editor 和 Vue-WYSIWYG 2 是易于使用的富文本编辑器，我们可以在我们的 Vue 应用程序中使用。

Vue YouTube Embed 让我们可以轻松嵌入 YouTube 视频。

Vue Chartkick 是一个易于使用的图表库。