# Vuetify —线性进度条自定义

> 原文：<https://blog.devgenius.io/vuetify-linear-progress-bar-customization-34096e61ee81?source=collection_archive---------3----------------------->

![](img/a9c6e858476c2feaea01acb57e4982e7.png)

照片由[凯尔·格伦](https://unsplash.com/@kylejglenn?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 不确定查询和确定查询

我们可以使用`query`状态来控制`indeterminate`支柱的真度。

例如，我们可以写:

```
<template>
  <div class="text-center">
    <v-progress-linear v-model="value" :active="show" :indeterminate="query" :query="true"></v-progress-linear>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      value: 0,
      query: false,
      show: true,
      interval: 0,
    };
  }, mounted() {
    this.queryAndIndeterminate();
  }, beforeDestroy() {
    clearInterval(this.interval);
  }, methods: {
    queryAndIndeterminate() {
      this.query = true;
      this.show = true;
      this.value = 0; setTimeout(() => {
        this.query = false; this.interval = setInterval(() => {
          if (this.value === 100) {
            clearInterval(this.interval);
            this.show = false;
            return setTimeout(this.queryAndIndeterminate, 2000);
          }
          this.value += 25;
        }, 1000);
      }, 2500);
    },
  },
};
</script>
```

我们将`this.query`状态设置为`false`，这样进度条就不会永远显示。

相反，它会根据我们设置为`v-model`的值的`this.value`来激活工具条。

# 自定义颜色

我们可以设置`color`和`background-color`道具来设置前景色和背景色:

```
<template>
  <div class="text-center">
    <v-progress-linear background-color="pink lighten-3" color="pink lighten-1" value="15"></v-progress-linear>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  data() {
    return {};
  },
};
</script>
```

# 圆形杆

我们可以添加`rounded`道具使进度条变圆:

```
<template>
  <div class="text-center">
    <v-progress-linear rounded value="15"></v-progress-linear>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  data() {
    return {};
  },
};
</script>
```

# 流媒体吧

这个`stream`道具让酒吧像小溪一样移动。

例如，我们可以写:

```
<template>
  <div class="text-center">
    <v-progress-linear buffer-value="0" stream value="15"></v-progress-linear>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  data() {
    return {};
  },
};
</script>
```

我们添加`stream`道具，让虚线的非填充部分移动。

# 条纹酒吧

我们可以添加`striped`道具，使填充的部分显示出条纹:

```
<template>
  <div class="text-center">
    <v-progress-linear buffer-value="0" striped value="15"></v-progress-linear>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  data() {
    return {};
  },
};
</script>
```

# 反向杆

我们可以用`reverse`道具使填充部分从右向左显示:

```
<template>
  <div class="text-center">
    <v-progress-linear buffer-value="0" reverse value="15"></v-progress-linear>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  data() {
    return {};
  },
};
</script>
```

# 工具栏加载器

`absolute`道具让我们在工具栏上显示`v-progress-linear`组件。

例如，我们可以写:

```
<template>
  <v-card class="mx-auto mt-6" width="344">
    <v-system-bar>
      <v-spacer></v-spacer>
      <v-icon>mdi-square</v-icon>
      <v-icon>mdi-circle</v-icon>
      <v-icon>mdi-triangle</v-icon>
    </v-system-bar>
    <v-toolbar>
      <v-btn icon>
        <v-icon>mdi-arrow-left</v-icon>
      </v-btn>
      <v-toolbar-title>App</v-toolbar-title>
      <v-progress-linear
        :active="loading"
        :indeterminate="loading"
        absolute
        bottom
        color="deep-purple accent-4"
      ></v-progress-linear>
      <v-spacer></v-spacer>
      <v-btn icon>
        <v-icon>mdi-dots-vertical</v-icon>
      </v-btn>
    </v-toolbar> <v-container style="height: 282px;">
      <v-row class="fill-height" align="center" justify="center">
        <v-scale-transition>
          <div v-if="!loading" class="text-center">
            <v-btn color="primary" @click="loading = true">Start loading</v-btn>
          </div>
        </v-scale-transition>
      </v-row>
    </v-container>
  </v-card>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    loading: false,
  }), watch: {
    loading(val) {
      if (!val) return; setTimeout(() => (this.loading = false), 5000);
    },
  },
};
</script>
```

我们有带有`absolute`和`bottom`道具的`v-progress-linear`组件，使其显示在工具栏的底部。

`v-progress-linear`组件在`v-toolbar`中，所以我们以这种方式显示它。

我们有`loading`状态来决定它什么时候加载或者不加载。

# 狭槽

我们可以将自己的内容添加到默认位置:

```
<template>
  <div>
    <v-progress-linear v-model="progress" color="blue-grey" height="25">
      <template v-slot="{ value }">
        <strong>{{ Math.ceil(value) }}%</strong>
      </template>
    </v-progress-linear>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    progress: 20,
  }),
};
</script>
```

`value`是`v-model`的值。

# 结论

我们可以用各种方式自定义一个线性进度条。