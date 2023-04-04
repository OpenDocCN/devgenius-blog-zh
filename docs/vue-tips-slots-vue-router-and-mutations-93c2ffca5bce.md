# Vue 提示—插槽、Vue 路由器和突变

> 原文：<https://blog.devgenius.io/vue-tips-slots-vue-router-and-mutations-93c2ffca5bce?source=collection_archive---------6----------------------->

![](img/77f1f6950940b65277f6cd5bbcab1f16.png)

[乔伊斯·罗梅罗](https://unsplash.com/@joyceromero?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Vue.js 是一个用于创建前端 web 应用程序的流行框架。

在本文中，我们将了解一些编写更好的 Vue.js 应用程序的技巧。

# 如何传递包装组件中的插槽

为了传递包装组件中的插槽，我们可以遍历所有插槽，并将它们传递给子组件。

例如，我们可以写:

```
<wrapper>
  <parent-table v-bind="$attrs" v-on="$listeners"> <slot v-for="slot in Object.keys($slots)" :name="slot" :slot="slot"/> <template v-for="slot in Object.keys($scopedSlots)" :slot="slot" slot-scope="scope">
      <slot :name="slot" v-bind="scope"/>
    </template> </parent-table>
</wrapper>
```

我们用`$slots`变量得到槽。

然后我们使用`Object.keys`来获取插槽的名称，这样我们就可以遍历所有的插槽，并将名称传递下去。

同样，我们可以用`$scopedSlots`变量遍历作用域槽。

我们以同样的方式获取密钥，并用`v-for`以同样的方式循环遍历它们。

在 Vue 2.6 中，引入了`v-slot=`指令来让 is 向下传递插槽。

例如，我们可以写:

```
<wrapper>
  <parent-table v-bind="$attrs" v-on="$listeners">
    <template v-for="(_, slot) of $scopedSlots" v-slot:[slot]="scope">
      <slot :name="slot" v-bind="scope"/>
    </template>
  </parent-table>
</wrapper>
```

我们用`v-for`循环通过插槽。

我们用`$scopedSlots`变量获取作用域槽。

`slot`又是槽名。

这一次，我们将它作为修饰符传递给`v-slot`指令，以传递已命名的槽。

`scope`具有作用域槽中的作用域。

我们使用`v-bind`来获取范围。

而我们

或者，我们可以使用渲染函数来传递插槽。

例如，我们可以写:

```
render(h) {
  const children = Object.keys(this.$slots)
    .map(slot => h('template', { slot }, this.$slots[slot])) return h('wrapper', [
    h('parent-table', {
      attrs: this.$attrs,
      on: this.$listeners,
      scopedSlots: this.$scopedSlots,
    }, children)
  ])
}
```

我们用`this.$slots`属性获取插槽。

我们调用`Object.keys`来获取插槽名称。

我们调用它的`map`来将插槽名称映射到`template`组件。

我们向下传递插槽名称和范围。

然后我们返回一个带有`parent-table`组件的`wrapper`组件，组件带有监听器、属性和作用域槽，而`children`是子组件。

# 从 Vue.js 中的 URL 获取查询参数

我们可以使用`this.$route.query`属性从 Vue 中的 URL 获取查询参数。

为了获得查询参数`foo=bar`，我们编写:

```
this.$route.query.foo
```

我们得到`'bar'`作为值。

假设我们在 Vue 应用中使用 Vue 路由器，这是可用的。

如果我们没有添加它，我们可以写:

`index.html`

```
<script src="https://unpkg.com/vue-router"></script>
```

`index.js`

```
const router = new VueRouter({
  mode: 'history',
  routes: [
    { 
      path: '/page', 
      name: 'page', 
      component: PageComponent
    }
  ]
});const vm = new Vue({
  router,
  el: '#app',
  mounted() {
    const q = this.$route.query.q;
    console.log(q)
  },
});
```

去得到它。

我们创建了`VueRouter`实例，并将其传递给我们传递给`Vue`构造函数的对象。

`routes`有航线。

# 用 Vuex 向突变传递多个参数

要用 Vuex 传递多个参数给 action，我们可以传递一个对象作为有效负载。

例如，我们可以通过编写以下内容来创建我们的突变:

```
mutations: {
  setToken(state, { token, expiration }) {
    localStorage.setItem('token', token);
    localStorage.setItem('expiration', expiration);
  }
}
```

我们有一个对象作为第二个参数。

它具有`token`和`expiration`属性。

然后我们可以通过写下:

```
store.commit('setToken', {
  token,
  expiration,
});
```

我们使用对象中的`token`和`expiration`属性调用`setToken`变异作为第二个参数。

# 用 Vue 路由器重新加载路由

要用 Vue Route 重新加载一条路由，我们可以调用`this.$router.go()`方法。

如果它没有参数，那么它将重新加载当前路线。

我们还可以为路由器视图中的`key`属性添加一个唯一的值:

```
<router-view :key="$route.fullPath"></router-view>
```

这样，当路径改变时它会注意到，并触发用新数据重新加载组件。

![](img/4f1bac37608e9afd49927d96d55f8fb1.png)

照片由 [Dieny Portinanni](https://unsplash.com/@dienyportinanni?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以用`this.$router.go()`重新加载我们的路线。

有许多方法可以将作用域传递给子级。

如果使用 Vue 路由器，我们可以在组件中获得查询参数。

要将多段数据传递给一个变异，我们可以将一个包含所有数据的对象传递给变异。