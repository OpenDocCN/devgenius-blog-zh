# 用 PrimeVue 框架开发 vue 3——树节点内容和一致性

> 原文：<https://blog.devgenius.io/vue-3-development-with-the-primevue-framework-tree-node-content-and-accordions-f2ebf1f048eb?source=collection_archive---------1----------------------->

![](img/6919e278134f9d5c5ce3909391859cc5.png)

照片由[维托·纳塔莱](https://unsplash.com/@otiv345?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

PrimeVue 是一个与 Vue 3 兼容的 UI 框架。

在本文中，我们将了解如何开始使用 PrimeVue 开发 Vue 3 应用程序。

# 树节点内容

我们可以用默认和`url`槽定制树节点的内容。

例如，我们可以写:

```
<template>
  <div>
    <Tree :value="nodes" :expandedKeys="expandedKeys">
      <template #default="slotProps">
        <b>{{ slotProps.node.label }}</b>
      </template>
      <template #url="slotProps">
        <a :href="slotProps.node.data">{{ slotProps.node.label }}</a>
      </template>
    </Tree>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      expandedKeys: undefined,
      nodes: [
        {
          key: "0",
          label: "Introduction",
          children: [
            {
              key: "0-0",
              label: "What is Vue.js?",
              data: "https://vuejs.org/v2/guide/#What-is-Vue-js",
              type: "url",
            },
            {
              key: "0-1",
              label: "Getting Started",
              data: "https://vuejs.org/v2/guide/#Getting-Started",
              type: "url",
            },
            {
              key: "0-2",
              label: "Declarative Rendering",
              data: "https://vuejs.org/v2/guide/#Declarative-Rendering",
              type: "url",
            },
            {
              key: "0-3",
              label: "Conditionals and Loops",
              data: "https://vuejs.org/v2/guide/#Conditionals-and-Loops",
              type: "url",
            },
          ],
        },
        {
          key: "1",
          label: "Components In-Depth",
          children: [
            {
              key: "1-0",
              label: "Component Registration",
              data: "https://vuejs.org/v2/guide/components-registration.html",
              type: "url",
            },
            {
              key: "1-1",
              label: "Props",
              data: "https://vuejs.org/v2/guide/components-props.html",
              type: "url",
            },
            {
              key: "1-2",
              label: "Custom Events",
              data: "https://vuejs.org/v2/guide/components-custom-events.html",
              type: "url",
            },
            {
              key: "1-3",
              label: "Slots",
              data: "https://vuejs.org/v2/guide/components-slots.html",
              type: "url",
            },
          ],
        },
      ],
    };
  },
  methods: {},
};
</script>
```

我们用`slotProps.node`属性获取每个槽中的节点数据。

此外，我们可以添加`filter`插槽来添加文本输入，让我们搜索节点。

# 手风琴

PrimeVue 附带了一个 accordion 组件，让我们可以在选项卡中添加内容。

为了补充它，我们写道:

`main.js`

```
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from "primevue/config";
import Accordion from 'primevue/accordion';
import AccordionTab from 'primevue/accordiontab';
import 'primevue/resources/primevue.min.css'
import 'primevue/resources/themes/saga-blue/theme.css'
import 'primeicons/primeicons.css'
import 'primeflex/primeflex.css';const app = createApp(App);
app.use(PrimeVue);
app.component("Accordion", Accordion);
app.component("AccordionTab", AccordionTab);
app.mount("#app");
```

`App.vue`

```
<template>
  <div>
    <Accordion>
      <AccordionTab header="Header 1"> Content </AccordionTab>
      <AccordionTab header="Header 2"> Content </AccordionTab>
      <AccordionTab header="Header 3"> Content </AccordionTab>
    </Accordion>
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

我们注册了`Accordion`和`AccordionTab`组件，让我们添加带有选项卡的手风琴。

我们只是将内容放入`AccordionTab`的默认槽来添加内容。

`header`具有显示在折叠标签标题中的字符串。

`Accordion`的`multiple`道具可以让我们同时打开多个标签。

我们也可以将`disabled`道具添加到`AccordionTab`来禁用标签。

此外，我们可以通过填充`header`槽将自定义内容添加到标题中:

```
<template>
  <div>
    <Accordion>
      <AccordionTab>
        <template #header>
          <i class="pi pi-calendar p-mr-2"></i>
          <span>Header 1</span>
        </template>
        Content
      </AccordionTab>
      <AccordionTab header="Header 2"> Content </AccordionTab>
      <AccordionTab header="Header 3"> Content </AccordionTab>
    </Accordion>
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

# 结论

我们可以使用 PrimeVue 将带有自定义内容和手风琴的树节点添加到 Vue 3 应用程序中。