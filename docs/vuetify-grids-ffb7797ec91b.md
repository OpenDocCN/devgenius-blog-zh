# 虚拟化—网格

> 原文：<https://blog.devgenius.io/vuetify-grids-ffb7797ec91b?source=collection_archive---------3----------------------->

![](img/3f5934950745fcfedf777c8e2209a057.png)

[兰斯·安德森](https://unsplash.com/@lanceanderson?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 网格

我们可以在网格中布局我们的组件。

例如，我们可以写:

```
<template>
  <v-container class="grey lighten-5">
    <v-row v-for="n in 2" :key="n" :class="n === 1 ? 'mb-6' : ''" no-gutters>
      <v-col v-for="k in n + 1" :key="k">
        <v-card class="pa-2" outlined tile>{{ k }} of {{ n + 1 }}</v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({}),
};
</script>
```

我们添加`v-row`和`v-col`组件来分别添加行和列。

我们可以在柱子里放任何东西。

# 等宽栏

可以使列具有相等的宽度。

例如，我们可以写:

```
<template>
  <v-container class="grey lighten-5">
    <v-row no-gutters>
      <template v-for="n in 6">
        <v-col :key="n">
          <v-card class="pa-2" outlined tile>Column</v-card>
        </v-col>
        <v-responsive v-if="n === 3" :key="`width-${n}`" width="100%"></v-responsive>
      </template>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({}),
};
</script>
```

我们添加`v-col`组件来添加列。

我们添加了`v-responsive`组件，根据`v-if`检查将列拆分为行。

我们将`n === 3`作为`v-if`的值，因此我们在每 3 列后显示`v-responsive`，以将每 3 列拆分成各自的行。

# 一个列宽

我们可以只定义一列的宽度，并让它的同级自动围绕它调整大小:

```
<template>
  <v-container class="grey lighten-5">
    <v-row class="mb-6" no-gutters>
      <v-col v-for="n in 3" :key="n" :cols="n === 1 ? 6 : undefined">
        <v-card class="pa-2" outlined tile>{{ n }} of 3 {{ n === 1 ? '(wider)' : '' }}</v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({}),
};
</script>
```

我们将第一列设置为用`cols`道具占据 6 列。

# 可变内容宽度

列的内容宽度可以是可变的。

例如，我们可以写:

```
<template>
  <v-container class="grey lighten-5">
    <v-row class="mb-6" justify="center" no-gutters>
      <v-col lg="2">
        <v-card class="pa-2" outlined tile>1 of 3</v-card>
      </v-col>
      <v-col md="auto">
        <v-card class="pa-2" outlined tile>Variable width content</v-card>
      </v-col>
      <v-col lg="2">
        <v-card class="pa-2" outlined tile>3 of 3</v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({}),
};
</script>
```

我们用`lg`和`md`道具设置断点。

达到这些断点时给定列数的宽度。

# 结论

我们可以添加行和列来创建我们的布局。

宽度可以按照我们的方式设定。