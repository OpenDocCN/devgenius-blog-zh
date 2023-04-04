# 使用基于类的组件开发 Vue 应用程序——定制装饰器和超类组件

> 原文：<https://blog.devgenius.io/developing-vue-apps-with-class-based-components-custom-decorators-and-superclass-component-e6b2f8834627?source=collection_archive---------8----------------------->

![](img/5e151a6f2412b12a300ae6738b676a4d.png)

照片由 [NeONBRAND](https://unsplash.com/@neonbrand?utm_source=medium&utm_medium=referral) 在[unsprash](https://unsplash.com?utm_source=medium&utm_medium=referral)上拍摄

如果我们更喜欢使用类，我们可以使用 Vue 附带的基于类的组件 API。

在本文中，我们将研究如何使用基于类的组件开发 Vue 应用程序。

# 定制装饰人员

我们可以在基于 Vue 类的组件中添加我们自己的装饰器。

例如，我们可以写:

```
<template>
  <div>
    <button @click="increment(2)">increment</button>
    <p>{{ count }}</p>
  </div>
</template><script>
import Vue from "vue";
import Component, { createDecorator } from "vue-class-component";const Log = createDecorator((options, key) => {
  const originalMethod = options.methods[key]; options.methods[key] = function wrapperMethod(...args) {
    console.log(`Invoked: ${key}(`, ...args, ")");
    originalMethod.apply(this, args);
  };
});@Component
export default class HelloWorld extends Vue {
  count = 0; @Log
  increment(val) {
    this.count += val;
  }
}
</script>
```

我们用`createDecorator`方法创建`Log`装饰器。

我们传入一个带有`options`和`key`参数的回调。

`options`拥有 Vue 类组件中的项目。

`key`是 Vue 组件类中属性的名称。

`options.methods`进入方式。

然后，我们可以通过分配另一个函数作为它的值来覆盖这个方法。

在`wrappedMethod`内部，我们调用`originalMethod.apply`来调用`originalMethod`，其中`this`是组件类实例。

`args`是我们传递给组件方法的参数。

由于我们将 2 作为`val`的值传入`increment`，当我们调用`increment`时，将会是`args`数组中的值

`key`是我们点击按钮时的`'increment'`方法名称。

# 扩展组件

使用基于类的组件的好处之一是，我们可以创建一个超类组件，它具有我们想要继承的共享部分。

例如，我们可以写:

```
<template>
  <div>
    <p>{{ superValue }}</p>
  </div>
</template><script>
import Vue from "vue";
import Component from "vue-class-component";@Component
class Super extends Vue {
  superValue = "Hello";
}@Component
export default class HelloWorld extends Super {}
</script>
```

我们有具有`superValue`属性的`Super`组件类。

我们有`HelloWorld`组件类，它扩展了`Super`类。

`HelloWorld`具有从`Super`继承的`superValue`属性，可以在模板中使用。

此外，我们可以访问子组件类中的值。

例如，我们可以写:

```
<template>
  <div>
    <p>{{ superValue }}</p>
  </div>
</template><script>
import Vue from "vue";
import Component from "vue-class-component";@Component
class Super extends Vue {
  superValue = "Hello";
}@Component
export default class HelloWorld extends Super {
  created() {
    console.log(this.superValue);
  }
}
</script>
```

我们在`created`钩子中记录从`Super`类继承的`this.superValue`属性。

我们应该会看到`'Hello'`被记录。

# 结论

当我们使用基于类的组件时，我们可以将自定义装饰器和超类组件添加到我们的 Vue 应用程序中。