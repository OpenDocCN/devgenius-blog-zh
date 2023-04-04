# 使用 Quasar 库开发 Vue 应用程序—顶部栏

> 原文：<https://blog.devgenius.io/developing-vue-apps-with-the-quasar-library-top-bars-82a193b2ec80?source=collection_archive---------8----------------------->

![](img/71d1a2a8488c3d63adf8b462288d0290.png)

在[退溅](https://unsplash.com?utm_source=medium&utm_medium=referral)时通过[压下](https://unsplash.com/@anhdung?utm_source=medium&utm_medium=referral)进行 GFP 照相

Quasar 是一个流行的 Vue UI 库，用于开发好看的 Vue 应用程序。

在本文中，我们将了解如何使用 Quasar UI 库创建 Vue 应用程序。

# 酒吧

我们可以添加一个带有`q-bar`组件的顶栏。

例如，我们可以通过编写以下代码来添加一个 Windows 样式的顶栏:

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
      href="https://use.fontawesome.com/releases/v5.0.13/css/all.css"
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
        <q-bar>
          <div class="cursor-pointer">File</div>
          <div class="cursor-pointer">Edit</div>
          <div class="cursor-pointer gt-xs">View</div>
          <div class="cursor-pointer gt-xs">Window</div>
          <div class="cursor-pointer">Help</div>
          <q-space></q-space>
          <q-btn dense flat icon="minimize"></q-btn>
          <q-btn dense flat icon="crop_square"></q-btn>
          <q-btn dense flat icon="close"></q-btn>
        </q-bar>
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

我们添加带有 div 的`q-bar`组件来添加顶部的栏项目。

当我们将鼠标悬停在元素上时，我们可以使用`cursor-pointer`将光标样式改为指针。

`q-space`将 div 与按钮分开。

我们还有:

```
<link
      href="https://use.fontawesome.com/releases/v5.0.13/css/all.css"
      rel="stylesheet"
      type="text/css"
    />
```

添加字体牛逼图标。

我们可以通过编写以下内容来添加一个 Mac OS 风格的顶部栏:

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
      href="https://use.fontawesome.com/releases/v5.0.13/css/all.css"
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
        <q-bar>
          <div class="text-weight-bold">
            App
          </div>
          <div class="cursor-pointer gt-md">File</div>
          <div class="cursor-pointer gt-md">Edit</div>
          <div class="cursor-pointer gt-md">View</div>
          <div class="cursor-pointer gt-md">Window</div>
          <div class="cursor-pointer gt-md">Help</div>
          <q-space></q-space>
          <q-btn dense flat icon="airplay" class="gt-xs"></q-btn>
          <q-btn dense flat icon="battery_charging_full"></q-btn>
          <q-btn dense flat icon="wifi"></q-btn>
          <div>9:41</div>
          <q-btn dense flat icon="search"></q-btn>
          <q-btn dense flat icon="list"></q-btn>
        </q-bar>
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

我们只是把图标改成苹果风格的。

为了添加一个 Android 风格的顶栏，我们写道:

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
      href="https://use.fontawesome.com/releases/v5.0.13/css/all.css"
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
        <q-bar dense class="bg-black text-white">
          <div>mobi-net</div>
          <q-icon name="email"></q-icon>
          <q-space></q-space>
          <q-icon name="bluetooth"></q-icon>
          <q-icon name="signal_wifi_4_bar"></q-icon>
          <q-icon name="signal_cellular_4_bar"></q-icon>
          <div class="gt-xs">100%</div>
          <q-icon name="battery_full"></q-icon>
          <div>10:00AM</div>
        </q-bar>
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

我们直接添加`q-icon`组件，而不是将图标添加到按钮中。

我们可以通过编写以下代码来添加一个 iOS 风格的顶栏:

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
      href="https://use.fontawesome.com/releases/v5.0.13/css/all.css"
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
      <div class="q-pa-md q-gutter-sm">
        <q-bar dense class="bg-teal text-white">
          <q-icon name="signal"></q-icon>
          <div>net</div>
          <div>4G</div>
          <q-space></q-space>
          <q-icon name="wifi"></q-icon>
          <q-icon name="near_me"></q-icon>
          <div>100%</div>
          <q-icon name="batteryFull"></q-icon>
        </q-bar>
      </div>
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

我们只是把 iOS 的图标换了。

# 结论

我们可以用 Quasar 轻松地在 Vue 应用程序中添加各种风格的顶栏。