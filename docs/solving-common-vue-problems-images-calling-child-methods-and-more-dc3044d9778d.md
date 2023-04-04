# 解决常见的 Vue 问题——图像、调用子方法等等

> 原文：<https://blog.devgenius.io/solving-common-vue-problems-images-calling-child-methods-and-more-dc3044d9778d?source=collection_archive---------33----------------------->

![](img/b4c6e3474fa67045db456f51182f0cfd.png)

约翰·托尔卡西奥在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Vue.js 让开发前端应用变得简单。然而，我们仍有可能遇到问题。

在这篇文章中，我们将看看一些常见的问题，并看看如何解决它们。

# 动态图像不起作用

如果我们添加一个带有`require`的图像，那么我们可以让它们用一种方法工作:

```
getImgUrl(pic) {
  return require(`../assets/${pic}`)
}
```

然后，我们可以通过编写以下内容在模板中使用它:

```
<img :src="getImgUrl(pic)" :alt="pic">
```

我们在`getImgUrl`方法中返回我们想要的图像，然后将返回的对象直接传递给`src`的值。

# 避免直接变异道具

我们千万不要直接变异道具。

这是一种反模式，因为属性是在对属性的本地更改被覆盖后呈现的。

因此，如果我们有道具，那么我们要么为它们设置一个默认值。

或者我们可以使用一个计算的属性从 props 中创建新的值。

例如，我们可以写:

```
Vue.component('task', {
  template: '#template',
  props: ['list'],
  data() {
    return {
        mutableList: Object.assign({}, this.list);
    }
  }
});
```

或者我们可以写:

```
Vue.component('task', {
  template: '#template',
  props: ['list'],
  computed: {
    newList(){
      return this.list;
    }
  }
});
```

在第一个例子中，我们用`Object.assign({}, this.list);`复制了一个`list`属性值。

在第二个示例中，我们用以下内容创建了一个计算属性`newList`:

```
computed: {
  newList(){
    return this.list;
  }
}
```

# 从异步 Vuex 操作返回承诺

动作是异步的。因此，我们可以在其中回报承诺。

例如，我们可以写:

```
actions: {
  async getData(context, data) {
    const res = await fetch("/api/something")
    const json = await res.json();
    return json;
  }
}
```

我们返回了 Fetch API 返回的获取数据的承诺。

然后，我们可以通过编写下面的代码在组件中分派带有`this.store.$dispatch`的动作:

```
async mounted() {
  try {
    const data = await this.$store.dispatch("getData")
  } catch (ex) {
    console.log(ex);
  }
}
```

# 检测元素外部的单击

我们可以通过创建自己的指令来检测元素外部的点击。

例如，我们可以写:

```
Vue.directive('click-outside', {
  bind () {
    this.event = event => this.vm.$emit(this.expression, event)
    this.el.addEventListener('click', this.stopProp)
    document.body.addEventListener('click', this.event)
  },   
  unbind() {
    this.el.removeEventListener('click', this.stopProp)
    document.body.removeEventListener('click', this.event)
  }, stopProp(event) { event.stopPropagation() }
})
```

当元素被点击时，我们调用`stopPropgation`来阻止点击事件传播到父元素。

这就是在`bind`方法中所做的。

当我们解除指令的绑定时，我们删除所有的监听器来停止事件的比例。

然后我们可以通过写来使用它:

```
<div v-click-outside="doSomething">
  ...
</div
```

当我们在元素外单击时，我们使用我们的指令来运行`doSomething`。

# 使用 Axios 设置数据

要用 Axios 设置数据，我们必须确保我们的`then`回调是一个箭头函数。

例如，我们写道:

```
mounted() {
  axios.get('api/url')
    .then((response) => {
      this.data = response.data;
    });
}
```

如果我们使用常规函数，那么`this`的值就是函数本身。

所以不能用常规函数做回调。

或者，我们可以通过编写以下代码来使用 async/await 语法:

```
async mounted() {
  consr res = await axios.get('api/url')
  this.data = res.data;
}
```

# 观察嵌套数据

我们可以用几种方法来观察嵌套数据。

我们可以通过编写以下代码来添加嵌套属性的观察器:

```
watch:{
  'item.prop1'(newVal){
     //...
  },
  'item.prop2'(newVal){
     //...
  }
}
```

我们用两个将属性路径作为键的观察器来观察`item.prop1`和`item.prop2`。

同样，我们可以将`deep`属性设置为`true`来观察它的值。

例如，我们可以写:

```
watch: {
  item: {
     handler(val){
       // ...
     },
     deep: true
  }
}
```

然后，由于`deep`被设置为`true`，所有嵌套的属性被观察。

当`item`的任何嵌套属性改变时`handler`将运行。

# 在组件外部调用 Vue 组件方法

要在组件外部调用 Vue 组件方法，我们可以在组件上设置一个 ref，然后通过访问 ref 来调用该方法。

例如，如果我们有一个名为`Foo`的组件，我们可以这样写来设置它的引用:

```
<Foo ref="foo"></Foo>
```

然后我们可以写:

```
this.$refs.foo.doSomething(); 
```

如果`Foo`组件有`doSomething`方法。

![](img/bc18d6d0b105b8268267c5da99e1e678.png)

简·梅乌斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们应该避免直接改变属性值。

它们的值将在下次渲染时被覆盖。

此外，我们可以通过阻止点击事件向父节点的传播来检测来自外部的点击。

我们也可以从带有引用的组件中调用子组件的方法。