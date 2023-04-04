# 用于添加文本编辑器、格式化日期、日历等的顶级 Vue 包

> 原文：<https://blog.devgenius.io/top-vue-packages-for-adding-text-editors-formatting-dates-calendars-and-more-36357b4b557c?source=collection_archive---------31----------------------->

![](img/98c4964e5a0e72e7f7b69074fb93e78f.png)

[埃斯特·扬森斯](https://unsplash.com/@esteejanssens?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在这篇文章中，我们将看看添加文本编辑器、格式化日期、日历等等的最佳包。

# vue-cal

vue-cal 是 vue 应用程序的一个易于使用的日历组件。

要使用它，我们通过运行以下命令来安装它:

```
npm i vue-cal
```

然后我们可以通过写来使用它:

```
<template>
  <div>
    <vue-cal hide-view-selector :time="false" active-view="month" xsmall></vue-cal>
  </div>
</template>

<script>
import VueCal from "vue-cal";
import "vue-cal/dist/vuecal.css";export default {
  components: { VueCal }
};
</script>
```

我们导入了 CSS 并注册了组件。

现在，由于有了`vue-cal`组件，我们可以显示一个日历。

`x-small`缩小日历表格。

`time`表示我们是否显示时间。

`active-view`设置活动视图。`month`表示我们显示月历。

我们可以设置最小和最大日期。

可以设置日期范围:

```
<template>
  <div>
    <vue-cal
      :min-date="minDate"
      :max-date="maxDate"
      hide-view-selector
      :time="false"
      active-view="month"
      xsmall
    ></vue-cal>
  </div>
</template>

<script>
import VueCal from "vue-cal";
import "vue-cal/dist/vuecal.css";export default {
  computed: {
    minDate() {
      return new Date().subtractDays(50);
    },
    maxDate() {
      return new Date().addDays(50);
    }
  },
  components: { VueCal }
};
</script>
```

它还支持本地化。

# vue-uid

vue-uid 是一个在 vue 组件中生成唯一 id 的包

要安装它，我们运行:

```
npm i vue-uid
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueUid from "vue-uid";Vue.use(VueUid);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>{{$_uid}}</div>
</template>

<script>
export default {};
</script>
```

我们只是使用注册组件后可用的`$_uid`变量来使用它。

它也可以在组件中注册为 mixin。

# vue 2 编辑器

Vue2Editor 是 Vue 应用程序的富文本编辑器。

要使用它，我们可以通过运行以下命令来安装它:

```
npm i vue2-editor
```

然后我们可以通过写来使用它:

```
<template>
  <div id="app">
    <vue-editor v-model="content"></vue-editor>
    <div v-html="content"></div>
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

我们所要做的就是使用`vue-editor`组件。

它有一些共同的特征，如粗体、斜体、列表、下划线、图像、视频插入、添加链接等。

`v-model`用`content`绑定输入到编辑器中的 HTML 内容。

它发出各种我们可以听到的事件。

此外，它支持多种定制。

# @handsontable/vue

@handsontable/vue 在我们的 vue 应用中为我们提供了基本的电子表格功能。

为了使用它，我们写:

```
npm i @handsontable/vue handsontable
```

它是`handsontable`的 Vue 版本，所以它是一个必需的依赖项。

然后我们可以通过写来使用它:

```
<template>
  <hot-table :data="data" colHeaders="year" rowHeaders="fruit" width="600" height="300"></hot-table>
</template>

<script>
import { HotTable } from "@handsontable/vue";export default {
  data: function() {
    return {
      data: [
        ["", "apple", "orange", "grape", "banana"],
        ["2019", 10, 11, 12, 13],
        ["2020", 20, 11, 14, 13],
        ["2021", 30, 15, 12, 13]
      ]
    };
  },
  components: {
    HotTable
  }
};
</script><style lang='scss'>
@import "~handsontable/dist/handsontable.full.css";
</style>
```

我们用`data`数组来存储数据。

`hot-table`是我们用来显示数据网格的组件。

`colHeaders`有列标题。

`rowHeaders`有行标题。

我们还可以设置宽度和高度。

此外，我们还引进了一些款式。

现在我们有了一个网格，可以像电子表格一样导航。

如果我们不想显示缺少许可证密钥的消息，则需要许可证密钥才能使用此包。

非商业用途免费。

它有许多其他电子表格功能，如过滤数据、合并单元格、自动填充等。

# vue 日期 fns

vue-date-fns 为我们提供了许多格式化日期的过滤器。

为了使用它，我们运行:

```
npm i vue-date-fns
```

来安装它。

然后我们可以用它来写:

```
<template>
  <div>{{ new Date() | date }}</div>
</template><script>
import { dateFilter } from "vue-date-fns";export default {
  filters: {
    date: dateFilter
  }
};
</script>
```

我们用`date`过滤器显示日期。

我们可以在组件中使用带有`this.$date`的过滤器。

也可以自定义区域设置。

![](img/a9b4aeceecf84846a5da4521c2ebae75.png)

Photo by [五玄土 ORIENTO](https://unsplash.com/@oriento?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

vue-date-fns 为我们提供了 vue 过滤器来格式化日期。

@handsontable/vue 允许我们在 vue 应用程序中添加电子表格。

vue-cal 是 vue 的一个日历组件。

vue-uid 允许我们在 vue 组件中创建唯一的 id

Vue2Editor 是一个富文本编辑器，有很多特性。