# 用于添加日历、平滑滚动、查询构建器和图表的顶级 Vue 包

> 原文：<https://blog.devgenius.io/top-vue-packages-for-adding-calendars-smooth-scrolling-query-builder-and-charts-1b62084dc961?source=collection_archive---------16----------------------->

![](img/e54424f2e7c2805f25b8412e30726c09.png)

由 [Patrick Schneider](https://unsplash.com/@patrick_schneider?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将看看添加日历、平滑滚动、查询构建器 UI 和图表的最佳包。

# vue-平滑滚动

vue-smoothscroll 让我们可以给我们的 vue 应用添加平滑滚动效果。

为了使用它，我们运行:

```
npm i vue-smoothscroll
```

然后我们写道:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
const vueSmoothScroll = require("vue-smoothscroll");
Vue.use(vueSmoothScroll);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div class="root">
    <div v-smoothscroll="{ duration : 500, callback: callback, axis :'y' }">
      <div :ref="`num-${n}`" v-for="n in 100" :key="n">{{n}}</div>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    callback() {
      console.log("scrolling");
    }
  }
};
</script>
```

去使用它。

我们使用`v-smoothscroll`指令来启用滚动。

当我们滚动时，将运行`callback`。

`axis`指定我们正在滚动的轴。

`duration`是我们滚动的持续时间。

# Vue 查询生成器

Vue Query Builder 是一个组件，它允许我们将查询构建器 UI 添加到我们的应用程序中。

要使用它，我们首先通过运行以下命令来安装它:

```
npm install vue-query-builder
```

然后我们通过书写来使用它:

```
<template>
  <div class="root">
    <vue-query-builder v-model="query" :rules="rules"></vue-query-builder>
    <p>{{query}}</p>
  </div>
</template>

<script>
import VueQueryBuilder from "vue-query-builder";export default {
  data() {
    return {
      rules: [
        {
          type: "text",
          id: "vegetable",
          label: "Vegetable"
        },
        {
          type: "radio",
          id: "fruit",
          label: "Fruit",
          choices: [
            { label: "apple", value: "apple" },
            { label: "orange", value: "orange" }
          ]
        }
      ],
      query: undefined
    };
  },
  components: { VueQueryBuilder }
};
</script>
```

我们使用`vue-query-builder`组件来添加查询构建器 UI。

我们指定规则，以便控件显示这些内容。

`choices`显示为选择。`radio`按钮显示广告单选按钮。

`type`显示在匹配类型下拉列表中。

我们可以根据需要添加规则和组。

# vue-schart

vue-schart 是一个易于使用的图表库。

我们可以通过安装它来使用它:

```
npm i vue-schart
```

然后我们写道:

```
<template>
  <div id="app">
    <schart class="wrapper" canvasId="canvas" :options="options"/>
  </div>
</template>
<script>
import Schart from "vue-schart";
export default {
  data() {
    return {
      options: {
        type: "bar",
        title: {
          text: "Sales"
        },
        bgColor: "white",
        labels: ["apple", "orange", "grape"],
        datasets: [
          {
            label: "January sales",
            fillColor: "green",
            data: [474, 281, 482]
          },
          {
            label: "February sales",
            data: [164, 178, 190]
          }
        ]
      }
    };
  },
  components: {
    Schart
  }
};
</script><style>
.wrapper {
  width: 80vw;
  height: 300px;
}
</style>
```

创建条形图。

我们使用`schart`组件。

此外，我们指定画布的 ID 来设置画布的 ID。

`options`拥有包括数据在内的所有图表选项。

`type`是图表类型。

`title`是图表标题。

`bgColor`是背景色。

`labels`是 x 轴标签。

`dataasets`拥有我们所有的数据集。

`label`有图例的标签。

`fillColor`是条形的填充颜色。

`data`具有 y 轴的值。

# vue-日历-组件

vue 日历组件是一个可定制的 Vue 日历组件。

为了使用它，我们运行:

```
npm i vue-calendar-component
```

来安装它。

然后我们可以通过写来使用它:

```
<template>
  <div id="app">
    <Calendar :textTop="textTop" v-on:choseDay="clickDay" v-on:changeMonth="changeDate"></Calendar>
  </div>
</template>
<script>
import Calendar from "vue-calendar-component";export default {
  components: {
    Calendar
  },
  data() {
    return {
      textTop: ["M", "T", "W", "T", "F", "S", "S"]
    };
  },
  methods: {
    clickDay(data) {
      console.log(data);
    },
    changeDate(data) {
      console.log(data);
    },
    clickToday(data) {
      console.log(data);
    }
  }
};
</script>
```

我们使用`Calendar`组件来显示日历。

让我们更改星期几的文本。

当从日历中选择一天或一周时，它也会发出事件。

此外，它还需要许多其他类型的定制。比如把开始时间从周一改为周日。

我们还可以使用 assign a ref 来以编程方式导航日历。

![](img/e092067ff9936db202766f8b3cc29fb1.png)

由[卢卡斯·布拉塞克](https://unsplash.com/@goumbik?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

# 结论

vue-smoothscroll 允许我们在 vue 应用程序中添加平滑滚动。

Vue 查询生成器是一个为我们提供查询生成器 UI 的组件。

vue-schart 是一个易于使用的图表组件。

vue-calendar-component 是 vue 应用程序的日历组件。