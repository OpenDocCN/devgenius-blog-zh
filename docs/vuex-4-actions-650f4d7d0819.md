# Vuex 4 —动作

> 原文：<https://blog.devgenius.io/vuex-4-actions-650f4d7d0819?source=collection_archive---------6----------------------->

![](img/3cf0ca70913ebdfa314be35da897435d.png)

照片由 [Pietro Mattia](https://unsplash.com/@pietromattia?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Vuex 4 处于测试阶段，可能会有变化。

Vuex 是一个流行的 Vue 状态管理库。

Vuex 4 是专为 Vue 3 设计的版本。

在本文中，我们将了解如何将 Vuex 4 与 Vue 3 配合使用。

# 行动

在 Vuex 4 中，动作类似于突变。

区别在于动作提交突变，动作可以有异步操作。

为了创建一个动作，我们可以向我们的存储添加一个`actions`属性。

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
          increment(context) {
            context.commit("increment");
          }
        }
      });
      const app = Vue.createApp({
        methods: {
          increment() {
            this.$store.dispatch("increment");
          }
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

我们创建了一个带有`actions`属性的 Vuex 商店，其中包含了`increment`方法。

它使用`comtext`参数和`commit`方法来提交变异。

为了在我们的组件中分派动作，我们用动作名调用`this.$store.dispatch`。

我们可以在一个动作中执行异步操作。

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
          incrementAsync({ commit }) {
            setTimeout(() => {
              commit("increment");
            }, 1000);
          }
        }
      });
      const app = Vue.createApp({
        methods: {
          increment() {
            this.$store.dispatch("incrementAsync");
          }
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

我们在`setTimeout`回调中调用了`commit`,将动作分派延迟了一秒。

此外，我们可以使用有效负载来分派动作。

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
          increment(state, payload) {
            state.count += payload.amount;
          }
        },
        actions: {
          incrementAsync({ commit }, payload) {
            setTimeout(() => {
              commit("increment", payload);
            }, 1000);
          }
        }
      });
      const app = Vue.createApp({
        methods: {
          increment() {
            this.$store.dispatch("incrementAsync", { amount: 2 });
          }
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

我们向`incrementAsync`动作添加了一个`payload`参数，以接受动作中的有效负载。

然后我们将一个对象作为第二个参数传递给`this.$store.dispatch`方法来调度我们的动作。

我们还可以传入一个具有`type`属性的对象，以及将包含在有效负载中的任意属性:

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
          increment(state, payload) {
            state.count += payload.amount;
          }
        },
        actions: {
          incrementAsync({ commit }, payload) {
            setTimeout(() => {
              commit("increment", payload);
            }, 1000);
          }
        }
      });
      const app = Vue.createApp({
        methods: {
          increment() {
            this.$store.dispatch({ type: "incrementAsync", amount: 2 });
          }
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

`type`是动作的名称，其他属性将在`incrementAsync`动作的第二个参数中的`payload`对象中结束。

# 结论

我们可以创建动作来提交突变，并使用 Vuex 4 运行异步代码。