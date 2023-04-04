# 用于添加拖放、动画和去抖动的顶级 Vue 包

> 原文：<https://blog.devgenius.io/top-vue-packages-for-adding-drag-and-drop-animation-and-debouncing-e00594c4daf3?source=collection_archive---------15----------------------->

![](img/c233eb48adebe53321b33def1c5020f2.png)

[吉米·张](https://unsplash.com/@photohunter?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将看看添加拖放元素、动画和去抖动的最佳包。

# Vue Slicksort

Vue Slicksort 是一个易于使用的包，让我们可以在我们的 Vue 应用程序中快速创建可排序的列表。

为了使用它，我们运行:

```
npm i vue-slicksort
```

来安装它。

然后我们写道:

```
<template>
  <div class="root">
    <SlickList lockAxis="y" v-model="items">
      <SlickItem v-for="(item, index) in items" :index="index" :key="index">{{ item }}</SlickItem>
    </SlickList>
  </div>
</template>

<script>
import { SlickList, SlickItem } from "vue-slicksort";export default {
  components: {
    SlickItem,
    SlickList
  },
  data() {
    return {
      items: Array(20).fill().map((_, i)=> `item ${i}`)
    };
  }
};
</script>
```

去使用它。

我们使用`SlickList`项来创建容器。

然后我们将`SlickItem`组件放入其中以显示可排序列表。

现在我们可以在列表中拖放项目。

# 武-德拉古拉

vue-dragula 是另一个易于使用的 vue 应用程序拖放库。

为了使用它，我们运行:

```
npm i vue-dragula
```

然后我们写道:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
const VueDragula = require("vue-dragula");Vue.use(VueDragula);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div class="root">
    <div class="container" v-dragula="colOne" bag="first-bag">
      <div v-for="text in colOne" :key="text">{{text}}</div>
    </div>
    <div class="container" v-dragula="colTwo" bag="first-bag">
      <div v-for="text in colTwo" :key="text">{{text}}</div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      colOne: Array(15)
        .fill()
        .map((_, i) => `item ${i}`),
      colTwo: Array(15)
        .fill()
        .map((_, i) => `item ${i + 20}`)
    };
  }
};
</script>
```

我们用`v-dragula`指令创建了两个可拖动列表。

我们将`bag`道具设置为相同的名称，以便我们可以在它们之间拖放。

在每个容器中，我们呈现可拖动的元素。

# vue-dragscroll

vue-dragscroll 是一个指令，让我们通过拖动鼠标来启用滚动。

为了使用它，我们运行:

```
npm i vue-dragscroll
```

来安装它。

然后我们写道:

```
<template>
  <div class="root">
    <div v-dragscroll id="scroll">
      <div v-for="i in items" :key="i">{{i}}</div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      items: Array(50)
        .fill()
        .map((_, i) => `item ${i}`)
    };
  }
};
</script><style>
#scroll {
  height: 300px;
  overflow-y: scroll;
}
</style>
```

通过点击并按住鼠标移动来创建一个可滚动的 div。

我们可以使用`x`和`y`修饰符来启用或禁用一个方向的拖动滚动。

此外，我们可以禁用子元素的拖动滚动。

# Vue 动画 CSS

Vue Animate CSS 允许我们在应用程序中添加动画效果，而无需我们自己从头开始创建。

我们可以通过运行以下命令来安装它:

```
npm i v-animate-css
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VAnimateCss from "v-animate-css";Vue.use(VAnimateCss);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div class="root">
    <div v-animate-css="'fadeIn'">foo</div>
  </div>
</template>

<script>
export default {};
</script>
```

我们通过使用带有字符串`'fadeIn'`的`v-animate-css`指令来添加淡入动画效果。

通过这个库，我们可以将许多其他效果添加到我们的应用程序中。

# 去抖

我们可以使用 vue-debounce 来延迟回调的运行。

为了使用它，我们运行:

```
npm i vue-debounce
```

来安装它。

然后我们写道:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import vueDebounce from "vue-debounce";Vue.use(vueDebounce);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div class="root">
    <input v-debounce:300ms="log" type="text">
  </div>
</template>

<script>
export default {
  methods: {
    log(e){
      console.log(e)
    }
  }
};
</script>
```

我们使用带有`300ms`修饰符的`v-debounce`指令在运行`log`方法之前将其延迟 300 毫秒。

然后我们可以在运行它的时候从它那里得到输入的值。

![](img/a18a823af4e0f21b6377f6ef5a99df7d.png)

[Levi XU](https://unsplash.com/@xusanfeng?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

Vue Slicksort 和 vue-dragula 允许我们在应用程序中添加可拖动的元素。

Vue Animate CSS 是一个库，可以让我们添加 CSS 动画效果，而无需从头开始编写。

vue-debounce 是一个延迟回调的库。