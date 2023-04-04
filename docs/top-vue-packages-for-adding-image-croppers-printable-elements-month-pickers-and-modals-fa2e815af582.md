# 用于添加图像裁剪器、可打印元素、月份选择器和模态的顶级 Vue 包

> 原文：<https://blog.devgenius.io/top-vue-packages-for-adding-image-croppers-printable-elements-month-pickers-and-modals-fa2e815af582?source=collection_archive---------13----------------------->

![](img/a47d09c0654b966ad232d7fa4d07c3f3.png)

杰姆·萨哈冈在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将看看添加图像裁剪器、创建可打印元素、月度选取器和模态的最佳包。

# 从 html 到纸张

我们可以使用 vue-html-to-paper 使我们的 vue 组件可以在纸上打印。

为了使用它，我们运行:

```
npm i vue-html-to-paper
```

来安装它。

然后我们写道:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueHtmlToPaper from "vue-html-to-paper";
Vue.use(VueHtmlToPaper);Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <div id="printMe">
      <h1>Print me!</h1>
    </div>
    <button @click="print">print</button>
  </div>
</template><script>
export default {
  data() {
    return {
      output: null
    };
  },
  methods: {
    print() {
      this.$htmlToPaper("printMe");
    }
  }
};
</script>
```

去使用它。

我们注册了插件，这样我们就可以用我们想要打印的元素的 ID`$htmlToPaper`了。

然后，当我们单击“打印”按钮时，我们将看到一个弹出窗口，显示内容并打开“打印”对话框。

我们可以用自己的 CSS 来定制它:

```
import Vue from "vue";
import App from "./App.vue";
import VueHtmlToPaper from "vue-html-to-paper";
const options = {
  name: "_blank",
  specs: ["fullscreen=yes", "titlebar=yes", "scrollbars=yes"],
  styles: [
    "https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css"
  ]
};Vue.use(VueHtmlToPaper, options);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

我们应用 Bootstrap 4 样式表中的样式，并将`fullscreen`设置为`yes`以支持全屏。

`titlebar`包括标题栏。

`scrollbar`包括滚动条。

`$htmlToPaper`也接受打印后调用的回调，然后运行代码。

# vue-每月采摘者

vue-monthly-picker 是一个用于 vue 应用程序的月选择器包。

为了使用它，我们运行:

```
npm i vue-monthly-picker moment
```

来安装它。

力矩是必需的依赖。

然后我们通过书写来使用它:

```
<template>
  <div>
    <vue-monthly-picker v-model="selectedMonth"></vue-monthly-picker>
    <p>{{selectedMonth}}</p>
  </div>
</template><script>
import VueMonthlyPicker from "vue-monthly-picker";export default {
  components: {
    VueMonthlyPicker
  },
  data() {
    return {
      selectedMonth: undefined
    };
  }
};
</script>
```

我们使用`vue-monthly-picker`来添加一个日期选择器。

`v-model`将选择的月份绑定到`selectedMonth`状态。

# 沃达尔

Vodal 是一个为 Vue 应用程序设计的模态组件。

为了使用它，我们运行:

```
npm i vodal
```

来安装它。

然后我们可以通过写来使用它:

```
<template>
  <div>
    <vodal :show="show" animation="rotate" @hide="show = false">
      <div>vue modal.</div>
    </vodal>
  </div>
</template><script>
import Vodal from "vodal";
import "vodal/common.css";
import "vodal/rotate.css";export default {
  components: {
    Vodal
  },
  data() {
    return {
      show: true
    };
  }
};
</script>
```

我们使用`vodal`组件和 CSS。

模态内容在`vodal`标签之间。

`show`道具使模态显示是否为`true`。

当我们单击关闭按钮时，会发出`hide`事件。

`animation`设置为旋转，以在我们打开和关闭模态时看到一个旋转动画。

我们可以根据自己的意愿定制样式和内容。

其他动画类型包括缩放、淡入淡出、翻转、向上滑动、向下滑动等等。

# Vue 高级收割机

Vue 高级裁剪器是一个方便的图像裁剪器，开发者和用户都很容易使用。

我们可以通过运行以下命令来安装它:

```
npm i vue-advanced-cropper
```

那么我们可以如下使用它:

```
<template>
  <div>
    <cropper
      class="cropper"
      :src="img"
      :stencilProps="{
      aspectRatio: 10/12
    }"
      @change="change"
    ></cropper>
  </div>
</template><script>
import { Cropper } from "vue-advanced-cropper";export default {
  data() {
    return {
      img: "http://placekitten.com/200/200"
    };
  },
  methods: {
    change({ coordinates, canvas }) {
      console.log(coordinates, canvas);
    }
  },
  components: {
    Cropper
  }
};
</script>
```

现在我们得到一个用我们为`img` URL 设置的图像裁剪的图像。

`src`设置为图像。

`stencilProps`设置各种选项，如长宽比。

像最小和最大纵横比、类名、行数等其他东西可以在`stencilProps`对象中设置。

![](img/47e36ec9ecb1d8c1f248ac355ace5f6c.png)

米兰·塞特勒在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

# 结论

vue-html-to-paper 允许我们制作可打印的元素并显示一个打印对话框。

vue-monthly-picker 是一个月选择器组件。

Vodal 是一个简单的可定制的模态组件。

Vue 高级裁剪器是一个图像裁剪器组件。