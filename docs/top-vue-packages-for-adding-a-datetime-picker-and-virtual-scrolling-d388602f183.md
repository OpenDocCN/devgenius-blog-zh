# 用于添加日期时间选择器和虚拟滚动的顶级 Vue 包

> 原文：<https://blog.devgenius.io/top-vue-packages-for-adding-a-datetime-picker-and-virtual-scrolling-d388602f183?source=collection_archive---------19----------------------->

![](img/b7afadf0cbd4defd8444703509ba7350.png)

由 [Wiktor Karkocha](https://unsplash.com/@rotkif?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在这篇文章中，我们将看看如何最好的包添加一个日期和时间选择器，以及一个虚拟滚动列表。

# Vue 日期时间选择器

Vue 日期选择器允许我们在 Vue 应用程序中添加日期和时间选择器。

要安装它，我们运行:

```
npm i vue-vanilla-datetime-picker
```

现在我们可以通过书写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import DateTimePicker from "vue-vanilla-datetime-picker";Vue.component("date-time-picker", DateTimePicker);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <date-time-picker v-model="datetime"></date-time-picker>
  </div>
</template><script>
export default {
  data() {
    return {
      datetime: {}
    };
  }
};
</script><style lang='scss'>
@import "../../../node_modules/vue-vanilla-datetime-picker/dist/DateTimePicker";
</style>
```

我们在`main.js`注册组件。

然后我们从组件的包中导入样式。

此外，我们使用`date-time-picker`让用户选择日期。

`v-model`让我们将选择保存到`datetime`状态。

它有许多插槽，让我们自定义日期时间选择器的任何部分。

# vue 2-日期选择器

vue2-datepicker 是另一个日期选择器包，我们可以用它来添加一个。

要使用它，首先我们通过运行以下命令来安装它:

```
npm i vue2-datepicker
```

然后我们通过书写来使用它:

```
<template>
  <div>
    <date-picker v-model="time" valuetype="format"></date-picker>
  </div>
</template><script>
import DatePicker from "vue2-datepicker";
import "vue2-datepicker/index.css";export default {
  components: { DatePicker },
  data() {
    return {
      time: null
    };
  }
};
</script>
```

我们添加`date-picker`来绑定到`v-model`来使用它。

它还可以让我们选择日期和时间。

我们可以写:

```
<template>
  <div>
    <date-picker v-model="time" type="datetime"></date-picker>
  </div>
</template>
<script>
import DatePicker from "vue2-datepicker";
import "vue2-datepicker/index.css";
export default {
  components: { DatePicker },
  data() {
    return {
      time: null
    };
  }
};
</script>
```

为了做到这一点。

我们把`type`道具改成了`datetime`。

我们还可以更改日期选择器，让我们一次选择开始和结束日期。

所以我们可以写:

```
<template>
  <div>
    <date-picker v-model="time" range></date-picker>
  </div>
</template>
<script>
import DatePicker from "vue2-datepicker";
import "vue2-datepicker/index.css";
export default {
  components: { DatePicker },
  data() {
    return {
      time: null
    };
  }
};
</script>
```

我们用`range`道具做到了这一点。

它为我们提供了许多其他选项，如弹出样式、步骤、日期和时间格式、类名等等。

# 虚拟滚动列表

vue-virtual-scroll-list 允许我们添加一个虚拟滚动列表，以便在显示列表项时只显示它们。

要使用它，我们首先通过运行以下命令来安装它:

```
npm i vue-virtual-scroll-list
```

然后我们可以用它来写:

`App.vue`

```
<template>
  <div>
    <virtual-list
      style="height: 360px; overflow-y: auto;"
      :data-key="'uid'"
      :data-sources="items"
      :data-component="itemComponent"
      :extra-props="{ otherPropValue: other }"
    />
  </div>
</template><script>
import Item from "./components/Item";
import VirtualList from "vue-virtual-scroll-list";export default {
  name: "root",
  data() {
    return {
      itemComponent: Item,
      items: Array(1000)
        .fill()
        .map((a, i) => ({ uid: "1", text: `row ${i}` })),
      other: "foo"
    };
  },
  components: { "virtual-list": VirtualList }
};
</script>
```

`components/Item.vue`

```
<template>
  <div class="hello">{{source.text}}</div>
</template><script>
export default {
  props: ["source"]
};
</script>
```

我们从`source`道具中获取要显示的物品。

`App`具有`virtua-list`组件来显示虚拟滚动列表。

`otherPropValue`是我们通过`exta-props`道具传递给`Item`的道具。

所以我们可以写:

```
<template>
  <div class="hello">{{source.text}} {{otherPropValue}}</div>
</template><script>
export default {
  props: ["source", "otherPropValue"]
};
</script>
```

来获取另一个值。

现在只有屏幕上显示的内容会被渲染。

这对于以高性能的方式显示大列表来说非常方便。

![](img/8315d241cefeeb3519e4ec60455e15af.png)

凯特·斯通·马西森在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

Vue Datetime Picker 和 vue2-datepicker 允许我们在应用程序中添加日期选择器。

vue-virtual-scroll-list 是一个允许我们创建虚拟滚动列表的库，它可以加载条目。