# 使用 Quasar 库开发 Vue 应用程序——按钮对齐和大小调整

> 原文：<https://blog.devgenius.io/developing-vue-apps-with-the-quasar-library-button-alignment-and-sizing-7b9391c362e4?source=collection_archive---------9----------------------->

![](img/9e506022f2a61ba6c06953327caac471.png)

照片由 [rawkkim](https://unsplash.com/@rawkkim?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Quasar 是一个流行的 Vue UI 库，用于开发好看的 Vue 应用程序。

在本文中，我们将了解如何使用 Quasar UI 库创建 Vue 应用程序。

# 按钮对齐

我们可以使用`align`道具来对齐按钮中的文本。

例如，我们可以写:

```
<!DOCTYPE html>
<html>
  <head>
    <link
      href="https://fonts.googleapis.com/css?family=Roboto:100,300,400,500,700,900|Material+Icons"
      rel="stylesheet"
      type="text/css"
    />
    <link
      href="https://cdn.jsdelivr.net/npm/quasar@1.12.13/dist/quasar.min.css"
      rel="stylesheet"
      type="text/css"
    />
  </head>
  <body class="body--dark">
    <script src="https://cdn.jsdelivr.net/npm/vue@^2.0.0/dist/vue.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/quasar@1.12.13/dist/quasar.umd.min.js"></script>
    <div id="q-app">
      <q-layout
        view="lHh Lpr lFf"
        container
        style="height: 100vh;"
        class="shadow-2 rounded-borders"
      >
        <q-btn
          align="left"
          class="btn-fixed-width"
          color="primary"
          label="Align to left"
        ></q-btn>
        <q-btn
          align="right"
          class="btn-fixed-width"
          color="secondary"
          label="Align to right"
        ></q-btn>
        <q-btn
          align="between"
          class="btn-fixed-width"
          color="accent"
          label="Align between"
          icon="flight_takeoff"
        ></q-btn>
        <q-btn
          align="around"
          class="btn-fixed-width"
          color="brown-5"
          label="Align around"
          icon="lightbulb_outline"
        ></q-btn>
      </q-layout>
    </div>
    <script>
      new Vue({
        el: "#q-app",
        data: {}
      });
    </script>
  </body>
</html>
```

我们将`align`道具设置为我们想要对齐内容的方向。

我们可以用`size`道具设置按钮的大小:

```
<!DOCTYPE html>
<html>
  <head>
    <link
      href="https://fonts.googleapis.com/css?family=Roboto:100,300,400,500,700,900|Material+Icons"
      rel="stylesheet"
      type="text/css"
    />
    <link
      href="https://cdn.jsdelivr.net/npm/quasar@1.12.13/dist/quasar.min.css"
      rel="stylesheet"
      type="text/css"
    />
  </head>
  <body class="body--dark">
    <script src="https://cdn.jsdelivr.net/npm/vue@^2.0.0/dist/vue.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/quasar@1.12.13/dist/quasar.umd.min.js"></script>
    <div id="q-app">
      <q-layout
        view="lHh Lpr lFf"
        container
        style="height: 100vh;"
        class="shadow-2 rounded-borders"
      >
        <q-btn
          v-for="size in sizes"
          :key="size"
          color="primary"
          :size="size"
          :label="`Size ${size}`"
        >
        </q-btn>
      </q-layout>
    </div>
    <script>
      new Vue({
        el: "#q-app",
        data: {
          sizes: ["xs", "sm", "md", "lg", "xl"]
        }
      });
    </script>
  </body>
</html>
```

此外，我们可以用`padding`道具改变填充:

```
<!DOCTYPE html>
<html>
  <head>
    <link
      href="https://fonts.googleapis.com/css?family=Roboto:100,300,400,500,700,900|Material+Icons"
      rel="stylesheet"
      type="text/css"
    />
    <link
      href="https://cdn.jsdelivr.net/npm/quasar@1.12.13/dist/quasar.min.css"
      rel="stylesheet"
      type="text/css"
    />
  </head>
  <body class="body--dark">
    <script src="https://cdn.jsdelivr.net/npm/vue@^2.0.0/dist/vue.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/quasar@1.12.13/dist/quasar.umd.min.js"></script>
    <div id="q-app">
      <q-layout
        view="lHh Lpr lFf"
        container
        style="height: 100vh;"
        class="shadow-2 rounded-borders"
      >
        <q-btn
          v-for="size in sizes"
          :key="size"
          color="primary"
          :padding="size"
          :label="`Padding ${size}`"
        >
        </q-btn>
      </q-layout>
    </div>
    <script>
      new Vue({
        el: "#q-app",
        data: {
          sizes: ["xs", "sm", "md", "lg", "xl"]
        }
      });
    </script>
  </body>
</html>
```

我们可以添加一个带有工具提示的按钮:

```
<!DOCTYPE html>
<html>
  <head>
    <link
      href="https://fonts.googleapis.com/css?family=Roboto:100,300,400,500,700,900|Material+Icons"
      rel="stylesheet"
      type="text/css"
    />
    <link
      href="https://cdn.jsdelivr.net/npm/quasar@1.12.13/dist/quasar.min.css"
      rel="stylesheet"
      type="text/css"
    />
  </head>
  <body class="body--dark">
    <script src="https://cdn.jsdelivr.net/npm/vue@^2.0.0/dist/vue.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/quasar@1.12.13/dist/quasar.umd.min.js"></script>
    <div id="q-app">
      <q-layout
        view="lHh Lpr lFf"
        container
        style="height: 100vh;"
        class="shadow-2 rounded-borders"
      >
        <q-btn color="primary" label="With Tooltip" class="q-mt-md">
          <q-tooltip content-class="bg-accent">I'm a tooltip</q-tooltip>
        </q-btn>
      </q-layout>
    </div>
    <script>
      new Vue({
        el: "#q-app",
        data: {}
      });
    </script>
  </body>
</html>
```

如果我们悬停在按钮上，我们会看到显示的工具提示。

同样，我们可以用`full-width`类将按钮设置为全角:

```
<!DOCTYPE html>
<html>
  <head>
    <link
      href="https://fonts.googleapis.com/css?family=Roboto:100,300,400,500,700,900|Material+Icons"
      rel="stylesheet"
      type="text/css"
    />
    <link
      href="https://cdn.jsdelivr.net/npm/quasar@1.12.13/dist/quasar.min.css"
      rel="stylesheet"
      type="text/css"
    />
  </head>
  <body class="body--dark">
    <script src="https://cdn.jsdelivr.net/npm/vue@^2.0.0/dist/vue.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/quasar@1.12.13/dist/quasar.umd.min.js"></script>
    <div id="q-app">
      <q-layout
        view="lHh Lpr lFf"
        container
        style="height: 100vh;"
        class="shadow-2 rounded-borders"
      >
        <q-btn color="black" class="full-width" label="Full-width"></q-btn>
      </q-layout>
    </div>
    <script>
      new Vue({
        el: "#q-app",
        data: {}
      });
    </script>
  </body>
</html>
```

# 结论

我们可以用 Quasar 改变按钮大小和填充，并在我们的 Vue 应用程序中添加工具提示。