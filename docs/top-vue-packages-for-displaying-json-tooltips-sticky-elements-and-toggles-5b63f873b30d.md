# 用于显示 JSON、工具提示、粘性元素和切换的顶级 Vue 包

> 原文：<https://blog.devgenius.io/top-vue-packages-for-displaying-json-tooltips-sticky-elements-and-toggles-5b63f873b30d?source=collection_archive---------14----------------------->

![](img/9e32b0e7582532968609a06a461f030f.png)

照片由 [Gary Bendig](https://unsplash.com/@kris_ricepees?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将看看显示 JSON、添加工具提示、添加粘性元素和切换开关的最佳包。

# vue-JSON-漂亮

vue-json-pretty 允许我们在 vue 应用中渲染 json。

要使用它，首先我们通过运行以下命令来安装它:

```
npm i vue-json-pretty
```

然后我们可以通过写来使用它:

```
<template>
  <vue-json-pretty :path="'res'" :data="{ foo: 'bar' }" @click="handleClick"></vue-json-pretty>
</template>

<script>
import VueJsonPretty from "vue-json-pretty";export default {
  components: {
    VueJsonPretty
  },
  methods: {
    handleClick(e) {
      console.log(e);
    }
  }
};
</script>
```

我们使用了`vue-json-pretty`组件，它包含了我们想要在屏幕上显示在`data` prop 中的 JSON。

`path`设置为根路径。

`handleClick`是随着`click`事件被发出而运行的。

每当我们单击呈现的 JSON 时，它就会发出。

我们将得到我们点击的路径。

还有许多其他选项可用，如显示线条、要显示的数据深度等等。

# 指令工具提示

vue-directive-tooltip 是工具提示的 vue 指令。

为了使用它，我们运行:

```
npm i vue-directive-tooltip
```

来安装它。

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import Tooltip from "vue-directive-tooltip";
import "vue-directive-tooltip/dist/vueDirectiveTooltip.css";Vue.use(Tooltip);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <span v-tooltip="'tooltip text'">foo</span>
  </div>
</template>

<script>
export default {};
</script>
```

我们注册了插件，然后使用`v-tooltip`指令在鼠标悬停在 span 上时显示工具提示。

价值就是内容。

我们可以改变内容、定位、延迟、偏移，并以不同的方式触发它。

此外，我们可以对工具提示应用自定义样式。

# 粘乎乎的

要创建粘性元素，我们可以使用 vue-sticky 包。

为了使用它，我们运行:

```
npm i vue-sticky
```

来安装它。

然后我们可以通过写来使用它:

```
<template>
  <div>
    <div v-sticky="{ zIndex: 1000, stickyTop: 0, disabled: false }">
      <p>lorem ipsum</p>
    </div>
    <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vivamus ac urna tincidunt, bibendum ipsum et, auctor risus. Cras eget velit ut risus viverra sodales. Suspendisse bibendum sollicitudin quam, vel ullamcorper quam sagittis nec. Aenean egestas elit enim, vitae eleifend leo sagittis in. Sed eget dignissim elit, at scelerisque justo. Mauris rhoncus lectus in sagittis rhoncus. Curabitur viverra eleifend nulla. Aliquam sollicitudin nulla nibh, porttitor eleifend purus pharetra vitae. Donec urna libero, aliquam in consectetur in, fringilla ut sem. Sed placerat ex at orci bibendum blandit. Sed sit amet odio ac purus hendrerit tincidunt. Donec sed lacinia tellus. Fusce vitae nunc id tellus interdum semper. Mauris et cursus velit. Praesent finibus congue iaculis. Vivamus aliquet et magna sit amet commodo.</p>
  </div>
</template>

<script>
import VueSticky from "vue-sticky";export default {
  directives: {
    sticky: VueSticky
  }
};
</script>
```

我们使用`v-sticky`指令使 div 具有粘性。

让我们指定相对于屏幕顶部放置粘性 div 的位置。

`zIndex`让我们设置粘性元素的 z 索引。

`disabled`表示我们是否要禁用粘性元素。

# 羽毛状图标

vue-gallery 是一个响应式视频和图像库。

为了使用它，我们运行:

```
npm i vue-feather-iconsto install it.
```

然后我们通过书写来使用它:

```
<template>
  <div>
    <activity-icon size="1.5x"></activity-icon>
  </div>
</template>

<script>
import { ActivityIcon } from "vue-feather-icons";export default {
  components: {
    ActivityIcon
  }
};
</script>
```

我们只是导入图标，然后使用它。

# Vue 开关

Vue Switches 是我们的 Vue 应用程序的拨动开关组件。

为了使用它，我们运行:

```
npm i vue-switches
```

来安装它。

然后我们可以通过写来使用它:

```
<template>
  <div>
    <switches v-model="enabled"></switches>
  </div>
</template>

<script>
import Switches from "vue-switches";export default {
  components: {
    Switches
  }, data() {
    return {
      enabled: false
    };
  }
};
</script>
```

我们只是使用`switches`组件来添加开关。

它通过`v-model`绑定到`enabled`状态。

它支持文本和颜色变化。

![](img/d1d0832e13118e7b3d7218502b131a16.png)

Raoul Croes 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

vue-json-pretty 是一个让我们在屏幕上显示 json 的包。

vue-directive-tooltip 是我们可以在 vue 应用中使用的工具提示指令。

vue-sticky 让我们创建一个粘性元素。

vue-feather-icons 是一组我们可以使用的图标组件。

Vue Switches 允许我们在屏幕上添加一个拨动开关。