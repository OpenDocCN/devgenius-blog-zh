# 用于添加图表、键盘快捷键和观看滚动的顶级 Vue 包

> 原文：<https://blog.devgenius.io/top-vue-packages-for-adding-charts-keyboard-shortcuts-and-watching-scrolling-9aecbbbd1e06?source=collection_archive---------21----------------------->

![](img/9751b04a892f44d58b09bd55a0ace8b3.png)

照片由[斯雷科·斯克罗比奇](https://unsplash.com/@sreckoskrobic?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将看看添加一个组件来用键盘跳转到一个项目、观察滚动和添加图表的最佳包。

# 跳转到

我们可以使用 vue-skip-to 包，通过按 Tab 键跳到我们想要的元素。

要使用它，我们通过运行以下命令来安装它:

```
npm i vue-skip-to
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueSkipTo from "vue-skip-to";Vue.use(VueSkipTo);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <vue-skip-to to="#num-100">skip</vue-skip-to>
    <div v-for="n in 100" :key="n" :id="`num-${n}`">{{n}}</div>
  </div>
</template>
<script>
export default {
  name: "app"
};
</script>
```

我们使用`vue-skip-to`组件，它让我们用给定的选择器移动到给定的元素。

我们可以通过按 Tab 键跳转到该元素。

# 滚动活动的

vue-scrollactive 可以让我们检测出滚动时哪个元素是可见的。

为了使用它，我们运行:

```
npm i vue-scrollactive
```

来安装它。

然后我们可以写:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueScrollactive from "vue-scrollactive";Vue.use(VueScrollactive);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <scrollactive v-on:itemchanged="onItemChanged">
      <a href="#home" class="scrollactive-item">Home</a>
      <a href="#about-us" class="scrollactive-item">About Us</a>
    </scrollactive>
    <div
      id="home"
    >Lorem ipsum dolor sit amet, consectetur adipiscing elit. Curabitur maximus lacus vel scelerisque placerat. Donec elementum ligula in ipsum gravida aliquam. Aenean feugiat risus eu est maximus, eu molestie nibh aliquet. Aliquam ipsum purus, pellentesque a rhoncus et, mattis imperdiet dui. Nunc ante quam, pretium at convallis at, finibus efficitur sem. Etiam venenatis nunc lacus, ac congue lacus luctus a. Maecenas pretium vestibulum ex, tristique commodo orci porta vel. Fusce auctor est nec lorem dignissim viverra. Nunc volutpat interdum eros a mattis.</div>
    <div
      id="about-us"
    >Nunc ac bibendum turpis, vitae semper tortor. Nullam leo lorem, euismod sit amet vulputate non, ullamcorper at dolor. In hac habitasse platea dictumst. Nulla tincidunt non quam vel viverra. Suspendisse ut augue vitae elit semper volutpat. Ut non porttitor risus, in condimentum orci. Aliquam erat volutpat. Aenean accumsan, leo et pulvinar elementum, quam dui pharetra libero, a mattis diam nibh et velit. Fusce ultrices massa mi, ut lobortis mi condimentum nec. In eget lectus tincidunt, imperdiet ipsum quis, euismod velit. Curabitur sodales nibh et enim efficitur, quis rhoncus ante semper. Donec id blandit nisl, vitae pharetra ante.</div>
  </div>
</template>
<script>
export default {
  name: "app",
  methods: {
    onItemChanged(event, currentItem, lastActiveItem) {
      console.log(event, currentItem, lastActiveItem);
    }
  }
};
</script><style>
#home,
#about-us {
  height: 500px;
}
</style>
```

去使用它。

我们使用`scrollactive`组件来观察滚动。

它将观察在`href`中用 ID 设置的元素。

因此，当我们滚动到`home`和`about-us`div 时，我们将看到`onItemChanged`方法运行。

`itemchanged`滚动时发出事件。

# 高图表-Vue

highcharts-vue 是一个 vue 应用程序的图表库。

为了使用它，我们运行:

```
npm i highcharts-vue highcharts
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import HighchartsVue from "highcharts-vue";Vue.use(HighchartsVue);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <highcharts :options="chartOptions"></highcharts>
</template>

<script>
export default {
  data() {
    return {
      chartOptions: {
        series: [
          {
            data: [1, 2, 3]
          }
        ]
      }
    };
  }
};
</script>
```

我们使用`highcharts`组件。然后我们使用`options` prop 来显示我们想要显示的数据。

`data`是我们要显示的 y 轴上的数值。

我们可以通过编写以下内容来设置标题和轴标签:

```
<template>
  <highcharts :options="chartOptions"></highcharts>
</template>

<script>
export default {
  data() {
    return {
      chartOptions: {
        title: {
          text: "Fruit Consumption"
        },
        xAxis: {
          title: {
            text: "Fruit Number"
          },
          tickInterval: 1,
          categories: ["2020-01-01", "2020-01-02", "2020-01-03"]
        },
        yAxis: {
          title: {
            text: "Fruit eaten"
          },
          tickInterval: 1
        },
        series: [
          {
            name: "alex",
            data: [1, 0, 4]
          },
          {
            name: "james",
            data: [5, 7, 3]
          }
        ]
      }
    };
  }
};
</script>
```

`title`有标题文字。

`categories`有 x 轴标签。

`yAxis`具有 y 轴文本，包括带标签的`text`。

让我们绘制一条或多条线。

`data`是 y 轴数值。

![](img/6c7af1e5697846caa6e860c2c5203dce.png)

威尔·帕特森在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

vue-skip-to 允许用户用键盘跳到一个元素。

vue-scrollactive 让我们观看滚动到某个元素。

这是一个简单易用的图表库