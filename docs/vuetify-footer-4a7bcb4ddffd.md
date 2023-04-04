# 虚拟化—页脚

> 原文：<https://blog.devgenius.io/vuetify-footer-4a7bcb4ddffd?source=collection_archive---------1----------------------->

![](img/0d2c8658c2a38f2871f2222e7e1e0dc0.png)

科林·梅纳德在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 脚注

我们可以用`v-footer`组件添加一个页脚。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-card height="150">
          <v-footer absolute class="font-weight-medium">
            <v-col class="text-center" cols="12">
              {{ new Date().getFullYear() }} —
              <strong>ABC Company</strong>
            </v-col>
          </v-footer>
        </v-card>
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

我们有带有`absolute`道具的`v-footer`组件来显示`v-card`底部的页脚。

# 无衬垫页脚

我们添加了`padless`道具来移除页脚的所有默认填充。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-footer padless class="font-weight-medium">
          <v-col class="text-center" cols="12">
            {{ new Date().getFullYear() }} —
            <strong>ABC Company</strong>
          </v-col>
        </v-footer>
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

# 公司页脚

我们可以创建一个带链接的页脚:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-footer color="primary lighten-1" padless>
          <v-row justify="center" no-gutters>
            <v-btn
              v-for="link in links"
              :key="link"
              color="white"
              text
              rounded
              class="my-2"
            >{{ link }}</v-btn>
            <v-col class="primary lighten-2 py-4 text-center white--text" cols="12">
              {{ new Date().getFullYear() }} —
              <strong>Vuetify</strong>
            </v-col>
          </v-row>
        </v-footer>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    links: ["Home", "About Us", "Contact Us"],
  }),
};
</script>
```

我们用`v-btn`组件呈现按钮。

我们添加了`v-col`来显示它下面的另一个条。

# 靛蓝页脚

我们还可以制作一个带有社交媒体按钮的页脚。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-footer dark padless>
          <v-card flat tile class="indigo lighten-1 white--text text-center">
            <v-card-text>
              <v-btn v-for="icon in icons" :key="icon" class="mx-4 white--text" icon>
                <v-icon size="24px">{{ icon }}</v-icon>
              </v-btn>
            </v-card-text> <v-card-text
              class="white--text pt-0"
            >Phasellus feugiat arcu sapien, et iaculis ipsum elementum sit amet. Mauris cursus commodo interdum. Praesent ut risus eget metus luctus accumsan id ultrices nunc. Sed at orci sed massa consectetur dignissim a sit amet dui. Duis commodo vitae velit et faucibus. Morbi vehicula lacinia malesuada. Nulla placerat augue vel ipsum ultrices, cursus iaculis dui sollicitudin.</v-card-text> <v-divider></v-divider> <v-card-text class="white--text">
              {{ new Date().getFullYear() }} —
              <strong>Vuetify</strong>
            </v-card-text>
          </v-card>
        </v-footer>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    icons: ["mdi-facebook", "mdi-twitter", "mdi-linkedin", "mdi-instagram"],
  }),
};
</script>
```

我们有`v-card-text`组件来显示图标和它下面的文本。

`dark`使背景变暗。

`padless`移除填充。

![](img/f7442934c52d31698891732adfbd5e73.png)

乔纳森·博尔巴在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以用 Vuetify 创建各种风格的页脚。