# 用于检查网络状态、复选框和单选按钮等的顶级 Vue 包

> 原文：<https://blog.devgenius.io/top-vue-packages-for-checking-network-status-checkboxes-and-radio-buttons-ab24ea5ccb07?source=collection_archive---------21----------------------->

![](img/1c4ec95d30b1509376e05711fbd7dc75.png)

马丁·莫雷诺在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在这篇文章中，我们将看看用于检查网络状态、粒子背景、复选框和单选按钮以及无限滚动的最佳包。

# Vue 离线

Vue Offline 是一个很好的插件，可以检测一个 Vue app 是否在线离线。

为了使用它，我们运行:

```
npm i vue-offline
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueOffline from "vue-offline";Vue.use(VueOffline);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <p v-if="isOnline">online</p>
    <p v-if="isOffline">Toffline</p>
  </div>
</template>

<script>
export default {
  mounted() {
    this.$on("offline", () => {
      console.log('offline')
    });
  }
};
</script>
```

我们注册插件。，然后我们可以使用`isOnline`和`isOffline`属性来检查应用程序是在线还是离线。

此外，我们可以使用`$on`的方法来监听网络状态的变化。

我们还可以将数据保存到本地存储中，以便离线时可以使用这些数据。

为了保存数据，我们可以写:

```
<template>
  <div>
    <p v-if="isOnline">online</p>
    <p v-if="isOffline">Toffline</p>
  </div>
</template>

<script>
export default {
  mounted() {
    this.$offlineStorage.set("user", { foo: "bar" });
  }
};
</script>
```

我们使用`this.$offlineStorage.set`方法保存数据。

第一个参数是键，第二个是值。

我们可以使用`this.$offlineStorage.get`通过键来获取值。

# 真空粒子

我们可以使用 vue-particles 在我们的 vue 应用程序中显示粒子背景。

为了使用它，我们运行:

```
npm i vue-particles
```

安装软件包。

然后我们可以写:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueParticles from "vue-particles";
Vue.use(VueParticles);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <vue-particles color="#dedede"></vue-particles>
  </div>
</template>

<script>
export default {};
</script>
```

去使用它。

我们用`vue-particles`组件来显示背景。

`color`是粒子的颜色。

我们还可以定制许多其他东西。

线条颜色，宽度，不透明度，距离，速度，不透明度，都可以定制。

# vue-复选框-单选

vue-checkbox-radio 允许我们在 vue 应用程序中添加单选按钮或复选框。

为了使用它，我们运行:

```
npm install vue-checkbox-radio --save
```

来安装它。

然后我们可以写:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import CheckboxRadio from "vue-checkbox-radio";Vue.use(CheckboxRadio);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <checkbox name="agree" v-model="agree" value="1">
      I agree to the
      <a href="#">terms</a>
    </checkbox>
  </div>
</template>

<script>
export default {
  data() {
    return { agree: false };
  }
};
</script>
```

去使用它。

`checkbox`是添加复选框的组件。

标签之间的内容将是复选框旁边的内容。

`name`是复选框的名称属性。

`v-model`将检查值绑定到`agree`状态。

我们还可以用它添加单选按钮。

为此，我们写道:

```
<template>
  <div>
    <radio name="robot" value="1" v-model="isRobot">I'm a robot</radio>
    <radio name="robot" value="0" v-model="isRobot">I'm not a robot</radio>
    <p>{{isRobot}}</p>
  </div>
</template>

<script>
export default {
  data() {
    return { isRobot: false };
  }
};
</script>
```

我们只需将`v-model`绑定到相同的状态，当我们检查它时，它就会被设置。

一组复选框的`name`属性将具有相同的名称。

# vue-mugen-卷轴

vue-mugen-scroll 是一个用于 vue 应用程序的无限滚动库。

要使用它，我们可以通过运行以下命令来安装它:

```
npm i vue-mugen-scroll
```

然后我们写道:

```
<template>
  <div id="app">
    <div class="list">
      <p v-for="n in num" :key="n">{{n}}</p>
    </div>
    <mugen-scroll :handler="fetchData" :should-handle="!loading">loading...</mugen-scroll>
  </div>
</template>

<script>
import MugenScroll from "vue-mugen-scroll";
export default {
  data() {
    return { loading: false, num: 50 };
  },
  methods: {
    fetchData() {
      this.loading = true;
      this.num += 50;
      this.loading = false;
    }
  },
  components: { MugenScroll }
};
</script>
```

向我们的组件添加无限滚动。

我们将任何需要无限滚动的内容放在`mugen-scroll`组件之上，这样它就可以监视滚动位置，并在需要时加载更多数据。

`handler`是获取数据的方法的道具。

`should-handle`是加载处理程序的状态。

![](img/86179b0c93dcdf9b32903c044ade3f16.png)

[小池俊雅](https://unsplash.com/@shunyakoide?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

vue-mugen-scroll 让我们可以添加无限滚动。

Vue Offline 让我们可以在 Vye 应用程序中检查网络状态。

粒子让我们创建一个粒子背景。

vue-checkbox-radio 允许我们添加复选框或单选按钮。