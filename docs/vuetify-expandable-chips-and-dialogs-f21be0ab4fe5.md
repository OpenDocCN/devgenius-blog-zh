# 虚拟化—可扩展芯片和对话框

> 原文：<https://blog.devgenius.io/vuetify-expandable-chips-and-dialogs-f21be0ab4fe5?source=collection_archive---------2----------------------->

![](img/eadd88d88298c12e5ab680a6f7a78b7f.png)

苏阿德·卡玛丁在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 可扩展芯片

一个芯片可以与`v-menu`相结合，为一个芯片启用一组动作。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-card max-width="400" class="mx-auto">
          <v-row class="px-6 py-3" align="center">
            <span class="mr-4">To</span>
            <v-menu v-model="menu" bottom right transition="scale-transition" origin="top left">
              <template v-slot:activator="{ on }">
                <v-chip pill v-on="on">
                  <v-avatar left>
                    <v-img src="https://cdn.vuetifyjs.com/images/john.png"></v-img>
                  </v-avatar>John Leider
                </v-chip>
              </template>
              <v-card width="300">
                <v-list dark>
                  <v-list-item>
                    <v-list-item-avatar>
                      <v-img src="https://cdn.vuetifyjs.com/images/john.png"></v-img>
                    </v-list-item-avatar>
                    <v-list-item-content>
                      <v-list-item-title>John Smith</v-list-item-title>
                      <v-list-item-subtitle>john@smith.com</v-list-item-subtitle>
                    </v-list-item-content>
                    <v-list-item-action>
                      <v-btn icon @click="menu = false">
                        <v-icon>mdi-close-circle</v-icon>
                      </v-btn>
                    </v-list-item-action>
                  </v-list-item>
                </v-list>
                <v-list>
                  <v-list-item @click="() => {}">
                    <v-list-item-action>
                      <v-icon>mdi-briefcase</v-icon>
                    </v-list-item-action>
                    <v-list-item-subtitle>john@gmail.com</v-list-item-subtitle>
                  </v-list-item>
                </v-list>
              </v-card>
            </v-menu>
          </v-row>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    menu: false,
  }),
};
</script>
```

我们有包裹在 T2 外面的 T1。

`v-chip`有一些文字和一个头像。

另外，当我们点击芯片时,`v-list-item`会显示列表。

`v-on="on"`指令让我们切换菜单，因为`on`包含了点击监听器。

此外，我们有一个闭合圆形按钮，当我们单击它时，可以将`menu`设置为`false`。

这样，菜单将关闭。

# 对话

`v-dialog`让我们显示一个对话框。

为了使用它，我们写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-row justify="center">
          <v-btn color="primary" dark @click.stop="dialog = true">Open Dialog</v-btn> <v-dialog v-model="dialog" max-width="290">
            <v-card>
              <v-card-title class="headline">Title</v-card-title>
              <v-card-text>Lorem ipsum.</v-card-text>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="green darken-1" text @click="dialog = false">Cancel</v-btn>
                <v-btn color="green darken-1" text @click="dialog = false">OK</v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-row>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    dialog: false,
  }),
};
</script>
```

用`v-dialog`组件创建一个对话框。

`v-model`绑定到`dialog`状态，控制对话框的打开和关闭。

当我们点击按钮时，我们还有`v-btn`来设置`dialog`到`false`。

当我们点击打开对话框按钮时，它会将`dialog`设置为`true`，这样当我们点击它时，对话框就会打开。

![](img/1ff47dc814997482cedf3d67a56ed45e.png)

[Ben White](https://unsplash.com/@benwhitephotography?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

# 结论

我们可以让芯片在点击时显示菜单。

同样，我们可以用`v-dialog`组件创建一个对话框。