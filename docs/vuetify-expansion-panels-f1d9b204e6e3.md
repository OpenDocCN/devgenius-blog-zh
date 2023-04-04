# 虚拟化—扩展面板

> 原文：<https://blog.devgenius.io/vuetify-expansion-panels-f1d9b204e6e3?source=collection_archive---------5----------------------->

![](img/de6b76af296cea306b6e61bdb127e077.png)

[澳门图片社](https://unsplash.com/@macauphotoagency?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 扩展面板

`v-expansion-panel`组件对于减少包含大量信息的垂直空间非常有用。

使用`multiple`支柱，扩展面板可以保持打开，直到它明确关闭。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <div>
          <div class="d-flex">
            <v-checkbox v-model="disabled" label="Disabled"></v-checkbox>
          </div> <v-expansion-panels v-model="panel" :disabled="disabled" multiple>
            <v-expansion-panel>
              <v-expansion-panel-header>Panel 1</v-expansion-panel-header>
              <v-expansion-panel-content>Some content</v-expansion-panel-content>
            </v-expansion-panel><v-expansion-panel>
              <v-expansion-panel-header>Panel 2</v-expansion-panel-header>
              <v-expansion-panel-content>Some content</v-expansion-panel-content>
            </v-expansion-panel> <v-expansion-panel>
              <v-expansion-panel-header>Panel 3</v-expansion-panel-header>
              <v-expansion-panel-content>Some content</v-expansion-panel-content>
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
    panel: [0, 1],
    disabled: false,
    readonly: false,
  }),
};
</script>
```

我们有`disabled`复选框来禁用面板。

此外，我们还有`v-expansion-panels`来创建扩展面板容器

然后`v-expansion-panel`组件就是扩展面板本身。

当我们点击标题时，显示标题和`v-expansion-panel-content`。

`panel`状态用于`v-model`并具有面板打开的索引。

# 只读

我们可以将扩展面板设为只读。

它的功能与 disabled 相同，但不改变样式。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <div>
          <div class="d-flex">
            <v-checkbox v-model="readonly" label="readonly"></v-checkbox>
          </div> <v-expansion-panels v-model="panel" :readonly="readonly" multiple>
            <v-expansion-panel>
              <v-expansion-panel-header>Panel 1</v-expansion-panel-header>
              <v-expansion-panel-content>Some content</v-expansion-panel-content>
            </v-expansion-panel><v-expansion-panel>
              <v-expansion-panel-header>Panel 2</v-expansion-panel-header>
              <v-expansion-panel-content>Some content</v-expansion-panel-content>
            </v-expansion-panel> <v-expansion-panel>
              <v-expansion-panel-header>Panel 3</v-expansion-panel-header>
              <v-expansion-panel-content>Some content</v-expansion-panel-content>
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
    panel: [0, 1],
    readonly: false,
  }),
};
</script>
```

# 劣质冲浪板

我们可以让膨胀板有突出的设计。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-expansion-panels popout>
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

我们只是添加了`popout`道具，使展开的扩展面板比其他的更宽。

![](img/4a9be90b75f4abbee9558bc2e1a17803.png)

[澳门图片社](https://unsplash.com/@macauphotoagency?utm_source=medium&utm_medium=referral)拍摄于 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以添加扩展面板来显示可以扩展的垂直内容。