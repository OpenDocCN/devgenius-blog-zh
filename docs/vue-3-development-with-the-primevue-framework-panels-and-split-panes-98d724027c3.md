# 使用 PrimeVue 框架开发 vue 3——面板和分割窗格

> 原文：<https://blog.devgenius.io/vue-3-development-with-the-primevue-framework-panels-and-split-panes-98d724027c3?source=collection_archive---------2----------------------->

![](img/579214a19c858c5df70d640f306dd24e.png)

照片由[凯特·奥斯伯恩](https://unsplash.com/@kateausburn?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

PrimeVue 是一个与 Vue 3 兼容的 UI 框架。

在本文中，我们将了解如何开始使用 PrimeVue 开发 Vue 3 应用程序。

# 面板

我们可以用 PrimeVue 的`Panel`组件添加一个面板。

例如，我们可以写:

`main.js`

```
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from "primevue/config";
import Panel from 'primevue/panel';
import 'primevue/resources/primevue.min.css'
import 'primevue/resources/themes/saga-blue/theme.css'
import 'primeicons/primeicons.css'
import 'primeflex/primeflex.css';const app = createApp(App);
app.use(PrimeVue);
app.component("Panel", Panel);
app.mount("#app");
```

`App.vue`

```
<template>
  <div>
    <Panel header="Header"> Lorem ipsum dolor sit amet </Panel>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {};
  },
  methods: {},
};
</script>
```

`header`道具有标题文本。

并且默认槽具有内容文本。

我们可以定制带有`header`槽的割台:

```
<template>
  <div>
    <Panel>
      <template #header> Header Content </template>
      Lorem ipsum dolor sit amet
    </Panel>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {};
  },
  methods: {},
};
</script>
```

我们还可以添加`toggleable`道具，使面板可翻转:

```
<template>
  <div>
    <Panel toggleable>
      <template #header> Header Content </template>
      Lorem ipsum dolor sit amet
    </Panel>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {};
  },
  methods: {},
};
</script>
```

我们也可以先用`collapsed`道具折叠面板:

```
<template>
  <div>
    <Panel toggleable collapsed>
      <template #header> Header Content </template>
      Lorem ipsum dolor sit amet
    </Panel>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {};
  },
  methods: {},
};
</script>
```

我们还可以通过填充`icons`插槽来添加自定义图标:

```
<template>
  <div>
    <Panel>
      <template #icons>
        <button class="p-panel-header-icon p-link p-mr-2">
          <span class="pi pi-cog"></span>
        </button>
      </template>
      Lorem ipsum dolor sit amet
    </Panel>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {};
  },
  methods: {},
};
</script>
```

# 分流器

PrimeVue 带有一个 splitter 组件，让我们可以添加一个拆分窗格。

例如，我们可以写:

`main.js`

```
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from "primevue/config";
import Splitter from 'primevue/splitter';
import SplitterPanel from 'primevue/splitterpanel';
import 'primevue/resources/primevue.min.css'
import 'primevue/resources/themes/saga-blue/theme.css'
import 'primeicons/primeicons.css'
import 'primeflex/primeflex.css';const app = createApp(App);
app.use(PrimeVue);
app.component("Splitter", Splitter);
app.component("SplitterPanel", SplitterPanel);
app.mount("#app");
```

`App.vue`

```
<template>
  <div>
    <Splitter style="height: 300px">
      <SplitterPanel> Panel 1 </SplitterPanel>
      <SplitterPanel> Panel 2 </SplitterPanel>
    </Splitter>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {};
  },
  methods: {},
};
</script>
```

我们添加带有`SplitterPane`的`Splitter`组件来添加窗格。

我们可以添加 2 个以上的`SplitPanel`。

此外，我们可以将`Splitter`的`layout`支柱设置为`'vertical'`以使布局垂直。

每个窗格的初始尺寸可以用`size`道具改变:

```
<template>
  <div>
    <Splitter style="height: 300px">
      <SplitterPanel :size="20"> Panel 1 </SplitterPanel>
      <SplitterPanel :size="80"> Panel 2 </SplitterPanel>
    </Splitter>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {};
  },
  methods: {},
};
</script>
```

我们也可以用 tyhe `minSize` prop 设置最小尺寸:

```
<template>
  <div>
    <Splitter style="height: 300px">
      <SplitterPanel :size="20" :minSize="10"> Panel 1 </SplitterPanel>
      <SplitterPanel :size="80" :minSize="20"> Panel 2 </SplitterPanel>
    </Splitter>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {};
  },
  methods: {},
};
</script>
```

`SplitterPanel`还可以嵌套在其他`SplitterPanel`中。

我们还可以使用`stateKey`和`stateStorage`道具将拆分器的大小保存到本地存储中:

```
<template>
  <div>
    <Splitter stateKey="splitter-key" stateStorage="local">
      <SplitterPanel :size="20" :minSize="10"> Panel 1 </SplitterPanel>
      <SplitterPanel :size="80" :minSize="20"> Panel 2 </SplitterPanel>
    </Splitter>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {};
  },
  methods: {},
};
</script>
```

`stateKey`是本地存储的密钥。

并且`stateStorage`指定我们将每个窗格的大小设置保存到本地存储。

# 结论

我们可以使用 PrimeVue 在 Vue 3 中添加面板和拆分面板。