# Nuxt.js — Vuex

> 原文：<https://blog.devgenius.io/nuxt-js-vuex-c984c54161eb?source=collection_archive---------5----------------------->

![](img/6be1f130036d3fc4934fa5a8cb93ea67.png)

由[保罗·吉尔摩](https://unsplash.com/@paulgilmore_?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Nuxt.js 是一个基于 Vue.js 的应用框架。

我们可以用它来创建服务器端渲染应用和静态站点。

在本文中，我们将看看如何使用 Vuex 和 Nuxt。

# 激活商店

Vuex 内置在 Nuxt 中。

我们可以导入它并将`store`选项添加到根 Vue 实例中。

要添加商店，我们可以写:

`store/index.js`

```
export const state = () => ({
  counter: 0
})export const mutations = {
  increment(state) {
    state.counter++
  }
}
```

我们为 Vuex 商店创建了一个根模块，因为代码在`index.js`文件中。

然后我们可以通过写来使用它:

`page/counter.vue`

```
<template>
  <div class="container">
    <button @click="increment">increment</button>
    <p>{{counter}}</p>
  </div>
</template><script>
export default {
  computed: {
    counter() {
      return this.$store.state.counter;
    },
  },
  methods: {
    increment() {
      this.$store.commit('increment');
    },
  },
};
</script>
```

我们可以访问`this.$store`对象。

`state`属性具有状态。

而`commit`方法让我们运行一个突变。

要创建命名空间模块，我们可以更改存储文件的名称。

例如，我们可以创建一个`store/counter.js`文件并编写:

```
export const state = () => ({
  count: 0
})export const mutations = {
  increment(state) {
    state.count++
  }
}
```

然后，我们可以通过编写以下内容来访问`counter`模块:

```
<template>
  <div class="container">
    <button @click="increment">increment</button>
    <p>{{count}}</p>
  </div>
</template><script>
import { mapMutations } from "vuex";export default {
  computed: {
    count() {
      return this.$store.state.counter.count;
    },
  },
  methods: {
    increment() {
      this.$store.commit('counter/increment');
    },
  },
};
</script>
```

我们添加名称空间来访问状态并提交我们的操作。

此外，我们可以使用 Vuex 中的 map 方法将 getters、mutations 和 actions 映射到组件中的属性。

例如，我们可以写:

```
<template>
  <div class="container">
    <button @click="increment">increment</button>
    <p>{{count}}</p>
  </div>
</template><script>
import { mapMutations } from "vuex";export default {
  computed: {
    count() {
      return this.$store.state.counter.count;
    },
  },
  methods: {
    ...mapMutations({
      increment: "counter/increment",
    }),
  },
};
</script>
```

用`mapMutations`方法将我们的`counter/increment`突变映射到`increment`方法。

# 插件

我们可以添加 Vuex 插件到我们的 Vuex 商店。

例如，我们可以通过编写以下代码将 Vuex 记录器添加到我们的应用程序中:

`store/index.js`

```
import createLogger from 'vuex/dist/logger'export const plugins = [createLogger()]export const state = () => ({
  count: 0
})export const mutations = {
  increment(state) {
    state.count++
  }
}
```

我们只是导出`plugins`数组来添加我们的插件。

# nuxtServerInit 操作

`nuxtServerInit`动作在商店中定义。

这可以在任何环境下运行。

要使用它，我们可以通过编写以下内容将其添加到我们的商店:

`store/index.js`

```
export const actions = {
  nuxtServerInit({ commit }, { req }) {
    commit('core/load', { foo: 'bar' })
  }
}
```

`store/core.js`

```
export const state = () => ({
  obj: {}
})export const mutations = {
  load(state, payload) {
    state.obj = payload;
  }
}
```

`foo.vue`

```
<template>
  <div class="container">{{obj}}</div>
</template><script>
export default {
  computed: {
    obj() {
      return this.$store.state.core.obj;
    },
  },
};
</script>
```

我们在根模块中有`nuxtServerInit`动作。

它有`commit`功能让我们提交突变。

`req`是请求对象。

# 结论

我们可以将 Vuex 商店添加到 Nuxt 应用程序中，只需做一些更改。