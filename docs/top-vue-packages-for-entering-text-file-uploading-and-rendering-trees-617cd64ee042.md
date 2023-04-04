# 用于输入文本、上传文件和渲染树的顶级 Vue 包

> 原文：<https://blog.devgenius.io/top-vue-packages-for-entering-text-file-uploading-and-rendering-trees-617cd64ee042?source=collection_archive---------14----------------------->

![](img/413df9d7cf38c97a522b6bea57e31ac6.png)

照片由[阿达什·库姆穆尔](https://unsplash.com/@akummur?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将看看添加可拖动和可调整大小的元素、文件上传、文本编辑器、显示树结构和输入掩码的最佳包。

# v 文件上传

我们可以使用 v-file-upload 包将文件上传组件添加到我们的 Vue 应用程序中。

为了使用它，我们运行:

```
npm i v-file-upload
```

来安装它。

然后我们可以写:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";import FileUpload from "v-file-upload";Vue.use(FileUpload);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <file-upload :url="url" :thumb-url="thumbUrl" :headers="headers" @change="onFileChange"></file-upload>
</template>

<script>
import Vue from "vue";export default {
  data() {
    return {
      url: "http://upload.url",
      headers: { "access-token": "<your-token>" },
      filesUploaded: []
    };
  },
  methods: {
    thumbUrl(file) {
      return file.thumnbnail;
    },
    onFileChange(file) {
      this.fileUploaded = file;
    }
  }
};
</script>
```

去使用它。

我们使用`file-upload`组件。

我们设置`url`道具来设置上传 URL。

`headers`有由头。

`thumbUrl`有缩略图的网址。

现在我们在屏幕上看到一个文件选择器。

# vue-jstree

vue-jstree 是 vue 应用的树形视图。

为了使用它，我们运行:

```
npm i vue-jstree
```

来安装它。

然后我们写道:

```
<template>
  <div>
    <v-jstree :data="data"></v-jstree>
  </div>
</template>

<script>
import VJstree from "vue-jstree";export default {
  components: {
    VJstree
  },
  data() {
    return {
      data: [
        {
          text: "apple",
          children: [
            {
              text: "orange",
              selected: true
            },
            {
              text: "danger",
              icon: "fa fa-warning icon-state-danger"
            }
          ]
        },
        {
          text: "grape"
        }
      ]
    };
  }
};
</script>
```

去使用它。

我们使用`v-jstree`将树形视图包含在我们的组件中。

节点由数组定义。

每个条目都是一个带有文本的`text`属性的对象。

`selected`是表示选择的可选属性。

`icon` os 图标的类。

`children`是给孩子们上的课。

我们可以改变图标类。

这需要一些道具。

`show-checkbox`表示是否显示复选框。

`multiple`让我们选择多个项目。

`collapse`将所有树节点设置为折叠状态。

`loading-text`是加载功能。

它需要更多的道具。

# Vue-Trix 文本编辑器

Vue-Trix 文本编辑器是一个丰富的文本编辑器，我们可以在我们的 Vue 应用程序中使用。,

为了使用它，我们运行:

```
npm i vue-trix
```

安装软件包。

然后我们写道:

```
<template>
  <div>
    <VueTrix v-model="editorContent" placeholder="Enter content" localStorage/>
  </div>
</template>

<script>
import VueTrix from "vue-trix";export default {
  components: {
    VueTrix
  },
  data(){
    return {
      editorContent: ''
    }
  }
};
</script>
```

添加编辑器。

我们添加`VueTrix`组件，将其添加到我们的应用程序中。

我们为`placeholder`设置一个值来添加一个占位符。

`v-model`将输入值绑定到`editorContent`状态。

`localStorage`将输入的文本保存到本地存储器。

# vue-拖动-调整大小

vue-drag-resize 是一个易于使用的插件，让我们添加可拖动和可调整大小的元素。

我们通过运行以下命令来安装它:

```
npm i vue-drag-resize
```

然后我们写道:

```
<template>
  <div id="app">
    <VueDragResize isActive :w="200" :h="200" v-on:resizing="resize" v-on:dragging="resize">
      <p>{{ top }} х {{ left }}</p>
      <p>{{ width }} х {{ height }}</p>
    </VueDragResize>
  </div>
</template>

<script>
import VueDragResize from "vue-drag-resize";export default {
  name: "app", components: {
    VueDragResize
  }, data() {
    return {
      width: 0,
      height: 0,
      top: 0,
      left: 0
    };
  }, methods: {
    resize(newRect) {
      this.width = newRect.width;
      this.height = newRect.height;
      this.top = newRect.top;
      this.left = newRect.left;
    }
  }
};
</script>
```

去使用它。

我们使用`VueDraggable`组件来创建一个可拖动和可调整大小的组件。

`isActive`将组件设置为活动状态。

我们用`h`道具设定初始高度。

`w`道具是初始宽度。

我们监听`resizing`和`dragging`事件来获取新维度的值。

![](img/6809799491a032bb593b5ddde4f04c22.png)

克里斯·莱贾拉祖在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

v-file-upload 是一个文件上传组件。

vue-jstree 让我们显示树结构。

Vue-Trix 文本编辑器是一个用于 Vue 应用程序的富文本编辑器。

vue-drag-resize 允许我们拖动元素并调整其大小。