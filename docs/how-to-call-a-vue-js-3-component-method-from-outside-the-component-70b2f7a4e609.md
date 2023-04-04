# 如何从组件外部调用一个 Vue.js 3 组件方法？

> 原文：<https://blog.devgenius.io/how-to-call-a-vue-js-3-component-method-from-outside-the-component-70b2f7a4e609?source=collection_archive---------3----------------------->

![](img/87650c8d4a488f78da6fd2a55cde4472.png)

[Julian Hochgesang](https://unsplash.com/@julianhochgesang?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

有时候，我们可能想从组件外部调用一个 Vue 3 组件方法。

在本文中，我们将看看如何从组件外部调用 Vue 3 组件方法。

# 给组件分配一个引用，并从它调用方法

我们可以给一个组件分配一个 ref，从 ref 对象中获取方法并调用它。

例如，我们可以写:

`App.vue`

```
<template>
  <HelloWorld ref="helloWorld" />
  <button @click="greet">greet</button>
</template><script>
import HelloWorld from "./components/HelloWorld";export default {
  name: "App",
  components: {
    HelloWorld,
  },
  methods: {
    greet() {
      this.$refs.helloWorld.greet("jane");
    },
  },
};
</script>
```

`components/HelloWorld.vue`

```
<template>
  <div>hello world</div>
</template><script>
export default {
  methods: {
    greet(name) {
      console.log(`hello world ${name}`);
    },
  },
};
</script>
```

我们将`helloWorld`引用分配给`HelloWorld`组件。

然后我们添加调用`this.$refs.helloWorld.greet`方法的`greet`方法，它是来自`HelloWorld`组件的`greet`方法。

因此，当我们单击 greet 按钮时，应该会看到记录的`'hello world jane'`。

这让我们可以从父组件调用子组件中的方法。

# 从子组件向父组件发送事件

如果我们想从子组件调用父组件中的方法，那么我们应该从子组件调用`$emit`方法，并监听父组件中子组件发出的事件。

例如，我们可以写:

`App.vue`

```
<template>
  <HelloWorld @greet="greet" />
</template><script>
import HelloWorld from "./components/HelloWorld";export default {
  name: "App",
  components: {
    HelloWorld,
  },
  methods: {
    greet(name) {
      console.log(`hello world ${name}`);
    },
  },
};
</script>
```

`components/HelloWorld.vue`

```
<template>
  <div>hello world</div>
  <button @click="greet">greet</button>
</template><script>
export default {
  methods: {
    greet() {
      this.$emit("greet", "jane");
    },
  },
};
</script>
```

在`HelloWorld.vue`中，我们有`greet`方法，它调用`this.$emit`，将事件名称作为第一个参数，将事件有效负载作为第二个参数。

后续参数也是事件有效负载，将用作事件处理程序方法的第二个和后续参数。

然后在`App.vue`中，用`@greet`指令监听`greet`事件。

我们将它的值设置为`greet`以获得作为第二个参数传入的值。

所以`name`就是`'jane'`。

当我们点击“欢迎”按钮时，我们应该会看到`'hello world jane'`被登录。

# 结论

为了从父组件调用子组件中的方法，我们给组件分配一个 ref，并从那里获取它的方法。

如果我们想从子组件调用父组件中的方法，那么我们在子组件中发出一个事件，并在父组件中监听该事件。