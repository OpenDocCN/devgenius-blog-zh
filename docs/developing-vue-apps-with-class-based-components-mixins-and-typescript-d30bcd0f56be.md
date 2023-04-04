# 使用基于类的组件开发 Vue 应用——Mixins 和 TypeScript

> 原文：<https://blog.devgenius.io/developing-vue-apps-with-class-based-components-mixins-and-typescript-d30bcd0f56be?source=collection_archive---------5----------------------->

![](img/1a1458dc58a19b0e384d1bfca155a979.png)

由[杰斯温·托马斯](https://unsplash.com/@jeswinthomas?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

如果我们更喜欢使用类，我们可以使用 Vue 自带的基于类的组件 API。

在本文中，我们将看看如何用基于类的组件开发 Vue 应用。

# 混合蛋白

我们可以用`mixins`函数为基于 Vue 类的组件创建 mixins。

例如，我们可以写:

```
<template>
  <div>
    <p>{{ hello }} {{ world }}</p>
  </div>
</template><script>
import Vue from "vue";
import Component, { mixins } from "vue-class-component";@Component
class Hello extends Vue {
  hello = "Hello";
}@Component
class World extends Vue {
  world = "World";
}@Component
export default class HelloWorld extends mixins(Hello, World) {
  created() {
    console.log(this.hello, this.world);
  }
}
</script>
```

我们用`Hello`和`World`类组件来调用`mixins`。

那么`hello`和`world`类属性将可以从`Helloworld`组件类中访问。

为了在类中访问它们，我们可以从`this`对象中访问它们。

我们也可以在模板中访问它们。

# 这个值

我们不能在组件类中使用箭头函数作为方法，因为我们需要访问里面的`this`。

Arrow 函数没有绑定到`this`，所以我们不能把它们作为类方法使用。

例如，如果我们写:

```
<template>
  <div>
    <p @click="bar">{{ foo }}</p>
  </div>
</template><script>
import Vue from "vue";
import Component from "vue-class-component";@Component
export default class HelloWorld extends Vue {
  foo = 123; bar = () => {
    this.foo = 456;
  };
}
</script>
```

那么`bar`不会更新`foo`的值，因为`this`的值不是类实例。

# 构造器

我们不应该在我们的类组件中添加`constructor`，因为它们不符合 Vue 的生命周期。

例如，我们不应该写:

```
<template>
  <div>
    <p>{{ foo }}</p>
  </div>
</template><script>
import Vue from "vue";
import Component from "vue-class-component";@Component
export default class HelloWorld extends Vue {
  foo = 123; constructor() {
    this.foo = 456;
  }
}
</script>
```

我们不能在`constructor`中访问反应属性，如果我们在构造函数中尝试，我们会得到一个错误。

# 用 TypeScript 创建类组件

我们可以用 TypeScript 创建基于类的组件。

例如，我们可以写:

`src/components/HelloWorld.vue`

```
<template>
  <div>
    <p>{{ message }}</p>
  </div>
</template><script lang="ts">
import Vue from "vue";
import Component from "vue-class-component";const GreetingProps = Vue.extend({
  props: {
    name: String,
  },
});@Component
export default class HelloWorld extends GreetingProps {
  get message(): string {
    return `Hello, ${this.name}`;
  }
}
</script>
```

`src/App.vue`

```
<template>
  <div id="app">
    <HelloWorld name="james" />
  </div>
</template><script lang='ts'>
import HelloWorld from "./components/HelloWorld.vue";export default {
  name: "App",
  components: {
    HelloWorld,
  },
};
</script>
```

`tsconfig.json`

```
{
  "compilerOptions": {
    "esModuleInterop": true
  }
}
```

我们添加了`compilerOptions.esModuleInterop`属性并将其设置为`true`，这样我们就可以在代码中导入 ES 模块。

在`App.vue`中，我们注册了`HelloWorld`组件。

而在`HelloWorld.vue`中，我们调用`Vue.extend`用道具创建`GreetProps`类。

然后我们可以使用`extends`关键字创建一个扩展`GreetingsProp`以接受`name`道具的类。

在模板中，我们显示了从`message` getter 获取的`message`。

# 结论

我们可以将基于 Vue 类的组件与 TypeScript 一起使用，并在我们的 Vue 项目中轻松定义 mixin 类。