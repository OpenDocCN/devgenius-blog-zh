# 虚拟化—下拉选项

> 原文：<https://blog.devgenius.io/vuetify-dropdown-options-b9013fc4268b?source=collection_archive---------2----------------------->

![](img/2c051b6f0a280dc08bcdd343d8547bb3.png)

安德烈·乔巴努在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 多重选择下拉列表

我们可以用`multiple`道具添加一个多选下拉菜单。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-select v-model="fruit" :items="items" menu-props="auto" label="Select" multiple></v-select>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    items: ["apple", "orange", "pear"],
    fruit: "",
  }),
};
</script>
```

现在，下拉菜单将有一个复选框，让我们选择项目。

使用`chips`道具可以将所选项目显示为筹码:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-select v-model="fruit" :items="items" menu-props="auto" label="Select" multiple chips></v-select>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    items: ["apple", "orange", "pear"],
    fruit: "",
  }),
};
</script>
```

# 密集下拉列表

`dense`道具让我们缩小下拉菜单的高度。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-select v-model="fruit" :items="items" menu-props="auto" label="Select" dense></v-select>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    items: ["apple", "orange", "pear"],
    fruit: "",
  }),
};
</script>
```

添加高度较低的下拉列表。

# 定制项目文本和值

可以用不同的文本和值定制该项目。

要定制它，我们可以使用`return-object`道具。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-select
          v-model="select"
          :items="items"
          menu-props="auto"
          label="State"
          return-object
          :hint="`${select.state}, ${select.abbr}`"
          item-text="state"
          item-value="abbr"
        ></v-select>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    items: [
      { state: "Florida", abbr: "FL" },
      { state: "Georgia", abbr: "GA" },
      { state: "Nebraska", abbr: "NE" },
      { state: "South Dakota", abbr: "SD" },
      { state: "New York", abbr: "NY" },
    ],
    select: "",
  }),
};
</script>
```

我们有`return-object`道具和`item-text`和`item-value`道具。

`item-text`是所选项目的键。

`item-value`是我们要设置为`v-model`的值的选中项的值的键。

我们可以像在`hint`属性的值中那样获得这两个值。

# 自定义菜单道具

我们可以用自定义选项设置`menu-props`道具。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-select :items="items" :menu-props="{ top: true, offsetY: true }" label="Label"></v-select>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    items: ["apple", "orange", "pear"],
    select: "",
  }),
};
</script>
```

我们将`top`属性设置为`true`以使下拉列表向上而不是向下。

`offsetY`使其位置上移。

![](img/1c97b783f221fdd14775e1253b446988.png)

康纳·乐迪在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

我们可以用 Vuetify 添加带有各种选项的下拉菜单。