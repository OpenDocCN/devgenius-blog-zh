# 用于添加可打印区域、链接预览等的顶级 Vue 包

> 原文：<https://blog.devgenius.io/top-vue-packages-for-adding-printable-areas-link-previews-and-more-42e105c15099?source=collection_archive---------9----------------------->

![](img/ffd81fe6d7bbfe94c102a641054e4621.png)

安德鲁·贝恩在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将看看添加可打印区域、高亮文本、链接预览、处理键盘快捷键和标签输入的最佳包。

# vue-打印-nb

vue-print-nb 允许我们在 vue 应用程序中定义可打印区域。

要安装它，我们运行:

```
npm i vue-print-nb
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import Print from "vue-print-nb";Vue.use(Print);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <div id="printMe">
      <p>foo</p>
      <p>bar</p>
      <p>baz</p>
    </div> <button v-print="'#printMe'">Print</button>
  </div>
</template><script>
export default {};
</script>
```

我们使用`v-print`指令，并将可打印元素的选择传递给它。

然后，当我们点击一个按钮，然后我们会看到一个打印对话框，预览可打印区域。

# 链接预览

link-prevue 为我们提供了一个 vue 组件，向我们显示给定 URL 的页面预览。

我们通过运行以下命令来安装它:

```
npm i link-prevue
```

然后我们可以通过写来使用它:

```
<template>
  <div id="app">
    <link-prevue url="https://example.com/"></link-prevue>
  </div>
</template><script>
import LinkPrevue from "link-prevue";export default {
  components: {
    LinkPrevue
  }
};
</script>
```

我们导入组件并设置`url`属性来设置链接的属性。

它是可定制的。

例如，我们可以设置一个自定义的加载指示器:

```
<template>
  <div id="app">
    <link-prevue url="https://example.com/">
      <template slot="loading">
        <h1>Loading...</h1>
      </template>
    </link-prevue>
  </div>
</template><script>
import LinkPrevue from "link-prevue";export default {
  components: {
    LinkPrevue
  }
};
</script>
```

我们用自己的内容填充`loading`槽。

然后，当它加载时，我们显示我们的加载指示器。

我们可以定制的其他东西包括卡片宽度、是否显示按钮等等。

# v 热键

热键是一个插件，让我们更容易处理键盘快捷键。

要安装它，我们运行:

```
npm i v-hotkey
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueHotkey from "v-hotkey";Vue.use(VueHotkey);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <span v-hotkey="keymap" v-show="display">Press 'ctrl + m' or hold 'enter'</span>
</template>

<script>
export default {
  data() {
    return {
      display: true
    };
  },
  methods: {
    toggle() {
      this.display = !this.display;
    },
    show() {
      this.display = true;
    },
    hide() {
      this.display = false;
    }
  },
  computed: {
    keymap() {
      return {
        "ctrl+m": this.toggle,
        enter: {
          keydown: this.hide,
          keyup: this.show
        }
      };
    }
  }
};
</script>
```

我们有`keymap` computed 属性，它将组合键映射到它调用的方法。

然后我们可以使用`v-hotkey`指令让我们在元素上使用组合键。

# vue 输入标签

vue-input-tag 是我们的 vue 应用程序的一个简单的标签输入。

为了使用它，我们运行:

```
npm i vue-input-tag
```

来安装它。

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import InputTag from "vue-input-tag";
Vue.component("input-tag", InputTag);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <input-tag v-model="tags"></input-tag>
    <p>{{tags}}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tags: []
    };
  }
};
</script>
```

我们使用`input-tag`组件来添加标签输入。

然后我们使用`v-model`将输入的标签绑定到一个字符串数组中。

我们可以设置各种选项，比如是否允许重复、占位符、最大标签数量等等。

# vue-文本-突出显示

vue-text-highlight 让我们突出显示 vue 组件中的文本。

为了使用它，我们运行:

```
npm i vue-text-highlight
```

来安装它。

然后我们通过书写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import TextHighlight from "vue-text-highlight";Vue.component("text-highlight", TextHighlight);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <text-highlight :queries="queries">{{ description }}</text-highlight>
  </div>
</template>

<script>
export default {
  data() {
    return {
      queries: ["jump", "fox"],
      description: "The quick brown fox jumps over the lazy dog"
    };
  }
};
</script>
```

我们使用带有`queries`属性的`text-highlight`组件来表示我们想要突出显示的内容。

该值是一个字符串数组。

`description`是我们要高亮显示的文本。

![](img/60c79d22cda4afbd7a300a429379d7a3.png)

[艾维·拉道舍尔](https://unsplash.com/@eviradauscher?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

vue-print-nb 允许我们在 vue 应用程序中添加可打印区域。

链接预览是一个为链接添加预览的组件。

热键让我们处理键盘组合。

vue-input-tag 是一个标签输入组件。

vue-text-highlight 让我们突出显示文本。