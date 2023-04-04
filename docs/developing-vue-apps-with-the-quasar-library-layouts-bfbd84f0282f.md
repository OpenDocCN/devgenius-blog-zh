# 使用 Quasar 库开发 Vue 应用程序—布局

> 原文：<https://blog.devgenius.io/developing-vue-apps-with-the-quasar-library-layouts-bfbd84f0282f?source=collection_archive---------5----------------------->

![](img/f00e3a3582a8a311430cd825c12df85d.png)

马蒂·芬尼在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Quasar 是一个流行的 Vue UI 库，用于开发好看的 Vue 应用程序。

在本文中，我们将了解如何使用 Quasar UI 库创建 Vue 应用程序。

# 布局

我们可以用`q-layout`组件创建一个简单的布局。

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
  </head> <body class="body--dark">
    <script src="https://cdn.jsdelivr.net/npm/vue@^2.0.0/dist/vue.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/quasar@1.12.13/dist/quasar.umd.min.js"></script>
    <div id="q-app">
      <q-layout
        view="lHh Lpr lFf"
        container
        style="height: 400px;"
        class="shadow-2 rounded-borders"
      >
        <q-drawer
          v-model="drawerLeft"
          :width="150"
          :breakpoint="700"
          behavior="desktop"
          bordered
          content-class="bg-grey-3"
        >
          <q-scroll-area class="fit">
            <div class="q-pa-sm">
              <div v-for="n in 50" :key="n">Drawer {{ n }} / 50</div>
            </div>
          </q-scroll-area>
        </q-drawer>
      </q-layout>
    </div> <script>
      new Vue({
        el: "#q-app",
        data: {
          drawerLeft: true
        },
        methods: {},
        watch: {
          ["$q.screen"]: {
            handler() {
              this.drawerLeft = this.$q.screen.width > 700;
            },
            deep: true
          }
        }
      });
    </script>
  </body>
</html>
```

我们添加了`q-layout`组件，将其用作布局容器。

然后我们在`q-layout`里面添加`q-drawer`来添加一个导航抽屉。

在 Vue 实例中，我们观察`$q.screen`的`width`属性的值来获得屏幕的宽度。

而我们只有在`this.$q.screen.width`大于 700 的时候才设置`drawerLeft`为`true`来显示抽屉。

让我们创建一个可滚动的容器。

此外，我们可以分别使用`q-header`和`q-page-container`组件添加标题和页面内容。

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
  </head> <body class="body--dark">
    <script src="https://cdn.jsdelivr.net/npm/vue@^2.0.0/dist/vue.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/quasar@1.12.13/dist/quasar.umd.min.js"></script>
    <div id="q-app">
      <q-layout
        view="lHh Lpr lFf"
        container
        style="height: 400px;"
        class="shadow-2 rounded-borders"
      >
        <q-header reveal elevated>
          <q-toolbar>
            <q-btn
              flat
              round
              dense
              icon="menu"
              @click="drawerLeft = !drawerLeft"
            /><q-toolbar-title>
              App
            </q-toolbar-title>
        </q-header> <q-drawer
          v-model="drawerLeft"
          :width="150"
          :breakpoint="700"
          behavior="desktop"
          bordered
          content-class="bg-grey-3"
        >
          <q-scroll-area class="fit">
            <div class="q-pa-sm">
              <div v-for="n in 50" :key="n">Drawer {{ n }} / 50</div>
            </div>
          </q-scroll-area>
        </q-drawer> <q-page-container>
          <q-page padding style="padding-top: 66px;">
            <p v-for="n in 15" :key="n">
              Lorem ipsum
            </p> <q-page-sticky expand position="top">
              <q-toolbar class="bg-accent text-white">
                <q-avatar>
                  <img src="https://cdn.quasar.dev/logo/svg/quasar-logo.svg" />
                </q-avatar>
                <q-toolbar-title>
                  Page Title
                </q-toolbar-title>
              </q-toolbar>
            </q-page-sticky>
          </q-page>
        </q-page-container>
      </q-layout>
    </div> <script>
      new Vue({
        el: "#q-app",
        data: {
          drawerLeft: true
        },
        methods: {},
      });
    </script>
  </body>
</html>
```

我们在`q-drawer`上方添加`q-header`组件来添加标题。

我们有`q-page-container`来添加页面容器。

`q-page-sticky`是保存页眉的容器。

`expand`使其可扩展。

`p`元素是显示在`q-page-sticky`组件下面的内容。

# 结论

我们可以将带有各种组件的布局添加到 Vue 应用程序中。