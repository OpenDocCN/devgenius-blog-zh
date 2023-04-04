# 用于添加文件上传输入、虚拟滚动和文本格式化的顶级 Vue 包

> 原文：<https://blog.devgenius.io/top-vue-packages-for-adding-file-upload-inputs-virtual-scrolling-and-text-formatting-851addca2e78?source=collection_archive---------44----------------------->

![](img/d131605818d857fa72372bc112f766c3.png)

由 [Austin Distel](https://unsplash.com/@austindistel?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在这篇文章中，我们将看看添加文件上传输入、虚拟滚动、文本和数组格式化等的最佳包。

# Vue 文件代理

Vue 文件代理是一个快速的文件上传组件，为我们提供预览功能。

它还包括拖放、验证和上传进度支持。

首先，我们通过运行以下命令安装它:

```
npm i vue-file-agent
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueFileAgent from "vue-file-agent";
import VueFileAgentStyles from "vue-file-agent/dist/vue-file-agent.css";Vue.use(VueFileAgent);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <VueFileAgent :uploadUrl="uploadUrl" v-model="fileRecords"></VueFileAgent>
    <p>{{fileRecords}}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      fileRecords: [],
      uploadUrl: "https://example.com"
    };
  }
};
</script>
```

`v-model`绑定到文件对象。`uploadUrl`是将文件上传到的 URL。

现在我们已经显示了拖放文件上传输入。

# vue 2-过滤器

vue2-filters 为我们提供了各种 vue 滤波器，我们可以在 Vue 组件中使用这些滤波器。

要安装它，我们运行:

```
npm i vue2-filters
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import Vue2Filters from "vue2-filters";Vue.use(Vue2Filters);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>{{ msg | capitalize }}</div>
</template>

<script>
export default {
  data() {
    return {
      msg: "hello world"
    };
  }
};
</script>
```

我们注册插件并使用提供的`capitalize`过滤器。

它将每个单词的第一个字母大写。

还有针对大写字母、小写字母、占位符、格式化数字等的过滤器。

还有接受参数的过滤器。

例如，我们可以使用`number`过滤器:

```
<template>
  <div>{{ 1234567 | number('0,0', { thousandsSeparator: ' ' }) }}</div>
</template>

<script>
export default {};
</script>
```

我们还可以使用过滤器来组织数组。

例如，我们可以写:

```
<template>
  <div>
    <ul>
      <li v-for="user in orderBy(users, 'name')" :key="user.name">{{ user.name }}</li>
    </ul>
  </div>
</template>

<script>
import Vue2Filters from "vue2-filters";export default {
  mixins: [Vue2Filters.mixin],
  data() {
    return {
      users: [{ name: "james" }, { name: "alex" }, { name: "mary" }]
    };
  }
};
</script>
```

我们从`vue2-filters`包中添加 mixins，这样我们就可以使用`orderBy`过滤器。

然后我们按`users`的`name`属性排序。

还有其他数组过滤器来查找项目和更多。

# vue-axios

vue-axios 是一个软件包，让我们可以将 Axios HTTP 客户端添加到我们的 vue 应用程序中。

要安装它，我们运行:

```
npm i vue-axios
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import axios from "axios";
import VueAxios from "vue-axios";Vue.use(VueAxios, axios);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>{{data.name}}</div>
</template>

<script>
export default {
  data() {
    return {
      data: {}
    };
  },
  async mounted() {
    const { data } = await this.axios.get("https://api.agify.io/?name=michael");
    this.data = data;
  }
};
</script>
```

我们注册了插件，这样我们就可以通过使用`this.axios`属性来使用 Axios。

或者，我们可以使用`Vue.axios`或`this.$http`来使用 Axios。

# 虚拟滚动条

vue-virtual-scroller 是一个虚拟滚动插件，我们可以用它在我们的 vue 应用程序中创建一个虚拟滚动元素。

要安装它，我们运行:

```
npm i vue-virtual-scroller
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import { RecycleScroller } from "vue-virtual-scroller";Vue.component("RecycleScroller", RecycleScroller);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <RecycleScroller class="scroller" :items="list" :item-size="32" key-field="id" v-slot="{ item }">
    <div>row {{item}}</div>
  </RecycleScroller>
</template>

<script>
export default {
  data() {
    return {
      list: Array(1000)
        .fill()
        .map((_, i) => i)
    };
  }
};
</script><style scoped>
.scroller {
  height: 100%;
}
</style>
```

我们使用`RecycleScroller`来加载屏幕上显示的内容。

这样的话，比一次加载所有东西的效率要高得多。

![](img/2f71ac887b52dd91f9b2d4dceed88856.png)

照片由[哈姆扎·贾瓦德](https://unsplash.com/@julytheseventifirst?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

vue2-filters 为我们提供了一组过滤器来格式化文本和数组。

Vue 文件代理为我们提供了文件上传输入。

vue-axios 让我们可以在 vue 中轻松使用 axios

vue-virtual-scroller 给了我们一个虚拟的滚动框，只有当项目显示在屏幕上时，才加载项目。