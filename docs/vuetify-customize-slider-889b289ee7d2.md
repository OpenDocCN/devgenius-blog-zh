# 虚拟化—自定义滑块

> 原文：<https://blog.devgenius.io/vuetify-customize-slider-889b289ee7d2?source=collection_archive---------2----------------------->

![](img/ccb50ef772259c5c609093c01f62d622.png)

布鲁斯·沃林顿在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 禁用滑块

我们可以用`disabled`道具禁用滑块:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-slider disabled label="Disabled" value="30"></v-slider>
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

# 只读滑块

滑块也可以用`readonly`道具禁用。`disabled`和`readonly`的区别在于只读滑块看起来很普通。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-slider readonly label="Disabled" value="30"></v-slider>
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

# 核标准情报中心

图标可以添加到滑块。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-slider v-model="sound" prepend-icon="mdi-plus"></v-slider>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    sound: 0,
  }),
};
</script>
```

用`prepend-icon`道具在滑块左边添加一个图标。

我们可以用`append-icon`道具做同样的事情，将图标添加到右边。

此外，我们可以使用`@click:append`和`@click:prepend`指令来监听图标点击。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-slider
          v-model="zoom"
          prepend-icon="mdi-minus"
          append-icon="mdi-plus"
          @click:append="zoomIn"
          @click:prepend="zoomOut"
        ></v-slider>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    zoom: 100,
  }),
  methods: {
    zoomOut() {
      this.zoom = this.zoom - 10 || 0;
    },
    zoomIn() {
      this.zoom = this.zoom + 10 || 100;
    },
  },
};
</script>
```

我们将`zoomIn`和`zoomOut`方法设置为指令的值，以改变`this.zoom`的值和滑块值。

# 垂直滑块

`vertical`道具使滑块垂直显示。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-slider v-model="value" vertical label="Regular"></v-slider>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    value: 100,
  }),
};
</script>
```

# 拇指

我们可以显示滑块点的标签。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-slider v-model="value" thumb-label="always"></v-slider>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    value: 100,
  }),
};
</script>
```

`thumb-label`道具显示滑块。

我们可以通过填充`thumb-label`插槽来添加自定义标签:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-slider v-model="value" thumb-label="always">
          <template v-slot:thumb-label="{ value }">{{ value }}</template>
        </v-slider>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    value: 100,
  }),
};
</script>
```

# 结论

我们可以添加滑块，让用户设置我们想要的值。