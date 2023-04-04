# 解决常见的 Vue 问题—加载图像、事件等

> 原文：<https://blog.devgenius.io/solving-common-vue-problems-reloading-components-and-more-c633e204a7b0?source=collection_archive---------5----------------------->

![](img/4e508a7d523d531c037d1fc70b6e57cf.png)

照片由[迈克·本纳](https://unsplash.com/@mbenna?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Vue.js 让开发前端应用变得简单。然而，我们仍有可能遇到问题。

在这篇文章中，我们将看看一些常见的问题，并看看如何解决它们。

# 从孙组件向祖组件发出事件

我们可以通过从孙子调用`this.$emit`来从孙子向祖父母发出事件。

然后它的父节点可以将它传递给模板中带有`v-on="$listeners"`代码的祖父节点。

然后祖父母可以用通常的`v-on`或`@`监听器代码来监听它。

例如，我们可以写:

```
Vue.component('grand-child', {
  template: '<button @click="emitEvent">emit value</button>',
  methods: {
    emitEvent() { this.$emit('event', 'value') }
  }
})
```

当按钮被点击时，我们发出一个`event`事件。

然后在`child`组件中，我们编写:

```
Vue.component('child', {
  template: '<grand-child v-on="$listeners"></grand-child>'
})
```

我们在`child`中引用了`grand-child`组件。为了将由`grand-child`发出的事件传递给祖父组件，我们有:

```
v-on="$listeners"
```

来冒充他们。

然后，为了创建祖父母，我们写:

```
Vue.component('parent', {
  template: '<child @event="value = $event"></child>',
  data(){
    return {
      value: undefined
    }
  },
})
```

那么`value`将被分配给`'value'`，因为那是我们从`grand-child`传入的。

`$event`具有在`$emit`的第二个参数中随事件发出的值。

# 更新数组

为了更新数组，我们可以通过编写以下代码来调用`Vue.set`方法:

```
Vue.set(object, prop, value)
```

`object`是任何对象，包括数组。

`prop`是属性值，也可以是数组索引。

`value`是我们要用来更新对象属性或数组条目的值。

在组件中，我们也可以使用带有相同参数的`this.$set`方法。

所以我们可以写:

```
this.$set(object, prop, val)
```

当我们调用以下数组方法时，需要调用`Vue.set`或`this.$set`:

*   `push()`
*   `pop()`
*   `shift()`
*   `unshift()`
*   `splice()`
*   `sort()`
*   `reverse()`

Vue 无法跟踪这些方法的变化。

如果我们想从一个对象或数组中删除一个条目，我们使用`Vue.delete`:

```
Vue.delete(obj, 'foo')
```

然后我们移除`obj.foo`属性。

# 将参数传递给计算的属性

如果我们的计算属性函数返回一个函数，我们可以将值传递给计算属性。

例如，我们可以写:

```
computed: {
   salutation() {
      return greeting => `${greeting} ${this.firstName} ${this.lastName}`
   }
}
```

我们有返回函数的`salutation`计算属性。

然后我们可以通过调用它来引用它，而不仅仅是访问属性值。

我们可以写:

```
this.salutation('hi');
```

返回我们想要的字符串。

我们也可以只用一种方法。

例如，我们可以写:

```
methods: {
  greet(greeting){
    return `${greeting} ${this.firstName} ${this.lastName}`
  }
}
```

# 在单个文件组件中使用图像

要在单个文件组件中使用图像，我们可以导入或需要它。

例如，我们可以写:

```
<template>
  <div id="app">
    <img :src="image" />
  </div>
</template><script>
import image from "./assets/pic.png"export default {
  data() {
    return {
      image: image
    }
 }
}
</script>
```

或者:

```
<template>
  <div id="app">
    <img :src="require('./assets/pic.png')" />
  </div>
</template><script>
export default {
}
</script>
```

我们将`import`或`require`返回的对象传入`src`道具。

# 定义全局数据

如果我们在传递给`Vue`构造函数的对象中放入一个`data`属性，我们就可以在 Vue 应用程序中定义全局数据。

例如，我们可以写:

```
new Vue({
  data:{
    shared
  }
})
```

`shared`是我们要做全局的数据。

`data`是公开数据的属性。

# 模板应该只包含一个根元素'错误

Vue 组件只能包含一个根元素。

所以我们必须把所有东西都包装在根元素中。

# 404 重新加载应用程序时

如果我们打开了`html5`模式，那么我们必须确保所有路径都指向我们的 Vue 应用，而不是服务器上的某个文件。

这是因为在 URL 中没有区别来指示我们想要去的地方。

如果我们在 URL 前有一个散列，那么我们就不会有这个问题。

如果我们不想在我们的网址散列，那么我们必须配置我们的服务器重定向所有的网址到 Vue 应用程序。

例如，我们可以写:

```
<ifModule mod_rewrite.c>
    RewriteEngine On
  RewriteBase /
  RewriteRule ^index\.html$ - [L]
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteRule . /index.html [L]
</ifModule>
```

我们使用以下内容将所有 URL 重定向到`index.html`:

```
RewriteRule ^index\.html$ - [L]
```

这是 Apache 服务器的配置。它应该是`.htaccess`文件的一部分。

![](img/e10ceddc4d19667b750cf82dadfe426b.png)

照片由 [Marliese Streefland](https://unsplash.com/@marliesebrandsma?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以用`v-on='$listener'`从孙辈传给祖辈。

此外，如果我们对 URL 使用 HTML5 模式，我们需要重定向到`index.html`。

图像可以导入或需要。