# 虚拟化—嵌套对话框

> 原文：<https://blog.devgenius.io/vuetify-nested-dialogs-d1aab08bbd16?source=collection_archive---------1----------------------->

![](img/cd05509bd2bec3588526d8c3779964b1.png)

照片由 [42 北](https://unsplash.com/@42north?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 加载器对话框

我们可以创建一个加载器对话框来显示里面的进度条。

为此，我们写道:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <div class="text-center">
          <v-btn
            :disabled="dialog"
            :loading="dialog"
            class="white--text"
            color="purple darken-2"
            @click="dialog = true"
          >Start loading</v-btn>
          <v-dialog v-model="dialog" hide-overlay persistent width="300">
            <v-card color="primary" dark>
              <v-card-text>
                Please wait
                <v-progress-linear indeterminate color="white" class="mb-0"></v-progress-linear>
              </v-card-text>
            </v-card>
          </v-dialog>
        </div>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    dialog: false,
  }),
  watch: {
    dialog(val) {
      if (!val) {
        return;
      }
      setTimeout(() => {
        this.dialog = false;
      }, 5000);
    },
  },
};
</script>
```

我们添加了一个对话框，显示带有`v-progress-linear`组件的进度条，

`indeterminate`道具使进度条移动，直到对话框消失。

# 嵌套对话框

我们可以在另一个对话框中打开对话框。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <div>
          <v-row justify="center">
            <v-btn color="primary" class="ma-2" dark @click="dialog = true">Open Dialog 1</v-btn>
            <v-btn color="primary" class="ma-2" dark @click="dialog2 = true">Open Dialog 2</v-btn>
            <v-btn color="primary" class="ma-2" dark @click="dialog3 = true">Open Dialog 3</v-btn>
            <v-menu bottom offset-y>
              <template v-slot:activator="{ on, attrs }">
                <v-btn class="ma-2" v-bind="attrs" v-on="on">A Menu</v-btn>
              </template>
            </v-menu>
            <v-dialog
              v-model="dialog"
              fullscreen
              hide-overlay
              transition="dialog-bottom-transition"
              scrollable
            >
              <v-card tile>
                <v-card-text>
                  <v-btn color="primary" dark class="ma-2" @click="dialog2 = !dialog2">Open Dialog 2</v-btn>
                  <v-list three-line subheader>
                    <v-subheader>User Controls</v-subheader>
                  </v-list>
                </v-card-text> <div style="flex: 1 1 auto;"></div>
              </v-card>
            </v-dialog> <v-dialog v-model="dialog2" max-width="500px">
              <v-card>
                <v-card-title>Dialog 2</v-card-title>
                <v-card-text>
                  <v-btn color="primary" dark @click="dialog3 = !dialog3">Open Dialog 3</v-btn>
                </v-card-text>
                <v-card-actions>
                  <v-btn color="primary" text @click="dialog2 = false">Close</v-btn>
                </v-card-actions>
              </v-card>
            </v-dialog>
            <v-dialog v-model="dialog3" max-width="500px">
              <v-card>
                <v-card-title>
                  <span>Dialog 3</span>
                  <v-spacer></v-spacer>
                  <v-menu bottom left>
                    <template v-slot:activator="{ on, attrs }">
                      <v-btn icon v-bind="attrs" v-on="on">
                        <v-icon>mdi-dots-vertical</v-icon>
                      </v-btn>
                    </template>
                  </v-menu>
                </v-card-title>
                <v-card-actions>
                  <v-btn color="primary" text @click="dialog3 = false">Close</v-btn>
                </v-card-actions>
              </v-card>
            </v-dialog>
          </v-row>
        </div>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    dialog: false,
    dialog2: false,
    dialog3: false,
    notifications: false,
    sound: true,
    widgets: false,
  }),
};
</script>
```

显示带有打开其他对话框的按钮的对话框。

它们都是用`v-dialog`组件创建的。

![](img/b65a9fb4539f81556ce58e6837c2a6c8.png)

照片由 [Orijit Chatterjee](https://unsplash.com/@orijit57?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以添加一个对话框来显示进度条，或者用 Vuetify 显示其他对话框。