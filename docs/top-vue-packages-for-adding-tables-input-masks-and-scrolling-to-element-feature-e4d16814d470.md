# 用于添加表格、输入掩码和滚动到元素功能的顶级 Vue 包

> 原文：<https://blog.devgenius.io/top-vue-packages-for-adding-tables-input-masks-and-scrolling-to-element-feature-e4d16814d470?source=collection_archive---------25----------------------->

![](img/e24f515423ee542e962d732dab1b2e2e.png)

[亚历山大·曾](https://unsplash.com/@alexander_tsang?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在这篇文章中，我们将看看添加表格、输入掩码和滚动到元素的最佳包。

# vue-数据表

vue-data-tables 包让我们可以轻松地将表格添加到 vue 应用程序中。

要安装它，我们运行:

```
npm i vue-data-tables element-ui
```

元素 UI 是 vue 数据表的依赖项。

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueDataTables from "vue-data-tables";
import ElementUI from "element-ui";
import "element-ui/lib/theme-chalk/index.css";
import lang from "element-ui/lib/locale/lang/en";
import locale from "element-ui/lib/locale";locale.use(lang);
Vue.use(ElementUI);
Vue.use(VueDataTables);Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

我们导入了元素 UI 和 vue 数据表插件并注册了它们。

此外，我们还为应用程序设置了区域设置。

元素 UI CSS 也被导入。

`App.vue`

```
<template>
  <div>
    <data-tables :data="data" :pagination-props="{ pageSizes: [2, 5, 10, 15] }">
      <el-table-column type="selection" width="55"></el-table-column><el-table-column
        v-for="title in titles"
        :prop="title.prop"
        :label="title.label"
        :key="title.prop"
        sortable="custom"
      ></el-table-column>
    </data-tables>
  </div>
</template>

<script>
export default {
  data() {
    return {
      data: [
        { id: 1, firstName: "james", lastName: "smith" },
        { id: 2, firstName: "jane", lastName: "smith" },
        { id: 3, firstName: "alex", lastName: "green" }
      ],
      titles: [
        {
          prop: "firstName",
          label: "first"
        },
        {
          prop: "lastName",
          label: "last"
        }
      ]
    };
  }
};
</script>
```

我们设置`data`道具来设置数据。

`title`数组具有`prop`和`label`属性来设置要用`prop`属性显示的列。

`label`有表格标题。

默认情况下，我们可以对标题进行排序。

分页也是基本表的一部分。我们设置`pagination-props`属性来设置页面大小。

`pageSizes`属性有页面大小。

我们还添加了一个将`type`设置为`selection`的列，让我们选择行。

它还带有许多其他定制选项。

# v 形面具

v-mask 允许我们在应用程序中添加输入掩码。

我们可以通过运行以下命令来安装它:

```
npm i v-mask
```

要使用它，我们注册插件，然后使用带有`v-mask`指令的输入元素:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueMask from "v-mask";
Vue.use(VueMask);Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <input type="text" v-mask="'###-###-####'" v-model="phone">
    <p>{{phone}}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      phone: ""
    };
  }
};
</script>
```

我们将`v-mask`指令设置为掩码，它限制了输入的格式。

`v-model`绑定到`phone`状态。

`#`表示数字，所以我们只能输入数字。

我们也可以将它用作过滤器，因此我们可以写:

```
<template>
  <div>
    <span>{{ '1234567890' | VMask('(###) ###-####') }}</span>
  </div>
</template>

<script>
export default {};
</script>
```

然后我们得到屏幕上显示的`(123) 456–7890`。

其他掩码占位符包括代表字母的`A`，代表数字或字母的`N`，代表任何符号的`X`，以及代表可选字符的`?`。

我们还可以自定义占位符。

# vue-scrollto

vue-scrollto 是一个易于使用的指令，让我们在应用程序中添加滚动到元素功能。

要安装它，我们可以运行:

```
npm i vue-scrollto
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
const VueScrollTo = require("vue-scrollto");Vue.use(VueScrollTo);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <a href="#" v-scroll-to="'#num-80'">Scroll to 80</a>
    <div :id="`num-${n}`" v-for="n in 100" :key="n">{{n}}</div>
  </div>
</template>

<script>
export default {};
</script>
```

我们将带有`v-scroll-to`指令的元素设置为当它被点击时我们想要滚动到的元素的选择器。

然后我们有了从`v-for`生成的带有 id 的元素。

当我们点击锚时，它会滚动到 ID 为`num-80`的元素。

它发出我们可以监听的事件，并有改变动画的选项。

![](img/366c220139ea1a75d23df924eabdb5fd.png)

照片由[marli ese stree land](https://unsplash.com/@marliesebrandsma?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以使用 vue-scrollto 包让我们滚动到一个元素。

vue-data-tables 让我们轻松地添加带有分页、排序和选择的表格。

v-mask 允许我们在应用程序中添加输入掩码。