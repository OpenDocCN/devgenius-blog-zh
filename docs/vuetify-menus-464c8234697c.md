# 菜单

> 原文：<https://blog.devgenius.io/vuetify-menus-464c8234697c?source=collection_archive---------4----------------------->

![](img/9b6d7e41adc77ce60ef2a45e8b98db3f.png)

照片由[日出照片](https://unsplash.com/@sunrisephotography?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 菜单

我们可以用`v-menu`组件添加菜单。

例如，我们可以写:

```
<template>
  <v-row>
    <v-col cols="12">
      <div class="text-center">
        <v-menu offset-y>
          <template v-slot:activator="{ on, attrs }">
            <v-btn color="primary" dark v-bind="attrs" v-on="on">Dropdown</v-btn>
          </template>
          <v-list>
            <v-list-item v-for="(item, index) in items" :key="index">
              <v-list-item-title>{{ item.title }}</v-list-item-title>
            </v-list-item>
          </v-list>
        </v-menu>
      </div>
    </v-col>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    items: [
      { title: "Click Me" },
      { title: "Click Me 2" },
      { title: "Click Me 3" },
    ],
  }),
};
</script>
```

我们添加了一个按钮来触发菜单。

我们将所有的`attrs`属性传递给`v-btn`，这样我们就可以用它来触发菜单。

`on`对象拥有触发菜单所需的所有事件监听器。

然后我们用菜单项目给了一个`v-list`。

# 绝对位置

菜单的位置可以用`absolute`杆改变。

例如，我们可以写:

```
<template>
  <v-row class="d-flex" justify="center">
    <v-menu v-model="showMenu" absolute offset-y style="max-width: 600px">
      <template v-slot:activator="{ on, attrs }">
        <v-card
          class="portrait"
          img="https://placekitten.com/600/600"
          height="300"
          width="600"
          v-bind="attrs"
          v-on="on"
        ></v-card>
      </template> <v-list>
        <v-list-item v-for="(item, index) in items" :key="index">
          <v-list-item-title>{{ item.title }}</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-menu>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    items: [
      { title: "Click Me" },
      { title: "Click Me 2" },
      { title: "Click Me 3" },
    ],
  }),
};
</script>
```

我们将`absolute`属性添加到`v-menu`中，这样它将显示鼠标被点击的位置。

当我们单击鼠标左键时，它会显示出来。

# 带有激活器和工具提示的菜单

我们可以添加一个带有激活器和工具提示的菜单。

例如，我们可以写:

```
<template>
  <div class="text-center">
    <v-menu>
      <template v-slot:activator="{ on: menu, attrs }">
        <v-tooltip bottom>
          <template v-slot:activator="{ on: tooltip }">
            <v-btn
              color="primary"
              dark
              v-bind="attrs"
              v-on="{ ...tooltip, ...menu }"
            >Dropdown with Tooltip</v-btn>
          </template>
          <span>tooltip</span>
        </v-tooltip>
      </template>
      <v-list>
        <v-list-item v-for="(item, index) in items" :key="index">
          <v-list-item-title>{{ item.title }}</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-menu>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    items: [
      { title: "Click Me" },
      { title: "Click Me 2" },
      { title: "Click Me 3" },
    ],
  }),
};
</script>
```

我们添加了一个工具提示，其中的`v-tooltip`包裹着我们的`v-btn`。

我们将事件监听器从 2 个`template`标签传递到我们的`v-btn`标签，这样我们就可以监听按钮和工具提示事件。

然后我们添加`v-list`组件来添加菜单项。

# 结论

我们可以用`v-menu`组件给我们的应用程序添加一个菜单。