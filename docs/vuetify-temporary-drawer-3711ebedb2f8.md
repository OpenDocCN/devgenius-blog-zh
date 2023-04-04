# Vuetify —临时抽屉

> 原文：<https://blog.devgenius.io/vuetify-temporary-drawer-3711ebedb2f8?source=collection_archive---------4----------------------->

![](img/9ca060442b624788663a657fae815da6.png)

照片由 [chuttersnap](https://unsplash.com/@chuttersnap?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 临时抽屉

我们可以创建一个位于应用程序上方的临时抽屉，并使用叠加来加深背景。

在抽屉外单击会导致它关闭。

例如，我们可以写:

```
<template>
  <v-row>
    <v-col cols="12">
      <v-sheet height="400" class="overflow-hidden" style="position: relative;">
        <v-container class="fill-height">
          <v-row align="center" justify="center">
            <v-btn color="pink" dark @click.stop="drawer = !drawer">Toggle</v-btn>
          </v-row>
        </v-container> <v-navigation-drawer v-model="drawer" absolute temporary>
          <v-list-item>
            <v-list-item-avatar>
              <v-img src="https://randomuser.me/api/portraits/men/78.jpg"></v-img>
            </v-list-item-avatar> <v-list-item-content>
              <v-list-item-title>John Smith</v-list-item-title>
            </v-list-item-content>
          </v-list-item> <v-divider></v-divider> <v-list dense>
            <v-list-item v-for="item in items" :key="item.title" link>
              <v-list-item-icon>
                <v-icon>{{ item.icon }}</v-icon>
              </v-list-item-icon> <v-list-item-content>
                <v-list-item-title>{{ item.title }}</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list>
        </v-navigation-drawer>
      </v-sheet>
    </v-col>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    drawer: null,
    items: [
      { title: "Click Me" },
      { title: "Click Me 2" },
      { title: "Click Me 3" },
    ],
  }),
};
</script>
```

我们有`v-navigation-drawer`组件来添加当我们点击切换按钮时打开的抽屉。

切换按钮是用带有`click`监听器的`v-btn`组件创建的。

我们只需在`true`和`false`之间切换`drawer`变量来切换菜单。

`v-navigation-drawer`的`v-model`指令使用`drawer`状态来控制抽屉的打开和关闭。

抽屉项目在`v-list-item`组件中。

# 右定位菜单

我们可以把菜单放在屏幕的右边。

例如，我们可以写:

```
<template>
  <v-row>
    <v-col cols="12">
      <v-card height="350px">
        <v-navigation-drawer absolute permanent right>
          <template v-slot:prepend>
            <v-list-item two-line>
              <v-list-item-avatar>
                <img src="https://randomuser.me/api/portraits/women/81.jpg" />
              </v-list-item-avatar> <v-list-item-content>
                <v-list-item-title>Jane Smith</v-list-item-title>
                <v-list-item-subtitle>Logged In</v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
          </template> <v-divider></v-divider> <v-list dense>
            <v-list-item v-for="item in items" :key="item.title">
              <v-list-item-icon>
                <v-icon>{{ item.icon }}</v-icon>
              </v-list-item-icon> <v-list-item-content>
                <v-list-item-title>{{ item.title }}</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list>
        </v-navigation-drawer>
      </v-card>
    </v-col>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    drawer: null,
    items: [
      { title: "Click Me" },
      { title: "Click Me 2" },
      { title: "Click Me 3" },
    ],
  }),
};
</script>
```

用`right`支柱将其放在右侧。

# 结论

我们添加临时抽屉，并用 Vuetify 把它放在屏幕的左边或右边。