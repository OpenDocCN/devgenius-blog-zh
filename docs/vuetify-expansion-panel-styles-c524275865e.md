# 虚拟化—扩展面板样式

> 原文：<https://blog.devgenius.io/vuetify-expansion-panel-styles-c524275865e?source=collection_archive---------0----------------------->

![](img/19c073c93808bdd79ded8113d0ac7280.png)

Ezra Jeffrey-Comeau 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 手风琴

我们可以创建一个手风琴风格的扩展面板。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-expansion-panels accordion>
          <v-expansion-panel v-for="(item,i) in 5" :key="i">
            <v-expansion-panel-header>Item</v-expansion-panel-header>
            <v-expansion-panel-content>Lorem ipsum.</v-expansion-panel-content>
          </v-expansion-panel>
        </v-expansion-panels>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({}),
};
</script>
```

我们有道具让它像手风琴一样展示。

# 可聚焦的

`focusable`道具让我们可以使扩展面板标题可聚焦。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-expansion-panels focusable>
          <v-expansion-panel v-for="(item,i) in 5" :key="i">
            <v-expansion-panel-header>Item</v-expansion-panel-header>
            <v-expansion-panel-content>Lorem ipsum.</v-expansion-panel-content>
          </v-expansion-panel>
        </v-expansion-panels>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({}),
};
</script>
```

使用`focusable`道具，选中的扩展面板将突出显示标题。

# 外部控制

`v-model`道具让我们控制哪些面板是打开的。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <div>
          <div class="text-center d-flex pb-4">
            <v-btn [@click](http://twitter.com/click)="all">all</v-btn>
            <div>{{ panel }}</div>
            <v-btn [@click](http://twitter.com/click)="none">none</v-btn>
          </div><v-expansion-panels v-model="panel" multiple>
            <v-expansion-panel v-for="(item,i) in items" :key="i">
              <v-expansion-panel-header>Header {{ item }}</v-expansion-panel-header>
              <v-expansion-panel-content>Lorem ipsum.</v-expansion-panel-content>
            </v-expansion-panel>
          </v-expansion-panels>
        </div>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    panel: [],
    items: 5,
  }),
  methods: {
    all() {
      this.panel = [...Array(this.items).keys()].map((k, i) => i);
    },
    none() {
      this.panel = [];
    },
  },
};
</script>
```

创建 5 个扩展面板。

我们有打开所有面板的`all`方法。

这是因为我们有了`multiple`道具。

`none`方法通过将`this.panel`设置为空数组来关闭所有面板。

`v-expansion-panels`上的`v-model`控制扩展面板的打开状态。

# 自定义图标

我们可以为右上角的图标添加自定义图标。

为此，我们写道:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <div>
          <v-expansion-panels>
            <v-expansion-panel>
              <v-expansion-panel-header>
                Item
                <template v-slot:actions>
                  <v-icon color="primary">$expand</v-icon>
                </template>
              </v-expansion-panel-header>
              <v-expansion-panel-content>Lorem ipsum.</v-expansion-panel-content>
            </v-expansion-panel> <v-expansion-panel>
              <v-expansion-panel-header disable-icon-rotate>
                Item
                <template v-slot:actions>
                  <v-icon color="teal">mdi-check</v-icon>
                </template>
              </v-expansion-panel-header>
              <v-expansion-panel-content>Lorem ipsum.</v-expansion-panel-content>
            </v-expansion-panel> <v-expansion-panel>
              <v-expansion-panel-header disable-icon-rotate>
                Item
                <template v-slot:actions>
                  <v-icon color="error">mdi-alert-circle</v-icon>
                </template>
              </v-expansion-panel-header>
              <v-expansion-panel-content>Lorem ipsum.</v-expansion-panel-content>
            </v-expansion-panel>
          </v-expansion-panels>
        </div>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({}),
};
</script>
```

我们在`actions`槽中填充图标。

![](img/88e29c3795cb46bdea850317571c0c8f.png)

[Belle Maluf](https://unsplash.com/@bellemaluf?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

# 结论

我们可以让扩展面板以手风琴的方式显示。

此外，我们可以添加自定义图标到扩展面板。