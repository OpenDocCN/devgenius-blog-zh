# Vuetify —底部工作表内容

> 原文：<https://blog.devgenius.io/vuetify-bottom-sheet-content-879e16517a5d?source=collection_archive---------4----------------------->

![](img/dcdd6b94ccec1f566b7dea465344e38a.png)

乔希·威瑟斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 插入底板

可以插入一张底部的纸，这样桌面上的最大宽度为 70%。

用`width`道具可以减的更多。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row class="text-center">
      <v-col col="12">
        <div class="text-center">
          <v-bottom-sheet v-model="sheet" inset>
            <template v-slot:activator="{ on, attrs }">
              <v-btn color="orange" dark v-bind="attrs" v-on="on">Open Inset</v-btn>
            </template>
            <v-sheet class="text-center" height="200px">
              <v-btn class="mt-6" text color="error" @click="sheet = !sheet">close</v-btn>
              <div class="my-3">Lorem ipsum</div>
            </v-sheet>
          </v-bottom-sheet>
        </div>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    sheet: false,
  }),
};
</script>
```

我们添加了`inset`支柱，以减少底板的宽度。

# 音乐播放器

底层可以用来展示音乐播放器。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row class="text-center">
      <v-col col="12">
        <div class="text-center">
          <v-bottom-sheet inset>
            <template v-slot:activator="{ on, attrs }">
              <v-btn color="red" dark v-bind="attrs" v-on="on">Open Player</v-btn>
            </template>
            <v-card tile>
              <v-progress-linear :value="50" class="my-0" height="3"></v-progress-linear><v-list>
                <v-list-item>
                  <v-list-item-content>
                    <v-list-item-title>Violin Sonata No. 9 in A Major, Op. 47</v-list-item-title>
                    <v-list-item-subtitle>Ludwig van Beethoven</v-list-item-subtitle>
                  </v-list-item-content> <v-spacer></v-spacer> <v-list-item-icon>
                    <v-btn icon>
                      <v-icon>mdi-rewind</v-icon>
                    </v-btn>
                  </v-list-item-icon><v-list-item-icon :class="{ 'mx-5': $vuetify.breakpoint.mdAndUp }">
                    <v-btn icon>
                      <v-icon>mdi-pause</v-icon>
                    </v-btn>
                  </v-list-item-icon> <v-list-item-icon class="ml-0" :class="{ 'mr-3': $vuetify.breakpoint.mdAndUp }">
                    <v-btn icon>
                      <v-icon>mdi-fast-forward</v-icon>
                    </v-btn>
                  </v-list-item-icon>
                </v-list-item>
              </v-list>
            </v-card>
          </v-bottom-sheet>
        </div>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    sheet: false,
  }),
};
</script>
```

在用作内容容器的`v-bottom-sheet`中有`v-card`。

`v-list`内`v-card` hs 正文。

`v-list-item-content`有标题和副标题。

`v-list-item-icon`有倒带、暂停和快进按钮。

# 在列表中打开

我们可以在底部的工作表中显示一个列表。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row class="text-center">
      <v-col col="12">
        <div class="text-center">
          <v-bottom-sheet v-model="sheet">
            <template v-slot:activator="{ on, attrs }">
              <v-btn color="purple" dark v-bind="attrs" v-on="on">Open In</v-btn>
            </template>
            <v-list>
              <v-subheader>Open in</v-subheader>
              <v-list-item v-for="tile in tiles" :key="tile.title" @click="sheet = false">
                <v-list-item-avatar>
                  <v-avatar size="32px" tile>
                    <img
                      :src="`https://cdn.vuetifyjs.com/images/bottom-sheets/${tile.img}`"
                      :alt="tile.title"
                    />
                  </v-avatar>
                </v-list-item-avatar>
                <v-list-item-title>{{ tile.title }}</v-list-item-title>
              </v-list-item>
            </v-list>
          </v-bottom-sheet>
        </div>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    sheet: false,
    tiles: [
      { img: "keep.png", title: "Keep" },
      { img: "inbox.png", title: "Inbox" },
      { img: "hangouts.png", title: "Hangouts" },
      { img: "messenger.png", title: "Messenger" },
      { img: "google.png", title: "Google+" },
    ],
  }),
};
</script>
```

我们有在`v-list`中呈现为`v-list-item`的`tiles`。

`v-list`在`v-bottom-sheet`中。

因此，当我们打开底部的工作表时，会看到显示的列表。

![](img/1d68b699ef57fac78cf31bb431cb62e1.png)

[David Jorre](https://unsplash.com/@davidjorre?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以在底部表单中包含任何内容。