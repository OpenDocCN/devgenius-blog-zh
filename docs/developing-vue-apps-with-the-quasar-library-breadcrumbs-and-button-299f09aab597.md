# 使用 Quasar 库开发 Vue 应用程序——面包屑和按钮

> 原文：<https://blog.devgenius.io/developing-vue-apps-with-the-quasar-library-breadcrumbs-and-button-299f09aab597?source=collection_archive---------6----------------------->

![](img/1e990b1313dc00d41ca0069cdb3a5c7e.png)

保罗·吉尔摩在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Quasar 是一个流行的 Vue UI 库，用于开发好看的 Vue 应用程序。

在本文中，我们将了解如何使用 Quasar UI 库创建 Vue 应用程序。

# 面包屑

Quasar 带有`q-breadcrumbs`组件，可以让我们添加面包屑。

为了使用它，我们写:

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
        <q-breadcrumbs>
          <q-breadcrumbs-el label="Home"></q-breadcrumbs-el>
          <q-breadcrumbs-el label="Components"></q-breadcrumbs-el>
          <q-breadcrumbs-el label="Breadcrumbs"></q-breadcrumbs-el>
        </q-breadcrumbs>
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

我们添加`q-breadcrumbs`组件作为容器。

我们用`q-breadecrumbs-el`组件添加面包屑条目。

`label`是我们显示的文本。

我们可以用`icon`道具添加图标:

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
        <q-breadcrumbs>
          <q-breadcrumbs-el label="Home"></q-breadcrumbs-el>
          <q-breadcrumbs-el
            label="Components"
            icon="widgets"
          ></q-breadcrumbs-el>
          <q-breadcrumbs-el label="Breadcrumbs"></q-breadcrumbs-el>
        </q-breadcrumbs>
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

我们也可以把它加在一个`q-toolbar`里面:

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
        <q-toolbar>
          <q-btn flat round dense icon="assignment_ind" /> <q-toolbar-title>Quasar</q-toolbar-title> <q-btn flat round dense icon="sim_card" class="q-mr-xs" />
          <q-btn flat round dense icon="gamepad" />
        </q-toolbar>
        <q-toolbar inset>
          <q-breadcrumbs>
            <q-breadcrumbs-el label="Home"></q-breadcrumbs-el>
            <q-breadcrumbs-el
              label="Components"
              icon="widgets"
            ></q-breadcrumbs-el>
            <q-breadcrumbs-el
              label="Breadcrumbs"
            ></q-breadcrumbs-el> </q-breadcrumbs
        ></q-toolbar>
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

我们将`q-toolbar`组件和`q-breadcrumbs`组件放在一起。

`inset`使面包屑组件在工具栏内对齐。

# 纽扣

我们可以使用`q-btn`组件将按钮添加到我们的 Vue 应用程序中。

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
        <q-btn color="white" text-color="black" label="Standard"></q-btn>
        <q-btn color="primary" label="Primary"></q-btn>
        <q-btn color="secondary" label="Secondary"></q-btn>
        <q-btn color="amber" glossy label="Amber"></q-btn>
        <q-btn color="brown-5" label="Brown 5"></q-btn>
        <q-btn color="deep-orange" glossy label="Deep Orange"></q-btn>
        <q-btn color="purple" label="Purple"></q-btn>
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

我们添加`q-btn`组件来添加按钮。

`color`设置背景颜色。

`label`设置按钮文本。

`glossy`使按钮有光泽。

# 结论

我们可以使用 Quasar 将面包屑和按钮添加到 Vue 应用程序中。