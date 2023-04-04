# Vuex 4 —模块名称空间

> 原文：<https://blog.devgenius.io/vuex-4-modules-namespace-e0b4f751119e?source=collection_archive---------5----------------------->

![](img/c4efe99ec643e7283b2f8aa7c6f6eb95.png)

丹尼尔·赫雷斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Vuex 4 处于测试阶段，可能会有变化。

Vuex 是一个流行的 Vue 状态管理库。

Vuex 4 是专为 Vue 3 设计的版本。

在本文中，我们将了解如何将 Vuex 4 与 Vue 3 配合使用。

# 在命名空间模块中访问全局资产

我们可以在命名空间模块中访问全局资产。

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
      <button @click="this['moduleA/increment']">increment</button>
      <p>{{this['moduleA/doubleCount']}}</p>
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
        actions: {
          increment({ commit, dispatch }) {
            commit("increment");
            commit("someAction", null, { root: true });
          }
        },
        getters: {
          doubleCount(state) {
            return state.count * 2;
          }
        }
      }; const store = new Vuex.Store({
        mutations: {
          someAction(state) {
            console.log("someAction");
          }
        },
        modules: {
          moduleA: {
            namespaced: true,
            ...moduleA
          }
        }
      });
      const app = Vue.createApp({
        methods: {
          ...Vuex.mapActions(["moduleA/increment"])
        },
        computed: {
          ...Vuex.mapGetters(["moduleA/doubleCount"])
        }
      });
      app.use(store);
      app.mount("#app");
    </script>
  </body>
</html>
```

我们有从根名称空间提交`someAction`变异的`increment`动作。

因此，当我们调度`moduleA/increment`动作时，我们应该看到`'someAction'`被记录。

我们用一个带有`root: true`属性的对象调用了`commit`，让它调度根突变。

我们可以用行动做同样的事情。例如，我们可以写:

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
      <button @click="this['moduleA/increment']">increment</button>
      <p>{{this['moduleA/doubleCount']}}</p>
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
        actions: {
          increment({ commit, dispatch }) {
            commit("increment");
            dispatch("someOtherAction", null, { root: true });
          }
        },
        getters: {
          doubleCount(state) {
            return state.count * 2;
          }
        }
      }; const store = new Vuex.Store({
        mutations: {
          someAction(state) {
            console.log("someAction");
          }
        },
        actions: {
          someOtherAction({ commit }) {
            commit("someAction");
          }
        },
        modules: {
          moduleA: {
            namespaced: true,
            ...moduleA
          }
        }
      });
      const app = Vue.createApp({
        methods: {
          ...Vuex.mapActions(["moduleA/increment"])
        },
        computed: {
          ...Vuex.mapGetters(["moduleA/doubleCount"])
        }
      });
      app.use(store);
      app.mount("#app");
    </script>
  </body>
</html>
```

我们用一个带有`root: true`属性的对象调用`dispatch`来设置让它调度根动作。

我们可以用`rootGetters`属性访问根 getters。

为此，我们写道:

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
      <button @click="this['moduleA/increment']">increment</button>
      <p>{{this['moduleA/doubleCount']}}</p>
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
        actions: {
          increment({ commit, dispatch, rootGetters }) {
            console.log("increment", rootGetters.one);
            commit("increment");
          }
        },
        getters: {
          doubleCount(state, getters, rootState, rootGetters) {
            console.log("doubleCount", rootGetters.one);
            return state.count * 2;
          }
        }
      }; const store = new Vuex.Store({
        getters: {
          one(state) {
            return 1;
          }
        },
        modules: {
          moduleA: {
            namespaced: true,
            ...moduleA
          }
        }
      });
      const app = Vue.createApp({
        methods: {
          ...Vuex.mapActions(["moduleA/increment"])
        },
        computed: {
          ...Vuex.mapGetters(["moduleA/doubleCount"])
        }
      });
      app.use(store);
      app.mount("#app");
    </script>
  </body>
</html>
```

我们在商店的根中有一个`one`根 getter 方法。

然后我们在`increment`方法的对象参数中有了`rootGetters`属性。

我们可以从`rootGetters.one`属性中获得 getter 的返回值。

其他 getters 可以从`rootGetters`参数中获取值。

# 结论

我们可以命名我们的存储，这样我们就可以把我们的存储分成更小的块，它们有自己的状态。