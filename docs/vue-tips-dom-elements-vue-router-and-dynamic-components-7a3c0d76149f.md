# Vue 提示— DOM 元素、Vue 路由器和动态组件

> 原文：<https://blog.devgenius.io/vue-tips-dom-elements-vue-router-and-dynamic-components-7a3c0d76149f?source=collection_archive---------17----------------------->

![](img/881b9fe0458299cabafd12fc58dcc7ca.png)

照片由[巴拉蒂·坎南](https://unsplash.com/@bk010397?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Vue.js 是一个用于创建前端 web 应用程序的流行框架。

在本文中，我们将了解一些编写更好的 Vue.js 应用程序的技巧。

# 使用 Vue 路由器在新标签中打开链接

我们可以用 Vue 路由器在一个新标签中打开一个链接。

例如，我们可以写:

```
const routeData = this.$router.resolve({ name: 'foo', query: { data: "bar" }});
window.open(routeData.href, '_blank');
```

我们调用`this.$router.resolve`方法来获取要打开的 URL。

然后我们调用`window.open`来打开给定的 URL。

第一个参数是 URL。

`'_blank'`表示我们在新的标签页中打开网址。

# 使用 HTTPS 运行 Vue.js 开发服务器

我们可以更改 Vue dev 服务器的配置，通过 HTTPS 而不是 HTTP 来服务项目。

为此，我们读取私钥和证书。

我们设置 URL 来服务这个项目。

例如，我们可以写:

```
const fs = require('fs')module.exports = {
  devServer: {
    https: {
      key: fs.readFileSync('./certs/key.pem'),
      cert: fs.readFileSync('./certs/cert.pem'),
    },
    public: 'https://localhost:8888/'
  }
}
```

我们读入文件，并将它们设置为`https`属性的属性。

要制作证书，我们可以使用`mkcert`程序来完成。

我们可以按照[https://github.com/FiloSottile/mkcert](https://github.com/FiloSottile/mkcert)上的说明在 Windows、Mac OS 或 Linux 上安装它。

然后，我们可以通过运行以下命令来创建新的密钥和证书:

```
mkcert -install
```

并且:

```
mkcert example.com "*.example.com" example.test localhost 127.0.0.1 ::1
```

然后我们创建了一个对本地主机有效的证书。

我们只需将文件复制到`cert`文件夹中，并重命名它们，以匹配我们在配置中的内容。

然后我们就可以运行`npm run dev`，为项目服务了。

# 重新运行 Vue 组件装载()方法

为了重新运行我们在`mounted`方法中编写的代码，我们可以将代码移动到它自己的方法中。

然后我们可以在`mounted`或其他任何地方调用它。

例如，我们可以写:

```
new Vue({
  methods: {
    init(){
      //...
    }
  },
  mounted(){
    this.init();
  }
})
```

然后我们可以写:

```
<button @click="init">reset</button>
```

我们将`init`方法设置为点击处理程序，并在`mounted`钩子中运行它。

现在我们可以随心所欲地重复使用它。

# 使用 Vue 触发事件

我们可以在元素上设置一个 ref，然后在元素上触发事件。

例如，我们可以写:

```
<template>
  <button type="button" @click="onClick" ref="aButton">
    click me
  </button>
</template><script>
export default {
  methods: {
    onClick($event) {
      const elem = this.$refs.aButton.$el;
      elem.click();
    }
  }
}
</script>
```

我们有一个指定了引用的按钮。

然后我们在`onClick`方法中得到按钮的 ref 元素。

它返回按钮的 DOM 节点。

然后我们调用`click`来触发按钮 DOM 节点上的点击。

# 在 Vue 的组件之间共享一个方法

为了在 Vue 的组件之间共享一个方法，我们可以创建一个模块来导出这个方法。

例如，我们可以写:

`src/shared.js`

```
export default {
  bar() { 
    alert("bar") 
  }
}
```

然后，我们可以通过编写以下代码在组件中使用它:

`src/App.js`

```
<template>...</template><script>
import shared from './shared'export default {
  created() { 
    shared.bar();
  }
}
</script>
```

我们导入了`shared`模块，并在其中调用了`bar`方法。

或者，我们可以创建一个 mixin，它是一段可以合并到我们的组件中的代码。

例如，我们可以写:

```
const cartMixin = {
  methods: {
    addToCart(product) {      
      this.cart.push(product);
    }
  }
};
```

然后，我们可以通过编写以下代码在组件中使用它:

```
const Store = Vue.extend({
  template: '#app',
  mixins: [cartMixin],
  data(){
    return {
      cart: []
    }
  }
})
```

我们调用`Vue.extend`来创建`Vue`构造函数的子类。

然后，我们可以创建一个新的`Store`实例，并在我们的应用程序中进行渲染。

# 将组件作为道具传递，并在 Vue 的子组件中使用它

我们可以将组件的标签名传入到`component`组件的`is`属性中。

例如，我们可以写:

```
<template>
  <div>
    <component :is="childComponent">foo bar</component>
  </div>
</template>
```

`childComponent`是带有组件标签名的字符串。

标签之间的内容是填充到默认槽中的内容。

![](img/8c9adf10e06cb6285b14811b96f0771e.png)

照片由[张秀坤·QN](https://unsplash.com/@dominik_qn?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以创建一个共享模块或 mixin 来在 Vue 中创建共享代码。

同样，我们可以使用`this.$router.resolve`来获得可以在其他地方使用的路径。

HTTPS 可以提供 Vue 项目。

我们可以用 refs 获取 HTML 元素的 DOM 对象。