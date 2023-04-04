# 分页、星级、网格布局和日期选择器的顶级 Vue 包

> 原文：<https://blog.devgenius.io/top-vue-packages-for-pagination-star-rating-grid-layout-and-date-picker-95bb17f7d031?source=collection_archive---------33----------------------->

![](img/6937b03df1087e06e75623590b7784f9.png)

菲尔·古德温在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将研究如何为分页、星级输入、网格布局和日期选择器提供最好的包

# vue-分页-2

vue-pagination-2 是一个分页组件，我们可以用它来添加这些链接。

要使用它，我们通过运行以下命令来安装它:

```
npm i vue-pagination-2
```

然后我们可以通过写来使用它:

```
<template>
  <div>
    <pagination v-model="page" :records="300" @paginate="myCallback"></pagination>
  </div>
</template>

<script>
import Pagination from "vue-pagination-2";export default {
  components: {
    Pagination
  },
  data() {
    return {
      page: 2
    };
  },
  methods: {
    myCallback(e) {
      console.log(e);
    }
  }
};
</script>
```

我们添加了`pagination`组件，并用`pagination`绑定了具有`page`状态的链接。

`records`是记录的数量。

我们可以通过监听`paginate`事件来获取页码。

# vue 网格布局

我们可以使用 vue-grid-layout 来创建一个灵活的网格布局。

首先，我们通过运行以下命令安装它:

```
npm i vue-grid-layout
```

然后我们可以写:

```
<template>
  <div>
    <grid-layout
      :layout.sync="layout"
      :col-num="12"
      :row-height="30"
      :is-draggable="true"
      :is-resizable="true"
      :is-mirrored="false"
      :vertical-compact="true"
      :margin="[10, 10]"
      :use-css-transforms="true"
    >
      <grid-item
        v-for="item in layout"
        :x="item.x"
        :y="item.y"
        :w="item.w"
        :h="item.h"
        :i="item.i"
        :key="item.i"
      >{{item.i}}</grid-item>
    </grid-layout>
  </div>
</template>

<script>
import VueGridLayout from "vue-grid-layout";
const layout = [
  { x: 0, y: 0, w: 2, h: 2, i: "0" },
  { x: 2, y: 0, w: 2, h: 4, i: "1" },
  { x: 4, y: 0, w: 2, h: 5, i: "2" }
];export default {
  components: {
    GridLayout: VueGridLayout.GridLayout,
    GridItem: VueGridLayout.GridItem
  },
  data() {
    return {
      layout
    };
  }
};
</script>
```

去使用它。

我们设置网格的布局。

`x`是该项的水平位置，`y`是该项的垂直位置。

`i`是项目的唯一标识符。

`w`是宽度。

`h`是身高。

`is-draggable`使项目可拖动。

`is-resizable`使项目可调整大小。

`margin`添加水平和垂直边距。

# vue-日期时间

vue-datetime 是 vue 应用程序的日期时间选择器组件。

要安装它，我们运行:

```
npm i vue-datetime luxon
```

Luxon 是用于日期格式化的依赖项。

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import { Datetime } from "vue-datetime";
import "vue-datetime/dist/vue-datetime.css";Vue.component("datetime", Datetime);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <datetime v-model="date"></datetime>
    <p>{{date}}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      date: undefined
    };
  }
};
</script>
```

我们注册组件并使用`v-model`将选择日期绑定到`date`。

此外，我们必须记住包括捆绑的 CSS。

# vue-js-切换按钮

vue-js-toggle-button 是 vue 应用程序的一个切换按钮组件。

要使用它，我们通过运行以下命令来安装它:

```
npm i vue-js-toggle-button
```

然后我们可以写:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import ToggleButton from "vue-js-toggle-button";Vue.use(ToggleButton);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <toggle-button v-model="toggle"/>
    <p>{{toggle}}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      toggle: false
    };
  }
};
</script>
```

我们使用`toggle-button`组件来添加切换组件。

它通过`v-model`将触发值绑定到`toggle`。

我们也可以改变颜色或添加标签。

为了补充所有这些，我们写道:

```
<template>
  <div>
    <toggle-button v-model="toggle" color="green" :sync="true" :labels="true"/>
    <p>{{toggle}}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      toggle: false
    };
  }
};
</script>
```

# 星级评定

我们可以使用 vue-star-rating 小部件向我们的 vue 应用程序添加星级输入。

要使用它，我们通过运行以下命令来安装它:

```
npm i vue-star-rating
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import StarRating from "vue-star-rating";Vue.component("star-rating", StarRating);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <star-rating v-model="rating"></star-rating>
    <p>{{rating}}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      rating: 0
    };
  }
};
</script>
```

我们使用`star-rating`组件，并将选择的值与`v-model`绑定。

![](img/6baae28192576d2d5366b0298ed21091.png)

照片由[克里斯汀娜·帕普](https://unsplash.com/@almapapi?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 结论

vue-pagination-2 是一个分页小部件。

vue-grid-layout 让我们创建一个灵活的网格布局，可以在其中拖动项目和调整大小。

vue-js-toggle-button 是一个拨动开关组件。

vue-star-rating 是一个方便的星级输入。