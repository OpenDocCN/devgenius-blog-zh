# 用于添加计时器、上下文菜单、签名输入和打字效果的顶级 Vue 包

> 原文：<https://blog.devgenius.io/top-vue-packages-for-adding-a-timer-context-menu-signature-input-and-typing-effect-ae7531e71870?source=collection_archive---------22----------------------->

![](img/cb4c7d6c8d1cbb2d24642f9cfd3b3ec8.png)

gff photo by[Ishan @ seefromthesky](https://unsplash.com/@seefromthesky?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将看看添加计时器、上下文菜单、签名和水印输入以及输入动画效果的最佳包。

# vue 计时器

vue-timers 是一个有用的包，我们可以用它来给我们的 vue 应用程序添加计时器。

我们可以通过运行以下命令来安装它:

```
npm i vue-timers
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueTimers from "vue-timers";Vue.use(VueTimers);
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
  timers: {
    log: { time: 1000, autostart: true }
  },
  methods: {
    log() {
      console.log("Hello");
    }
  }
};
</script>
```

我们注册了这个插件。

然后我们添加定时器作为`timers`的值来添加定时器。

`timers`中的属性名是定时器的回调。

所以`log`每 1 秒被调用一次。

`time`是以毫秒为单位的间隔。

`autostart` is 表示组件安装后方法开始运行。

# vue 上下文

vue-context 允许我们向我们的 vue 应用程序添加上下文菜单。

要使用它，我们通过运行以下命令来安装它:

```
npm i vue-context
```

然后我们可以通过写来使用它:

```
<template>
  <div id="app">
    <div>
      <p @contextmenu.prevent="$refs.menu.open">Right click me</p>
    </div> <vue-context ref="menu">
      <li>Option 1</li>
      <li>Option 2</li>
    </vue-context>
  </div>
</template>

<script>
import { VueContext } from "vue-context";export default {
  components: {
    VueContext
  }
};
</script>
```

我们使用`vue-context`组件。

当我们右击 p 元素时，我们监听`contextmenu`事件来切换菜单。

# vue-深集

vue-deepset 允许我们在 vue 应用程序中使用动态路径引用对象。

为了使用它，我们运行:

```
npm i vue-deepset
```

来安装它。

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import * as VueDeepSet from "vue-deepset";
Vue.use(VueDeepSet);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <input type="text" v-model="model['a.bar']">
    <p>{{model['a.bar']}}</p>
  </div>
</template>

<script>
export default {
  computed: {
    model() {
      return this.$deepModel(this.data);
    }
  },
  data() {
    return {
      data: {
        a: { bar: "baz" },
        foo: {
          bar: "qux"
        }
      }
    };
  }
};
</script>
```

我们用 vue-deepset 附带的`this.$deepModel`方法创建了一个计算属性。

我们传入`data`以便解析字符串路径，我们可以通过路径访问值。

# vue 签名

vue-signature 是一个组件，让我们添加一个框，让用户写他们的签名，并添加水印到图像。

为了使用它，我们运行:

```
npm i vue-signature
```

来安装它。

然后我们写道:

```
<template>
  <div id="app">
    <vueSignature ref="signature" :sigOption="option" w="800px" h="400px" :defaultUrl="dataUrl"></vueSignature>
    <button @click="save">Save</button>
    <button @click="clear">Clear</button>
    <button @click="undo">Undo</button>
    <button @click="addWaterMark">addWaterMark</button>
  </div>
</template><script>
export default {
  name: "app",
  data() {
    return {
      option: {
        penColor: "red",
        backgroundColor: "white"
      },
      disabled: false,
      dataUrl: "http://placekitten.com/200/200"
    };
  },
  methods: {
    save() {
      this.$refs.signature1.save("image/jpeg");
    },
    clear() {
      this.$refs.signature.clear();
    },
    undo() {
      this.$refs.signature.undo();
    },
    addWaterMark() {
      this.$refs.signature.addWaterMark({
        text: "mark ",
        font: "20px Arial",
        style: "all",
        fillStyle: "red",
        strokeStyle: "blue"
      });
    }
  }
};
</script>
```

去使用它。

我们使用`vueSignature`组件。

我们给它附加了一个 ref，这样我们就可以通过调用组件自带的方法来使用它。

我们可以利用它。签名可以被清除。

我们可以添加一个我们选择的样式的水印。

# vue-typer

如果我们想以打字动画效果显示文本，我们可以在 vue 应用程序中使用 vue-typer 来实现。

要使用它，我们通过运行以下命令来安装它:

```
npm i vue-typer
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueTyperPlugin from "vue-typer";
Vue.use(VueTyperPlugin);Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <vue-typer text="hello world"></vue-typer>
  </div>
</template><script>
export default {};
</script>
```

我们使用`vue-typer`组件来显示输入效果。

无论`text`道具中的字符串是什么，都会以这种效果显示出来。

当文本是动画时，它发出事件。

![](img/4b6332d00b38aa598b8bb8699d816392.png)

照片由[肖恩·奥·](https://unsplash.com/@seantookthese?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

vue-timers 允许我们添加一个计时器。

vue-context 为我们提供了一种添加上下文菜单的方法。

vue-signature 允许用户添加签名和水印。

vue-typer 让我们用打字效果来激活文本。