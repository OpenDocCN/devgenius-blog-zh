# 使用基于类的组件开发 Vue 应用——类型脚本、超类、钩子和混合

> 原文：<https://blog.devgenius.io/developing-vue-apps-with-class-based-components-typescript-superclasses-and-mixins-b621e3f75ac3?source=collection_archive---------3----------------------->

![](img/65fb72e5e41ccbe2fefb9ab4cedc4daf.png)

照片由 [NeONBRAND](https://unsplash.com/@neonbrand?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

如果我们更喜欢使用类，我们可以使用 Vue 自带的基于类的组件 API。

在本文中，我们将看看如何用基于类的组件开发 Vue 应用。

# 类型脚本、超类和混合

我们可以在 Vue TypeScript 项目中使用`mixins`方法添加 props 和继承超类组件。

例如，我们可以写:

```
<template>
  <div>
    <p>{{ message }}</p>
  </div>
</template><script lang="ts">
import Vue from "vue";
import Component, { mixins } from "vue-class-component";const GreetingProps = Vue.extend({
  props: {
    name: String,
  },
});@Component
class Super extends Vue {
  lastName = "smith";
}@Component
export default class HelloWorld extends mixins(GreetingProps, Super) {
  get message(): string {
    return `Hello, ${this.name} ${this.lastName}`;
  }
}
</script>
```

我们用`1astName`属性创建了`Super`组件类。

我们有用`Vue.extend`方法创建的`GreetProps`类，这样我们可以用一种 TypeScript 可以接受的方式接受道具。

然后我们用`GreetProps`和`Super`方法调用`mixins`方法，这样我们可以从两个类中继承。

我们从两个类中继承，所以`this.name`是`'james'`，而`this.lastName`是`'smith'`。

我们可以用类型定义来定义属性。

例如，我们可以写:

```
<template>
  <div>
    <p v-for="p of persons" :key="p">{{ p.firstName }} {{ p.lastName }}</p>
  </div>
</template><script lang="ts">
import Vue from "vue";
import Component from "vue-class-component";interface Person {
  firstName: string;
  lastName: string;
}@Component
export default class HelloWorld extends Vue {
  persons!: Person[] = [
    { firstName: "james", lastName: "smith" },
    { firstName: "jane", lastName: "doe" },
  ];
}
</script>
```

我们创建了`Person`接口，并使用它来定义`persons`类属性的类型。

`!`意味着类属性不可为空。

现在，如果`persons`数组条目有额外的属性，我们将从 TypeScript 编译器得到一个错误。

# 参考文献和打字稿

为了用基于 TypeScript 类的 Vue 组件定义和分配 ref，我们编写:

```
<template>
  <div>
    <input ref="input" />
  </div>
</template><script lang="ts">
import Vue from "vue";
import Component from "vue-class-component";@Component
export default class HelloWorld extends Vue {
  $refs!: {
    input: HTMLInputElement;
  }; mounted() {
    this.$refs.input.focus();
  }
}
</script>
```

我们必须为我们分配的每个`$refs`属性设置类型。

`input`是一个 HTML 输入元素，所以我们将其设置为`HTMLInputElement`类型。

然后我们调用 call `focus`对它进行聚焦。

添加了类型注释后，当我们在`mounted`钩子中键入代码时，我们得到了自动完成。

# 挂钩自动完成

为了给钩子添加自动完成功能，我们编写:

`main.ts`

```
import Vue from "vue";
import App from "./App.vue";
import "vue-class-component/hooks";
Vue.config.productionTip = false;new Vue({
  render: (h) => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <HelloWorld />
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

`components/HelloWorld.vue`

```
<template>
  <div>
    {{ message }}
  </div>
</template><script lang="ts">
import Vue from "vue";
import Component from "vue-class-component";@Component
export default class HelloWorld extends Vue {
  data() {
    return {
      message: "hello world",
    };
  }
}
</script>
```

一旦我们加上:

```
import "vue-class-component/hooks";
```

在`main.ts`中，当我们在`HelloWorld`组件中键入`data`时，我们得到自动完成。

# 结论

我们可以在用 TypeScript 编写的基于类的 Vue 组件中添加钩子自动完成、混合和超类继承以及引用类型注释。