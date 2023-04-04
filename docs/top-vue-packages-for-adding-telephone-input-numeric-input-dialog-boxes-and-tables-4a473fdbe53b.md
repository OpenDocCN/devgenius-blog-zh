# 用于添加电话输入、数字输入、对话框和表格的顶级 Vue 包

> 原文：<https://blog.devgenius.io/top-vue-packages-for-adding-telephone-input-numeric-input-dialog-boxes-and-tables-4a473fdbe53b?source=collection_archive---------25----------------------->

![](img/6343088b7b7b1c777d9b904240227a96.png)

照片由[斯雷科·斯克罗比奇](https://unsplash.com/@sreckoskrobic?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将看看添加电话输入、对话框、数字输入和表格的最佳包。

# 电话输入

vue-tel-input 是一个软件包，可以让我们在 vue 应用程序中添加电话号码输入。

要安装它，我们运行:

```
npm i vue-tel-input
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueTelInput from "vue-tel-input";Vue.use(VueTelInput);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <vue-tel-input v-model="phone"></vue-tel-input>
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

我们有`vue-tel-input`，我们可以用`v-model`绑定输入值。

现在我们应该看到一个显示国家的下拉菜单。

然后我们可以在输入框中输入电话号码。

它为我们提供了许多选项，如设置默认国家，最大长度，占位符，焦点，等等。

它还在输入值改变、国家改变等情况下发出事件。

此外，它还提供了一个用于更改箭头图标的插槽。

# vue 数字

vue-numeric 允许我们向我们的 vue 应用程序添加数字输入。

我们可以通过运行以下命令来安装它:

```
npm i vue-numeric
```

然后我们可以通过写来使用它:

```
<template>
  <div>
    <vue-numeric currency="$" separator="," v-model="price"></vue-numeric>
    <p>{{price}}</p>
  </div>
</template>

<script>
import VueNumeric from "vue-numeric";export default {
  components: {
    VueNumeric
  },
  data() {
    return {
      price: 0
    };
  }
};
</script>
```

我们使用软件包附带的`vue-numeric`组件。

`currency`是货币前缀的字符串。

`separator`是千位分隔符。

`v-model`将输入值绑定到`price`状态。

它还支持使用`min`和`max`道具设置数字的最小值和最大值。

我们也可以禁用负值。

小数精度可以用`precision`道具改变。

占位符可以用`placeholder`道具改变。

# 好桌子

vue-good-table 是一个向我们的 vue 应用程序添加表格的包。

要使用它，首先我们通过运行以下命令来安装它:

```
npm i vue-good-table
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueGoodTablePlugin from "vue-good-table";
import "vue-good-table/dist/vue-good-table.css";Vue.use(VueGoodTablePlugin);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <vue-good-table :columns="columns" :rows="rows" :search-options="{
    enabled: true
  }"></vue-good-table>
  </div>
</template>

<script>
import { VueGoodTable } from "vue-good-table";export default {
  components: {
    VueGoodTable
  },
  data() {
    return {
      columns: [
        {
          label: "first",
          field: "firstName"
        },
        {
          label: "last",
          field: "lastName"
        },
        {
          label: "age",
          field: "age",
          type: "number"
        }
      ],
      rows: [
        { firstName: "james", lastName: "smith", age: 20 },
        { firstName: "alex", lastName: "green", age: 34 }
      ]
    };
  }
};
</script>
```

我们有带列定义数组的`columns`。

每个条目至少有`lavel`和`field`属性。

`type`是我们可以随意指定的列类型。

我们使用`vue-good-table`组件来显示表格。

`columns`和`rows`有各自的道具。

`search-options`让我们改变搜索选项。

`enabled`将启用它。

我们用几行代码得到一个带有搜索栏的表来搜索行。

默认情况下也包括排序。

# vue js-对话框

vue-dialog 是一个添加对话框的有用组件。

要使用它，我们通过运行以下命令来安装它:

```
npm i vuejs-dialog
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VuejsDialog from "vuejs-dialog";import "vuejs-dialog/dist/vuejs-dialog.min.css";Vue.use(VuejsDialog);
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
  async mounted() {
    const dialog = await this.$dialog.alert("success!");
    console.log("closed");
  }
};
</script>
```

我们只需注册插件并使用`this.$dialog.alert`方法打开一个对话框。

![](img/f1d1e246b2ed763aa97125a8893258f5.png)

约翰·麦肯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

vue-tel-input 允许我们在应用程序中添加电话输入。

vuejs-dialog 是一个易于使用的软件包，用于我们添加对话框。

vue-good-table 让我们可以轻松地在应用程序中添加表格。

vue-numeric 是一个有用的数字输入。