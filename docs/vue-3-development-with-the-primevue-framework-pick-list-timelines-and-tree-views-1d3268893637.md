# 使用 PrimeVue 框架开发 vue 3——选择列表、时间线和树形视图

> 原文：<https://blog.devgenius.io/vue-3-development-with-the-primevue-framework-pick-list-timelines-and-tree-views-1d3268893637?source=collection_archive---------1----------------------->

![](img/da83f95518ac4fdda06bcc22a18701d6.png)

由[托德·夸肯布什](https://unsplash.com/@toddquackenbush?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

PrimeVue 是一个与 Vue 3 兼容的 UI 框架。

在本文中，我们将了解如何开始使用 PrimeVue 开发 Vue 3 应用程序。

# 选择列表

我们添加了选项列表组件，让我们可以在不同的列表之间重新排序项目。

例如，我们可以写:

```
<template>
  <div>
    <PickList v-model="cars" dataKey="vin">
      <template #item="slotProps">
        <div class="p-caritem">
          <p>{{ slotProps.item.vin }}</p>
          <p>{{ slotProps.item.year }}</p>
          <p>{{ slotProps.item.color }}</p>
        </div>
      </template>
    </PickList>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      cars: [
        [
          {
            brand: "Volkswagen",
            year: 2012,
            color: "Orange",
            vin: "dsad231ff",
          },
        ],
        [
          { brand: "Audi", year: 2011, color: "Black", vin: "gwregre345" },
          { brand: "Renault", year: 2005, color: "Gray", vin: "h354htr" },
        ],
      ],
    };
  },
  methods: {},
};
</script>
```

选项列表的模型是一个包含两个数组的数组。

第一个数组用于左边的列表。

第二个数组用于右边的列表。

`items`槽有项目显示。

`slotProps.item`有条目的条目。

现在我们应该有两个列表，按钮让我们在选择一个列表后在两个列表之间移动项目。

# 时间表

我们可以用`Timeline`组件给我们的 Vue 3 应用添加一个时间线。

为了补充它，我们写道:

`main.js`

```
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from "primevue/config";
import Timeline from 'primevue/timeline';
import Card from 'primevue/card';
import 'primevue/resources/primevue.min.css'
import 'primevue/resources/themes/saga-blue/theme.css'
import 'primeicons/primeicons.css'
import 'primeflex/primeflex.css';const app = createApp(App);
app.use(PrimeVue);
app.component("Timeline", Timeline);
app.component("Card", Card);
app.mount("#app");
```

`App.vue`

```
<template>
  <div>
    <Timeline :value="events">
      <template #marker="slotProps">
        <span
          class="custom-marker p-shadow-2"
          :style="{ backgroundColor: slotProps.item.color }"
        >
          <i :class="slotProps.item.icon"></i>
        </span>
      </template>
      <template #content="slotProps">
        <Card>
          <template #title>
            {{ slotProps.item.status }}
          </template>
          <template #subtitle>
            {{ slotProps.item.date }}
          </template>
          <template #content>
            <p>Lorem ipsum</p>
          </template>
        </Card>
      </template>
    </Timeline>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      events: [
        {
          status: "Ordered",
          date: "15/10/2020 10:30",
          icon: "pi pi-shopping-cart",
          color: "#9C27B0",
          image: "game-controller.jpg",
        },
        {
          status: "Processing",
          date: "15/10/2020 14:00",
          icon: "pi pi-cog",
          color: "#673AB7",
        },
        {
          status: "Shipped",
          date: "15/10/2020 16:15",
          icon: "pi pi-shopping-cart",
          color: "#FF9800",
        },
        {
          status: "Delivered",
          date: "16/10/2020 10:00",
          icon: "pi pi-check",
          color: "#607D8B",
        },
      ],
      events2: ["2020", "2021", "2022", "2023"],
    };
  },
  methods: {},
};
</script>
```

我们注册了`Timeline`组件。

然后我们用每个时间轴条目的内容填充`content` prop。

`marker`道具让我们把标记改成我们想要的。

我们用每个槽中的`slotProps.item`属性获取项目数据。

# 树形视图

我们可以用 PrimeVue 的`Tree`组件在我们的 Vue 3 应用中添加一个树形视图。

要添加一个，我们写:

`maim.js`

```
import { createApp } from "vue";
import App from "./App.vue";
import PrimeVue from "primevue/config";
import Tree from 'primevue/tree';
import 'primevue/resources/primevue.min.css'
import 'primevue/resources/themes/saga-blue/theme.css'
import 'primeicons/primeicons.css'
import 'primeflex/primeflex.css';const app = createApp(App);
app.use(PrimeVue);
app.component("Tree", Tree);
app.mount("#app");
```

`App.vue`

```
<template>
  <div>
    <Tree :value="nodes" :expandedKeys="expandedKeys"></Tree>
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

我们在`main.js`中注册`Tree`组件。

我们将`value`属性设置为`nodes`数组，该数组包含一些具有`keys`、`label`和`children`属性的对象。

`label`有标签文本。

`children`有一个包含更多节点对象的数组。

# 结论

我们可以使用 PrimeVue 框架将选项列表、树形视图和时间线添加到我们的 Vue 3 应用程序中。