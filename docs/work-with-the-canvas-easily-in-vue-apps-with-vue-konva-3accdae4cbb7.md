# 使用 Vue Konva 在 Vue 应用程序中轻松处理画布

> 原文：<https://blog.devgenius.io/work-with-the-canvas-easily-in-vue-apps-with-vue-konva-3accdae4cbb7?source=collection_archive---------3----------------------->

![](img/f89677a7d169bb4a62fb6c9c7d712bdc.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上 [Kelli Tungay](https://unsplash.com/@kellitungay?utm_source=medium&utm_medium=referral) 拍摄的照片

借助 Vue Konva 库，我们可以在 Vue 应用程序中更轻松地使用 HTML 画布。

在本文中，我们将了解如何使用 Vue Konva 来简化 Vue 应用程序中的 HTML 画布。

# 装置

我们通过运行以下命令来安装 Vue Konva 库:

```
npm install vue-konva konva --save
```

然后，我们通过编写以下代码来注册插件:

```
import Vue from "vue";
import App from "./App.vue";
import VueKonva from "vue-konva";Vue.use(VueKonva);
Vue.config.productionTip = false;new Vue({
  render: (h) => h(App)
}).$mount("#app");
```

在`main.js`

# 简单的形状

现在我们可以用它来添加一些形状。

例如，我们可以通过编写以下内容向画布添加一些简单的形状:

```
<template>
  <v-stage :config="configKonva">
    <v-layer>
      <v-circle :config="configCircle"></v-circle>
    </v-layer>
  </v-stage>
</template><script>
export default {
  data() {
    return {
      configKonva: {
        width: 400,
        height: 400,
      },
      configCircle: {
        x: 200,
        y: 200,
        radius: 70,
        fill: "red",
        stroke: "black",
        strokeWidth: 4,
      },
    };
  },
};
</script>
```

我们添加了`v-stage`组件来存放画布内容。

然后我们将`v-circle`添加到`v-layer`中，并用设置对它们进行配置。

`config`道具有配置。

`width`和`height`有画布的宽度和高度。

`x`和`y`是圆心的 x 和 y 坐标。

`radius`是半径。

`fill`是圆的背景色。

`stroke`有边框颜色。

`strokeWidth`有边框宽度。

我们也可以使用 CDN 中的 Vue Konva 和 Konva:

```
<html>
  <head>
    <meta charset="utf-8" />
    <meta
      name="viewport"
      content="width=device-width, initial-scale=1, shrink-to-fit=no"
    />
    <meta http-equiv="x-ua-compatible" content="ie=edge" />
  </head>
  <body>
    <div id="app">
      <v-stage ref="stage" :config="configKonva">
        <v-layer ref="layer">
          <v-circle :config="configCircle"></v-circle>
        </v-layer>
      </v-stage>
    </div>
    <script src="https://unpkg.com/vue/dist/vue.js"></script>
    <script src="https://unpkg.com/konva@4.0.0/konva.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/vue-konva@2.1.6/umd/vue-konva.min.js"></script>
    <script>
      new Vue({
        el: "#app",
        data: {
          configKonva: {
            width: 400,
            height: 400
          },
          configCircle: {
            x: 200,
            y: 200,
            radius: 70,
            fill: "red",
            stroke: "black",
            strokeWidth: 4
          }
        }
      });
    </script>
  </body>
</html>
```

我们不需要像 NPM 版本那样注册插件。

代码的其余部分是相同的。

# 核心形状

Vue Konva 带有一些预建的形状。

包括`v-rect`、`v-circle`、`v-ellipse`、`v-line`、`v-image`、`v-text`、`v-text-path`、`v-star`、`v-label`、`v-path`、`v-regular-polygon`。

例如，我们可以通过编写以下内容来添加一些文本和一个矩形:

```
<template>
  <v-stage :config="configKonva">
    <v-layer>
      <v-text :config="{ text: 'Some text', fontSize: 15 }" />
      <v-rect
        :config="{
          x: 20,
          y: 50,
          width: 120,
          height: 120,
          fill: 'red',
          shadowBlur: 10,
        }"
      />
    </v-layer>
  </v-stage>
</template><script>
export default {
  data() {
    return {
      configKonva: {
        width: 400,
        height: 400,
      },
    };
  },
};
</script>
```

例如，我们可以写:

```
<template>
  <v-stage :config="configKonva">
    <v-layer>
      <v-line
        :config="{
          x: 20,
          y: 200,
          points: [0, 0, 220, 0, 160, 100],
          tension: 0.5,
          closed: true,
          stroke: 'black',
          fillLinearGradientStartPoint: { x: -50, y: -50 },
          fillLinearGradientEndPoint: { x: 50, y: 50 },
          fillLinearGradientColorStops: [0, 'green', 1, 'yellow'],
        }"
      />
    </v-layer>
  </v-stage>
</template><script>
export default {
  data() {
    return {
      configKonva: {
        width: 400,
        height: 400,
      },
    };
  },
};
</script>
```

使用`v-line`组件制作我们自己的形状，其中`points`数组具有点的`x`和`y`坐标。

`fillLinearGradientStartPoint`有渐变开始坐标。

`fillLinearGradientEndPoint`有渐变终点坐标。

`fillLinearGradientColorStops`具有渐变开始和结束的颜色。

# 结论

我们可以在 Vue Konva 的 Vue 应用程序中轻松地将基本形状添加到 HTNML 画布上。