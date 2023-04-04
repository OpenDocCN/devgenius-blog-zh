# Vue 问题—混合、插槽和计算属性

> 原文：<https://blog.devgenius.io/vue-problems-mixins-slots-and-computed-properties-5280990d2f04?source=collection_archive---------11----------------------->

![](img/079b6b48d45f2bf7319ed34da2f044ea.png)

劳尔·popadineți 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Vue.js 让开发前端应用变得简单。然而，我们仍有可能遇到问题。

在这篇文章中，我们将看看一些常见的问题，并看看如何解决它们。

# 使用 Webpack 导入 Vue 组件

要在另一个组件中使用一个组件，我们必须导入该组件并注册它。

然后我们可以在组件中使用它。

例如，我们可以写:

```
<template>
  <top-bar></top-bar>
</template><script>
import TopBar from './top-bar.vue'
export default {
  components: {
    TopBar
  }
}
</script>
```

我们导入了`TopBar`组件，然后通过将它放入`components`对象中来注册它。

然后我们可以参考烤肉串盒或它所在的原始盒。

Vue 会自动将上面的骆驼盒映射到烤肉串盒。

# 对不同的路线使用相同的组件

如果我们把`key`道具放入`router-view`，那么我们可以为不同的路线使用相同的组件。

这是因为 Vue 路由器会完全重载组件，而不是重载它认为需要重载的部分。

所以我们可以写:

```
<router-view :key="$route.path"></router-view>
```

# 将所有道具和监听器从父传递给子

我们可以通过使用`v-bind`和`v-on`指令将所有的道具和监听器从父节点传递给子节点。

`v-bind`通过所有道具。`v-on`通过所有监听器。

例如，我们可以写:

```
<child v-bind="$attrs" v-on="$listeners"></child>
```

然后我们用`$attrs`把传递给父节点的所有道具传递给`child`。

我们将传递给父节点的所有事件传递给带有`$listeners`值的子节点。

# 继承带有混合的模板

我们可以让组件模板与插槽一起重用。

例如，我们可以写:

```
Vue.component('not-found', {
  template: '#not-found'
});
```

然后在我们的模板中，我们可以写:

```
<template id="not-found">
  <div>
    <h1>404 Not Found</h1>
    <slot></slot>
  </div>
</template>
```

可以容纳我们想要的任何东西。

我们把东西放在`not-found`的标签之间。

要使用它，我们可以写:

```
<not-found>
  <p>hello</p>
</not-found>
```

然后`<p>hello</p>`将被放置在`slot`组件的位置。

# 将类应用于父组件中的 Vue 功能组件

如果我们有一个功能性的 Vue 组件，我们可以使用`data.staticClass`从父组件中获取类。

我们可以编写的 Gir 实例:

```
<template functional>
  <div class="my-class" :class="data.staticClass || ''" v-bind="data.attrs">
    //...
  </div>
</template>
```

我们对`data.attrs`做同样的事情来获得其他属性。

这样，我们可以从父组件中获取所有的类，并将它们应用到子组件中。

# 在 Vue 组件中重用来自外部资源的方法

通过使用 mixins，我们可以在 Vue 组件中重用来自外部资源的方法。

例如，我们可以写:

```
import mixin from './mixin.js'export default {
  mixins: [mixin]
}
```

Vue mixin 只是一个具有与组件相同的方法和属性的对象。

它也以同样的方式引用`this`。

这是因为如果我们将它合并进来，这些方法将会被合并到组件本身中。

# 将 JSON 数据传递给 Vue 实例

我们可以将 JSON 传递给 Vue 实例。我们只是把它们作为一个普通的对象来解析。

我们只是使用`JSON.parse`将一个 JSON 字符串解析成一个普通的 JavaScript 对象。

例如，我们可以写:

```
import json from 'data.json';new Vue({
  el: '#app',
  data: {
    json
  },
  methods: {
    setJson(payload) {
      this.json = payload
    },
  }
})
```

然后我们可以写:

```
<div id="app">
  <pre>{{ json }}</pre>
</div>
```

Vue 将按原样显示解析后的对象。

# 搜索过滤

为了显示过滤后的搜索结果，我们将创建一个计算属性。

例如，我们可以写:

```
computed: {
  resultQuery(){
    if(this.query){
      return this.resources.filter((item) => {
        return item.name.toLowerCase().includes(this.query)
      })
    }
    else {
      return this.resources;
    }
  }
}
```

我们检查`this.query`的价值。如果它被定义了，那么我们用它来检查`this.resources`中的任何东西是否与列出的条目相匹配。

我们用使`item.name.toLowerCase().includes(this.query)`返回`true`的条目返回一个数组。

那么我们可以这样写:

```
...
<tr v-for="item in resultQuery">
  <td><a>{{item.title}}</a></td>
</tr>
...
```

我们呈现的结果具有包含`this.query`的`name`属性。

![](img/411dc77b5550e3b5d09b514ccefee5d9.png)

照片由[捷尔吉·巴科斯](https://unsplash.com/@thinkdeep?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以导入和注册组件来使用它们。

此外，如果我们想要过滤掉项目，我们可以创建一个计算属性来返回项目。

Mixins 和 slots 对于创建可重用代码非常有用。