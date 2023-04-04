# Vuex 4 —动态模块注册和记录

> 原文：<https://blog.devgenius.io/vuex-4-dynamic-module-registration-and-logging-60db113a5bb3?source=collection_archive---------2----------------------->

![](img/353ce08a51f5efa5eefa767bfc0caa95.png)

由 [Antonio Grosz](https://unsplash.com/@angro?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Vuex 4 处于测试阶段，可能会有变化。

Vuex 是一个流行的 Vue 状态管理库。

Vuex 4 是专为 Vue 3 设计的版本。

在本文中，我们将了解如何将 Vuex 4 与 Vue 3 配合使用。

# 动态模块注册

我们可以在用`store.registerModule`方法创建存储之后注册模块。

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
      <p>{{doubleCount}}</p>
    </div>
    <script>
      const moduleA = {
        state: () => ({
          count: 0
        }),
        mutations: {
          increment(state) {
            state.count++;
          }
        },
        getters: {
          doubleCount(state) {
            return state.count * 2;
          }
        }
      }; const store = new Vuex.Store({}); store.registerModule("moduleA", moduleA);
      const app = Vue.createApp({
        methods: {
          ...Vuex.mapMutations(["increment"])
        },
        computed: {
          ...Vuex.mapGetters(["doubleCount"])
        }
      });
      app.use(store);
      app.mount("#app");
    </script>
  </body>
</html>
```

我们用我们的模块名和对象调用了`store.registerModule`方法来注册模块。

然后我们可以像其他模块一样在我们的组件中使用它。

# 插件

我们可以在商店里创建和使用插件。

为此，我们只需创建一个接受`store`参数的函数，并在函数中对其进行处理

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
      <p>{{doubleCount}}</p>
    </div>
    <script>
      const myPlugin = (store) => {
        store.subscribe((mutation, state) => {
          console.log(state);
        });
      }; const store = new Vuex.Store({
        state: () => ({
          count: 0
        }),
        mutations: {
          increment(state) {
            state.count++;
          }
        },
        getters: {
          doubleCount(state) {
            return state.count * 2;
          }
        },
        plugins: [myPlugin]
      }); const app = Vue.createApp({
        methods: {
          ...Vuex.mapMutations(["increment"])
        },
        computed: {
          ...Vuex.mapGetters(["doubleCount"])
        }
      });
      app.use(store);
      app.mount("#app");
    </script>
  </body>
</html>
```

我们用带有`mutation`和`state`参数的回调函数调用了`subscribe`。

`mutation`有正在提交的变异数据。

`state`有商店的当前状态。

# 内置记录器插件

Vuex 4 提供了一个日志插件。

要使用它，我们调用`createLogger`函数。

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
      <p>{{doubleCount}}</p>
    </div>
    <script>
      const store = new Vuex.Store({
        state: () => ({
          count: 0
        }),
        mutations: {
          increment(state) {
            state.count++;
          }
        },
        getters: {
          doubleCount(state) {
            return state.count * 2;
          }
        },
        plugins: [Vuex.createLogger()]
      }); const app = Vue.createApp({
        methods: {
          ...Vuex.mapMutations(["increment"])
        },
        computed: {
          ...Vuex.mapGetters(["doubleCount"])
        }
      });
      app.use(store);
      app.mount("#app");
    </script>
  </body>
</html>
```

去使用它。

我们调用了`Vuex.createLogger`方法来返回登录的插件，并将其传递给`plugins`数组。

该方法接受一个具有一些属性的对象，我们可以将这些属性用作选项。

它们是各种过滤器。

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
      <p>{{doubleCount}}</p>
    </div>
    <script>
      const store = new Vuex.Store({
        state: () => ({
          count: 0
        }),
        mutations: {
          increment(state) {
            state.count++;
          }
        },
        getters: {
          doubleCount(state) {
            return state.count * 2;
          }
        },
        plugins: [
          Vuex.createLogger({
            collapsed: false,
            filter(mutation, stateBefore, stateAfter) {
              return mutation.type !== "foo";
            },
            actionFilter(action, state) {
              return action.type !== "foo";
            },
            transformer(state) {
              return state.subTree;
            },
            mutationTransformer(mutation) {
              return mutation.type;
            },
            actionTransformer(action) {
              return action.type;
            },
            logActions: true,
            logMutations: true,
            logger: console
          })
        ]
      }); const app = Vue.createApp({
        methods: {
          ...Vuex.mapMutations(["increment"])
        },
        computed: {
          ...Vuex.mapGetters(["doubleCount"])
        }
      });
      app.use(store);
      app.mount("#app");
    </script>
  </body>
</html>
```

来设定动作。

`filter`让我们过滤要记录的突变或状态。

`actionFilter`让我们根据动作或状态属性过滤动作。

`transformer`让我们转换日志输出。

`mutationTransformer`让我们转换突变日志输出。

`actionTransformer`让我们转换我们的操作日志输出。

`logActions`是一个布尔值，如果它是`true`的话，让我们记录动作。

`logMutations`如果是`true`，让我们记录突变。

`logger`让我们设置想要用来记录日志的记录器。

# 结论

我们可以动态注册 Vuex 4 商店模块。

此外，Vuex 4 自带记录器。