# 用于添加文本编辑器、微调器、进度条和文件上传的顶级 Vue 包

> 原文：<https://blog.devgenius.io/top-vue-packages-for-adding-text-editor-spinner-progress-bar-and-file-upload-73307e168259?source=collection_archive---------39----------------------->

![](img/78d58c290f3fc48e9a4100f7e6fc3cda.png)

[David Clode](https://unsplash.com/@davidclode?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在这篇文章中，我们将看看添加富文本编辑器、文件上传控件、微调器和进度条的最佳包。

# vue 2 编辑器

Vue2Editor 是一个基于 Quill 的易于使用的富文本编辑器组件。

要安装它，我们可以运行:

```
npm i vue2-editor
```

然后我们可以通过写来使用它:

```
<template>
  <div id="app">
    <vue-editor v-model="content"></vue-editor>
    <p v-html="content"></p>
  </div>
</template>

<script>
import { VueEditor } from "vue2-editor";export default {
  components: {
    VueEditor
  }, data() {
    return {
      content: "<h1>hello</h1>"
    };
  }
};
</script>
```

我们有通过`v-model`绑定到`content`状态的`vue-editor`组件。

它有一些常见的功能，比如粗体、斜体、下划线、不同风格的文本、列表、图像、视频、链接等等。

还有许多其他自定义，如占位符。

它还发出模糊、聚焦、添加图像、更改选择等事件。

我们也可以改变图片上传的方式。

# 真空旋转器

vue-spinner 是一个 spinner 组件，这样我们就可以在加载数据时显示一些内容。

要使用它，我们首先通过运行以下命令来安装它:

```
npm i vue-spinner
```

然后我们可以通过写来使用它:

```
<template>
  <div id="app">
    <pulse-loader loading :color="color"></pulse-loader>
  </div>
</template>

<script>
import PulseLoader from "vue-spinner/src/PulseLoader.vue";export default {
  components: {
    PulseLoader
  },
  data() {
    return {
      color: "green"
    };
  }
};
</script>
```

我们设置`loading`道具来表示它正在加载。

`color`设置颜色。

它有许多其他风格，如`grid-loader`、`clip-loader`等等。

# vue 2-下降区

vue2-dropzone 是一个 dropzone 组件，我们可以使用它向我们的 vue 应用程序添加文件上传输入。

为了使用它，我们运行:

```
npm i vue2-dropzone
```

然后我们可以通过写来使用它:

```
<template>
  <div id="app">
    <vue-dropzone ref="dropzone" id="dropzone" :options="dropzoneOptions"></vue-dropzone>
  </div>
</template>

<script>
import vue2Dropzone from "vue2-dropzone";
import "vue2-dropzone/dist/vue2Dropzone.min.css";
export default {
  name: "app",
  components: {
    vueDropzone: vue2Dropzone
  },
  data() {
    return {
      dropzoneOptions: {
        url: "https://example.com",
        thumbnailWidth: 150,
        maxFilesize: 0.5,
        headers: { "Header": "header value" }
      }
    };
  }
};
</script>
```

我们使用带有一些选项的`vue-dropzone`组件。

`url`是要上传到的 URL。

`thumbnailwidth`是缩略图的宽度。

`maxFilesize`是所选文件的最大文件大小，。

`headers`是我们随请求一起发送的头。

# vue-计数到

vue-count-to 是一个组件，让我们添加动画来向上或向下计数到一个特定的数字。

要安装它，我们运行:

```
npm i vue-count-to
```

然后我们可以通过写来使用它:

```
<template>
  <countTo :startVal="startVal" :endVal="endVal" :duration="3000"></countTo>
</template>

<script>
import countTo from "vue-count-to";
export default {
  components: { countTo },
  data() {
    return {
      startVal: 0,
      endVal: 2020
    };
  }
};
</script>
```

我们只是设置`startVal`道具来设置动画的起始值。

`endVal`设置动画的结束值。

`duration`是动画时长。

# vue-进度条

vue-progressbar 是一个进度条组件。

要使用它，首先我们通过运行以下命令来安装它:

```
npm i vue-progressbar
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueProgressBar from "vue-progressbar";const options = {
  color: "green",
  failedColor: "red",
  thickness: "5px",
  transition: {
    speed: "1.2s",
    opacity: "1.6s",
    termination: 300
  },
  autoRevert: true,
  location: "top",
  inverse: false
};Vue.use(VueProgressBar, options);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <vue-progress-bar></vue-progress-bar>
  </div>
</template>

<script>
export default {
  mounted() {
    this.$Progress.finish();
  },
  created() {
    this.$Progress.start();
  }
};
</script>
```

我们有带有为进度条设置的选项的`options`对象。

`color`是条形颜色。

`failedColor`是加载失败时的颜色。

`thickness`是棒材的粗细。

`transition`是动画的速度。

`opacity`是条的不透明度。

`termination`是它终止的时候。

`autoRevert`表示加载失败时自动反转。

`location`是酒吧的位置。

`inverse`正在反转方向。

![](img/c36c98d5ad2ccde9cd31c30d17899270.png)

[David Clode](https://unsplash.com/@davidclode?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

Vue2Editor 是富文本编辑器。

vue2-dropzone 是一个文件上传小工具。

vue-count-to 为我们提供了一种动画数字显示的方法。

vue-progressbar 让我们显示一个进度条。

vue-spinner 是一个我们可以添加的 spinner。