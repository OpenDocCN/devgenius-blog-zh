# 解决常见的 Vue 问题——子到父的通信，计算属性与方法

> 原文：<https://blog.devgenius.io/solving-common-vue-problems-child-to-parent-communication-and-computed-properties-vs-methods-d16fd840f6c3?source=collection_archive---------10----------------------->

![](img/096ba47ffeb055a9ed688ed633ba1003.png)

由 [Tara Raye](https://unsplash.com/@tararaye?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Vue.js 让开发前端应用变得简单。然而，我们仍有可能遇到问题。

在这篇文章中，我们将看看一些常见的问题，并看看如何解决它们。

# 强制 Vue 重新加载

我们可以使用`this.$forceUpdate();`来强制重载一个 Vue 组件。

然而，这并不是解决大多数问题的好办法。

我们组件上的数据应该用其他方法更新。

# 方法与计算属性

方法的计算属性是不同的，尽管它们都是函数。

计算属性是返回从其他值派生的值的函数。

例如，如果我们有一个字符串形式的`message`状态，我们可以在一个计算属性中返回一个被尊崇的版本:

```
computed: {
  reversedMessage() {
    return this.message.split('').reverse().join('')
  }
}
```

然后它可以作为`this.reveresedMessage`在组件中被访问。

在模板中，我们可以写:

```
{{ reversedMessage }}
```

去访问它。

它只是一个 getter 函数，所以我们不能直接给它设置一个新值。

方法是可以在模板中组件或访问方法的其他部分调用的函数。

例如，我们可以通过编写以下内容来编写方法:

```
methods:{
  search(keyword){
    return this.names.filter(n => n.includes(keyword))
  }
}
```

然后，我们可以通过编写以下代码在组件中调用它:

```
this.search('foo');
```

或者我们可以在模板中这样调用它:

```
search('foo')
```

# 在父事件上调用子组件中的函数

我们可以在子组件上调用一个函数，方法是在子组件上分配一个 ref，然后在其中调用一个函数。

例如，如果我们的子组件如下:

```
const ChildComponent = {
  template: '<div>{{value}}</div>',
  data: function () {
    return {
      value: 0
    };
  },
  methods: {
    setValue(value) {
      this.value = value;
    }
  }
}
```

然后我们可以从父节点调用它:

```
new Vue({
  el: '#app',
  components: {
    'child-component': ChildComponent
  },
  methods: {
    click() {
      this.$refs.childComponent.setValue(5);
    }
  }
})
```

鉴于我们有:

```
<div id="app">
  <child-component ref="childComponent"></child-component>
  <button @click="click">Click</button>  
</div>
```

我们把`childComponent`裁判指派给了`child-component`。

然后可以像我们在`setValue`中做的那样通过 ref 访问子方法。

# 部署 Vue 应用程序

我们可以用`npm run build`命令部署一个 Vue 应用程序。

这将构建应用程序。

# [Vue warn]:属性或方法未在实例上定义，但在呈现过程中被引用'错误

发生此错误是因为状态或方法不在组件代码中，但在模板中定义了它。

如果我们在模板中引用一个方法，那么我们必须在`methods`部分中有它。

例如，如果我们有:

```
const app = new Vue({
  el: "#settings",
  data,
  methods: {
    setValue(index) {
      //...
    }
  }
});
```

然后我们可以在模板中引用它:

```
<button @click='setValue(index)'> set value </button>
```

如果我们访问一个状态，那么我们必须将它包含在我们用`data`方法返回的对象中。

例如，如果我们有:

```
const app = new Vue({
  el: "#settings",
  data(){
    return {
      prop: 'foo'
    }
  }
});
```

然后，我们可以通过编写以下内容在模板中访问它:

```
{{ prop }}
```

否则，我们会得到一个错误。

# 在 Vue.js 中显示未转义的 HTML

我们可以用`v-html`指令在 Vue.js 中显示未转义的 HTML。

例如，我们可以写:

```
<div v-html="htmlContent"> </div>
```

然后假设`htmlContent`是一个包含 HTML 代码的字符串，就直接显示出来。

# 从父数据访问子数据

我们可以发出一个事件，将数据从子节点发送到父节点。

例如，在《孩子》中，我们可以这样写:

```
this.$emit('event', 'foo');
```

`'foo'`是要发出的数据，`'event'`是事件名称。

然后在母体内，我们可以通过书写来听它:

```
<chilld-component @event='doSomething($event)'></child-component>
```

假设子组件叫做`child-component`，我们可以监听它发出的`event`事件。

然后我们可以通过使用拥有数据的`$event`对象来获取数据。

这种情况下，`$event`就是`'foo'`。

然后我们可以对它做一些事情，比如像上面那样将它传递给`doSomething`方法。

![](img/98d6588fa4eebe903e7c152707912c63.png)

[乔丹·罗兰](https://unsplash.com/@yakimadesign?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

我们可以使用`this.$forceUpdate()`方法强制页面重新加载。

为了按原样显示 HTML，我们可以使用`v-html`指令。

我们可以通过调用`this.$emit`将数据从子节点传递给父节点。