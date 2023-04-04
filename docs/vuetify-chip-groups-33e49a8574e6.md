# 虚拟化—芯片组

> 原文：<https://blog.devgenius.io/vuetify-chip-groups-33e49a8574e6?source=collection_archive---------2----------------------->

![](img/579b23fbb4332980c0184d2856be975a.png)

[丹参赞](https://unsplash.com/@dancounsell?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 芯片组

我们可以用`v-chip-group`组件将芯片分组在一起。

例如，我们可以写:

```
<template>
  <v-container class="grey lighten-5">
    <v-row>
      <v-col>
        <v-sheet elevation="10" class="pa-4">
          <v-chip-group column active-class="primary--text">
            <v-chip v-for="tag in tags" :key="tag">{{ tag }}</v-chip>
          </v-chip-group>
        </v-sheet>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    tags: ["mango", "apple", "orange", "pear", "grape"],
  }),
};
</script>
```

我们将`v-chip`组件放在`v-chip-group`组件中。

# 命令的

我们可以添加`mandatory`属性，这样就有一个值被选中了。

例如，我们可以写:

```
<template>
  <v-container class="grey lighten-5">
    <v-row>
      <v-col>
        <v-sheet elevation="10" class="py-4 px-1">
          <v-chip-group mandatory active-class="primary--text">
            <v-chip v-for="tag in tags" :key="tag">{{ tag }}</v-chip>
          </v-chip-group>
        </v-sheet>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    tags: ["mango", "apple", "orange", "pear", "grape"],
  }),
};
</script>
```

我们把`mandatory`道具放在`v-chip-group`组件上。

# 多重

`multiple`道具让我们选择多个筹码。

例如，我们可以写:

```
<template>
  <v-container class="grey lighten-5">
    <v-row>
      <v-col>
        <v-sheet elevation="10" class="py-4 px-1">
          <v-chip-group multiple active-class="primary--text">
            <v-chip v-for="tag in tags" :key="tag">{{ tag }}</v-chip>
          </v-chip-group>
        </v-sheet>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    tags: ["mango", "apple", "orange", "pear", "grape"],
  }),
};
</script>
```

然后我们可以选择多个芯片。

# 过滤结果

芯片可以通过`filter`道具获得额外的反馈。

例如，我们可以写:

```
<template>
  <v-container class="grey lighten-5">
    <v-row>
      <v-col>
        <v-card class="mx-auto" max-width="400">
          <v-card-text>
            <h2 class="title mb-2">Choose fruits</h2> <v-chip-group v-model="fruits" column multiple>
              <v-chip filter outlined>apple</v-chip>
              <v-chip filter outlined>orange</v-chip>
              <v-chip filter outlined>grape</v-chip>
            </v-chip-group>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    fruits: [],
  }),
};
</script>
```

若要添加被选中时会显示勾号的筹码。

`v-model`将选中的项目绑定到模型上。

`filter`是显示复选标记的原因。

`outlined`使筹码在未被选中时以白色背景显示。

# 结论

我们可以添加芯片组来显示我们可以选择的一组芯片。