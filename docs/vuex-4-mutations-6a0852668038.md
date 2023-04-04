# Vuex 4 —突变

> 原文：<https://blog.devgenius.io/vuex-4-mutations-6a0852668038?source=collection_archive---------6----------------------->

![](img/f758fad799db95d3c4636742ca37a17b.png)

罗斯·芬登在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Vuex 4 处于测试阶段，可能会有变化。

Vuex 是一个流行的 Vue 状态管理库。

Vuex 4 是专为 Vue 3 设计的版本。

在本文中，我们将了解如何将 Vuex 4 与 Vue 3 配合使用。

# 对象风格提交

我们可以向`commit`方法传递一个对象。

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
      <button @click="decrement">decrement</button>
      <p>{{count}}</p>
    </div>
    <script>
      const store = new Vuex.Store({
        state: {
          count: 0
        },
        mutations: {
          decrement(state, payload) {
            state.count -= payload.amount;
          }
        }
      });
      const app = Vue.createApp({
        methods: {
          decrement() {
            this.$store.commit({
              type: "decrement",
              amount: 5
            });
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

我们将一个对象传递给`this.$store.commit`方法。

除了`type`之外的属性会出现在`payload`对象中。

所以我们可以在`decrement`突变方法中用`payload.amount`访问`amount`属性。

突变遵循 Vue 3 的反应性规则，因此每当一个反应性状态变量被更新时，存储状态就会改变。

# 对突变类型使用常数

我们可以对突变类型使用常数。

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
      <button @click="decrement">decrement</button>
      <p>{{count}}</p>
    </div>
    <script>
      const DECREMENT = "decrement"; const store = new Vuex.Store({
        state: {
          count: 0
        },
        mutations: {
          [DECREMENT](state) {
            state.count -= 1;
          }
        }
      });
      const app = Vue.createApp({
        methods: {
          decrement() {
            this.$store.commit(DECREMENT);
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

我们传入了一个`DECREMENT`常量，作为`mutations`属性和`commit`方法中的变异名称。

这对于在多个地方重用同一个名称很有用。

# 突变必须是同步的

突变必须是同步的。

这是因为它里面的代码必须按顺序运行，这样我们才能跟踪代码。

# 在组件中进行突变

我们可以调用`mapMutations`方法来将我们的突变映射到一个方法。

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
      <button [@click](http://twitter.com/click)="decrement">decrement</button>
      <p>{{count}}</p>
    </div>
    <script>
      const store = new Vuex.Store({
        state: {
          count: 0
        },
        mutations: {
          decrement(state) {
            state.count -= 1;
          }
        }
      });
      const app = Vue.createApp({
        methods: {
          ...Vuex.mapMutations(["decrement"])
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

将`Vuex.mapMutations`方法调用为一个方法。

然后，当我们单击“减量”按钮时，我们可以运行它。

当我们用`mapMutations`映射突变时，有效载荷是受支持的。

我们可以将一个突变映射到一个具有不同名称的方法:

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
      <button [@click](http://twitter.com/click)="subtract">decrement</button>
      <p>{{count}}</p>
    </div>
    <script>
      const store = new Vuex.Store({
        state: {
          count: 0
        },
        mutations: {
          decrement(state) {
            state.count -= 1;
          }
        }
      });
      const app = Vue.createApp({
        methods: {
          ...Vuex.mapMutations({
            subtract: "decrement"
          })
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

我们用传递给`mapMutations`的对象的键将`decrement`突变映射到`subtract`方法。

# 结论

我们可以在各种中创建和突变来改变 Vuex 状态数据。