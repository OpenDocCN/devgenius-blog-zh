# 用于添加看板和分页按钮的顶级 Vue 包

> 原文：<https://blog.devgenius.io/top-vue-packages-for-adding-a-kanban-board-and-pagination-buttons-8e1c815709a4?source=collection_archive---------25----------------------->

![](img/29edeb9722d8bc38b555c8a68f1ace28.png)

由 [kishan bishnoi](https://unsplash.com/@kishanbishnoi?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将看看添加看板和分页按钮的最佳包。

# vue-看板

我们可以使用 vue-kanban 组件将看板添加到我们的 Vue 应用程序中。

看板中的项目可以根据其状态拖放到不同的列中。

要安装它，我们运行:

```
npm i vue-kanban
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import vueKanban from "vue-kanban";Vue.use(vueKanban);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`styles.css`

```
ul.drag-list,
ul.drag-inner-list {
  list-style-type: none;
  margin: 0;
  padding: 0;
}.drag-container {
  max-width: 1000px;
  margin: 20px auto;
}.drag-list {
  display: flex;
  align-items: flex-start;
}
[@media](http://twitter.com/media) (max-width: 500px) {
  .drag-list {
    display: block;
  }
}.drag-column {
  flex: 1;
  margin: 0 10px;
  position: relative;
  background: rgba(0, 0, 0, 0.2);
  overflow: hidden;
}
@media (max-width: 500px) {
  .drag-column {
    margin-bottom: 30px;
  }
}
.drag-column h2 {
  font-size: 0.8rem;
  margin: 0;
  text-transform: uppercase;
  font-weight: 600;
}.drag-column-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px;
}.drag-inner-list {
  min-height: 50px;
  color: white;
}.drag-item {
  padding: 10px;
  margin: 10px;
  height: 100px;
  background: gray;
}
.drag-item.is-moving {
  transform: scale(1.5);
  background: lightgray;
}.drag-header-more {
  cursor: pointer;
}.drag-options {
  position: absolute;
  top: 44px;
  left: 0;
  width: 100%;
  height: 100%;
  padding: 10px;
  transform: translateX(100%);
  opacity: 0;
}
.drag-options.active {
  transform: translateX(0);
  opacity: 1;
}
.drag-options-label {
  display: block;
  margin: 0 0 5px 0;
}
.drag-options-label input {
  opacity: 0.6;
}
.drag-options-label span {
  display: inline-block;
  font-weight: 400;
  margin-left: 5px;
}.gu-mirror {
  position: fixed !important;
  margin: 0 !important;
  z-index: 9999 !important;
  opacity: 0.8;
  list-style-type: none;
}.gu-hide {
  display: none !important;
}.gu-unselectable {
  -webkit-user-select: none !important;
  -moz-user-select: none !important;
  -ms-user-select: none !important;
  user-select: none !important;
}.gu-transit {
  opacity: 0.2;
}
```

这些样式源自位于[https://github . com/Brock reee/vue-kanban/blob/master/src/assets/kanban . CSS](https://github.com/BrockReece/vue-kanban/blob/master/src/assets/kanban.css)的起始样式。

`App.vue`

```
<template>
  <div>
    <kanban-board :stages="stages" :blocks="blocks"></kanban-board>
  </div>
</template><script>
export default {
  data() {
    return {
      stages: ["not-started", "in-progress", "needs-review", "approved"],
      blocks: [
        {
          id: 1,
          status: "not-started",
          title: "Test"
        }
      ]
    };
  }
};
</script><style src='./styles.css'>
</style>
```

我们需要 CSS 来设计看板的样式，因为它没有样式。

在`App.vue`中，我们传入的`stages`是一个字符串数组。

`blocks`是带有`id`、`status`和`title`的对象的数组。

# vue js-分页

vuejs-paginate 是一个分页按钮组件。

要安装它，我们运行:

```
npm i vuejs-paginate
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import Paginate from "vuejs-paginate";
Vue.component("paginate", Paginate);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <div>page {{pageNum}}</div>
    <paginate
      :page-count="20"
      :click-handler="onClick"
      :prev-text="'Prev'"
      :next-text="'Next'"
      :container-class="'className'"
    ></paginate>
  </div>
</template><script>
export default {
  data() {
    return {
      pageNum: 0
    };
  },
  methods: {
    onClick(pageNum) {
      this.pageNum = pageNum;
    }
  }
};
</script>
```

我们在`main.js`中注册插件。

然后，我们通过传递一些支持导航的道具来添加`paginate`组件。

`page-count`有页数。

`click-handler`是点击分页按钮时运行的函数。

`prev-text`有上一步按钮的文本。

`next-text`有下一步按钮的文本。

`container-class`有分页按钮容器的类名。

现在我们将看到一个按钮列表，我们可以单击它转到我们想要的页面。

![](img/48630b44047890ba309ed0f121690432.png)

菲利普·莱昂在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以使用 vue-kanban 在我们的应用程序中添加看板。

vuejs-paginate 插件允许我们为任何东西添加分页按钮。