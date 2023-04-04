# 用于添加上下文菜单、管理会话存储等的顶级 Vue 包

> 原文：<https://blog.devgenius.io/top-vue-packages-for-adding-context-menu-managing-session-storage-and-more-300fedcb5559?source=collection_archive---------16----------------------->

![](img/d1477a1aa80cb1ed746134e5746b462b.png)

照片由[沃尔夫冈·哈塞尔曼](https://unsplash.com/@wolfgang_hasselmann?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在这篇文章中，我们将看看用于附加元素、管理会话存储、添加可拖动元素、分页和上下文菜单的最佳包。

# 动词词缀

我们可以使用 vue-affix 包将一个元素放在另一个元素之前。

为了使用它，我们运行:

```
npm i vue-affix
```

然后我们可以通过写来使用它:

```
<template>
  <div>
    <affix class="sidebar" relative-element-selector="#section">
      <p>foo</p>
      <p>bar</p>
      <p>baz</p>
    </affix>
    <section id="section">
      <p>affix element</p>
    </section>
  </div>
</template>

<script>
import { Affix } from "vue-affix";export default {
  components: {
    Affix
  }
};
</script>
```

我们将`relative-element-selector`属性设置为我们想要将元素放在前面的选择器。

# vue 上下文菜单

我们可以使用 vue-context-menu 来显示上下文菜单

为了使用它，我们写:

```
npm i vue-context-menu
```

然后我们可以通过写来使用它:

```
<template>
  <div>
    <div @contextmenu.prevent="$refs.ctxMenu.open">right click</div>
    <context-menu id="context-menu" ref="ctxMenu">
      <li>option 1</li>
      <li>option 2</li>
    </context-menu>
  </div>
</template>

<script>
import contextMenu from "vue-context-menu";
export default {
  components: { contextMenu }
};
</script>
```

我们监听`contextmenu`事件并运行`$refs.ctxMenu.open`来打开上下文菜单。

在`context-menu`组件中，我们将 ref 设置为`ctxMenu`，这样它就可以被打开。

我们可以在上下文菜单中放入任何东西。

# vue 会话

我们可以使用 vue 会话包来管理会话存储。

为了使用它，我们运行:

```
npm i vue-session
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueSession from "vue-session";
Vue.use(VueSession);
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
  mounted() {
    this.$session.set("foo", "bar");
  }
};
</script>
```

我们使用`this.$session`属性在会话存储中设置一个值。

我们也可以使用`get`通过键来获取值。

`getAll`获取所有值。

`colear`删除所有按键。

`destroy`销毁会话。

`renew`更新会话。

`start`开始新的会话。

`exists`检查密钥是否存在。

# 可拖动指令

draggable-vue-directive 让我们可以轻松地创建一个可拖动的组件。

为了使用它，我们运行:

```
npm i draggable-vue-directive
```

来安装它。

然后我们写道:

```
<template>
  <div>
    <div v-draggable>drag me</div>
  </div>
</template>

<script>
import { Draggable } from "draggable-vue-directive";export default {
  directives: {
    Draggable
  }
};
</script>
```

创建基本的可拖动元素。

我们所要做的就是使用`v-draggable`指令。

此外，我们可以为它设置一个值并访问它:

```
<template>
  <div>
    <div v-draggable="draggableValue">drag me</div>
  </div>
</template>

<script>
import { Draggable } from "draggable-vue-directive";export default {
  directives: {
    Draggable
  },
  data() {
    return {
      draggableValue: {
        onPositionChange: this.onPosChanged
      }
    };
  },
  methods: {
    onPosChanged(positionDiff, absolutePosition, event) {
      console.log(positionDiff, absolutePosition);
    }
  }
};
</script>
```

`draggableValue`对象可以有各种方法，当对可拖动元素执行操作时调用这些方法。

`onPositionChange`当可拖动元素的位置改变时运行。

`positionDiff`是与前一个位置的差异。

`absolutePosition`是当前位置。

我们可以放入对象的属性包括`handle`、`onDragEnd`、`resetInitialPos`、`initialPosition`等等。

# vue-分页

我们可以使用 vue-paginate 包对任何内容进行分页。

为了使用它，我们运行:

```
npm i vue-paginate
```

然后我们可以通过写来使用它:

```
<template>
  <div>
    <paginate name="fruits" :list="fruits" :per="2">
      <li v-for="fruit in paginated('fruits')" :key="fruit">{{ fruit }}</li>
    </paginate>
    <paginate-links for="fruits"></paginate-links>
  </div>
</template>

<script>
export default {
  data() {
    return {
      fruits: ["apple", "orange", "grape", "banana", "pear"],
      paginate: ["fruits"]
    };
  }
};
</script>
```

`paginate`有分页的条目。

`per`每页有条目。

我们有要在标签中显示的项目。

并且`pagination-links`有访问每个页面的链接。

现在我们可以看到访问每个页面的链接，以及每个页面上显示的项目。

![](img/e708de3e404cfc1b7827acd6875a8af3.png)

马修·斯皮特里在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

vue-affix 允许我们添加一个元素作为另一个元素的兄弟。

vue-context-menu 允许我们向我们的 vue 应用程序添加上下文菜单。

vue-paginate 是一个我们可以使用的分页组件。

draggable-vue-directive 允许我们创建可拖动的元素。

vue-session 让我们可以管理会话存储。