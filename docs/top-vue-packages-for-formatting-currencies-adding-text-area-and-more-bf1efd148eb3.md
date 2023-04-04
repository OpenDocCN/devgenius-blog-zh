# 用于格式化货币、添加文本区域等的顶级 Vue 包

> 原文：<https://blog.devgenius.io/top-vue-packages-for-formatting-currencies-adding-text-area-and-more-bf1efd148eb3?source=collection_archive---------17----------------------->

![](img/8ef5c5ede7a14b5b7495b4793f369c7e.png)

[锚李](https://unsplash.com/@anchorlee?utm_source=medium&utm_medium=referral)在[防溅器](https://unsplash.com?utm_source=medium&utm_medium=referral)上拍照

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将研究用于格式化货币、添加文本区域、允许用户将数据复制到剪贴板以及添加照片砌筑网格的最佳包。

# vue-货币-过滤器

vue-currency-filter 是一个软件包，它为我们提供了格式化货币的过滤器。

要使用它，首先我们通过运行以下命令来安装它:

```
npm install vue-currency-filter
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueCurrencyFilter from "vue-currency-filter";
Vue.use(VueCurrencyFilter, {
  symbol: "$",
  thousandsSeparator: ".",
  fractionCount: 2,
  fractionSeparator: ",",
  symbolPosition: "front",
  symbolSpacing: true
});
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">{{ 3000 | currency}}</div>
</template><script>
export default {};
</script>
```

`symbol`是货币符号，

`thousandsSeparator`是千位分隔符，

`fractionCount`是小数位数。

`fractionSeparator`是小数位数的分隔符。

`symbolPosition`是我们放置货币符号的地方。

`symbolSpacing`是货币符号的间距。

在模板中，我们使用`currency`过滤器来格式化数字。

# vue-文本区域-自动调整大小

vue-textarea-autosize 是一个自动改变高度的文本区域组件。

要安装它，我们运行:

```
npm i vue-textarea-autosize
```

然后我们可以写:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import TextareaAutosize from "vue-textarea-autosize";Vue.use(TextareaAutosize);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <textarea-autosize
      placeholder="text"
      ref="textarea"
      v-model="value"
      :min-height="30"
      :max-height="350"
    />
  </div>
</template><script>
export default {
  data() {
    return {
      value: ""
    };
  }
};
</script>
```

去使用它。

我们有`placeholder`道具来设置占位符。

`ref`是我们可以用来调用 DOM 方法的 ref。

`min-height`和`max-height`调整文本区域的高度。

我们可以写:

```
<template>
  <div id="app">
    <textarea-autosize
      placeholder="text"
      ref="textarea"
      v-model="value"
      :min-height="30"
      :max-height="350"
    />
  </div>
</template><script>
export default {
  data() {
    return {
      value: ""
    };
  },
  mounted() {
    this.$refs.textarea.$el.focus();
  }
};
</script>
```

聚焦文本区域。

要添加自动调整大小，我们可以使用`autosize`道具:

```
<template>
  <div id="app">
    <textarea-autosize
      autosize
      placeholder="text"
      ref="textarea"
      v-model="value"
      :min-height="30"
      :max-height="350"
    />
  </div>
</template><script>
export default {
  data() {
    return {
      value: ""
    };
  }
};
</script>
```

# vue-剪贴板

vue-clipboard 允许我们使用捆绑指令将文本复制或剪切到剪贴板。

要安装它，我们运行:

```
npm i vue-clipboards
```

然后我们可以写:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueClipboards from "vue-clipboards";Vue.use(VueClipboards);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <button v-clipboard="copyData">Copy</button>
  </div>
</template><script>
export default {
  data() {
    return {
      copyData: "hello"
    };
  }
};
</script>
```

我们注册了`VueClipboards`插件。

然后我们使用`v-clipboards`指令让我们在按钮被按下时复制数据。

# 圬工

vue-masonry 是一个 vue 插件，让我们可以轻松地在我们的 Vue 应用程序中添加砖石效果。

我们可以像 Google 或 Flickr 一样在网格中显示图片。

要使用它，我们通过编写以下内容来安装它:

```
npm i vue-masonry
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import { VueMasonryPlugin } from "vue-masonry";Vue.use(VueMasonryPlugin);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <div v-masonry="containerId" transition-duration="0.3s" item-selector=".item">
      <div v-masonry-tile class="item" v-for="n in 30" :key="n">
        <img src="http://placekitten.com/200/200">
      </div>
    </div>
  </div>
</template><script>
export default {
  data() {
    return {
      containerId: "id"
    };
  }
};
</script>
```

我们注册插件。

然后我们使用`v-masonry`和`v-masonry-tile`指令将砖石效果应用到照片上。

`transition-duration`是调整网格大小时的过渡长度。

![](img/c98194d662c01d32a852aab11c614fd4.png)

安德烈科·波迪尔尼克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

# 结论

vue-currency-filter 是一个让我们格式化货币的插件。

vue-textarea-autosize 是一个文本区域，可以根据文本调整大小。

vue-clipboard 允许我们将数据复制到剪贴板

vue-masonry 是一个照片圬工网格。