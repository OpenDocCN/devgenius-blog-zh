# Vuex 4 —动作和组件

> 原文：<https://blog.devgenius.io/vuex-4-actions-and-components-89ee0c66a77d?source=collection_archive---------4----------------------->

![](img/40beec2b739c2692f66c40d29b703ea9.png)

Ethan Elisara 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

**Vuex 4 处于测试阶段，可能会有变化。**

Vuex 是一个流行的 Vue 状态管理库。

Vuex 4 是专为 Vue 3 设计的版本。

在本文中，我们将了解如何将 Vuex 4 与 Vue 3 配合使用。

# 组件中的调度操作

我们可以用`mapActions`方法将我们的动作映射到组件方法。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <script src="https://unpkg.com/vue@next"></script>
    <script src="https://unpkg.com/vuex@4.0.0-beta.4/dist/vuex.global.js"></script>
    <title>App</title>
  </head>
  <body>
    <div id="app">
      <button @click="increment">increment</button>
      <p>{{count}}</p>
    </div>
    <script>
      const store = new Vuex.Store({
        state: {
          count: 0
        },
        mutations: {
          increment(state) {
            state.count++;
          }
        },
        actions: {
          increment({ commit }) {
            commit("increment");
          }
        }
      });
      const app = Vue.createApp({
        methods: {
          ...Vuex.mapActions(["increment"])
        },
        computed: {
          count() {
            return this.$store.state.count;
          }
        }
      });
      app.use(store);
      app.mount("#app");
    </script>
  </body>
</html>
```

我们调用了`Vuex.mapActions`来将我们的动作映射到一个方法中。

我们也可以用一个不同于动作的名字把它映射到我们的方法。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <script src="https://unpkg.com/vue@next"></script>
    <script src="https://unpkg.com/vuex@4.0.0-beta.4/dist/vuex.global.js"></script>
    <title>App</title>
  </head>
  <body>
    <div id="app">
      <button @click="add">increment</button>
      <p>{{count}}</p>
    </div>
    <script>
      const store = new Vuex.Store({
        state: {
          count: 0
        },
        mutations: {
          increment(state) {
            state.count++;
          }
        },
        actions: {
          increment({ commit }) {
            commit("increment");
          }
        }
      });
      const app = Vue.createApp({
        methods: {
          ...Vuex.mapActions({ add: "increment" })
        },
        computed: {
          count() {
            return this.$store.state.count;
          }
        }
      });
      app.use(store);
      app.mount("#app");
    </script>
  </body>
</html>
```

我们向`Vuex.mapActions`传递了一个对象，以将`increment`动作映射到`add`方法。

关键是我们想要映射到的方法名。

# 撰写动作

动作通常是异步的。我们可以通过返回一个承诺来检查它是否完成。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <script src="https://unpkg.com/vue@next"></script>
    <script src="https://unpkg.com/vuex@4.0.0-beta.4/dist/vuex.global.js"></script>
    <title>App</title>
  </head>
  <body>
    <div id="app">
      <button @click="increment2">increment</button>
      <p>{{count}}</p>
    </div>
    <script>
      const store = new Vuex.Store({
        state: {
          count: 0
        },
        mutations: {
          increment(state, amount) {
            state.count += amount;
          }
        },
        actions: {
          async increment({ commit }) {
            return commit("increment", 1);
          },
          async increment2({ commit }) {
            await commit("increment", 1);
            return commit("increment", 2);
          }
        }
      });
      const app = Vue.createApp({
        methods: {
          ...Vuex.mapActions(["increment2"])
        },
        computed: {
          count() {
            return this.$store.state.count;
          }
        }
      });
      app.use(store);
      app.mount("#app");
    </script>
  </body>
</html>
```

我们有一个`increment2`动作，它用数量 1 调度`increment`动作。

然后我们用数量 2 再次调度它。

因为它们是承诺，我们等待第一个，它们将按顺序运行。

# 结论

使用 Vuex 4，我们可以以不同的方式调度操作。

动作可以用`Vuex.mapActions`映射到方法。