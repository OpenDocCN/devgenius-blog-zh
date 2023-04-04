# 使用 Quasar 库开发 Vue 应用程序——活动列表项目和表格

> 原文：<https://blog.devgenius.io/developing-vue-apps-with-the-quasar-library-active-list-items-and-tables-70b3ae896b02?source=collection_archive---------2----------------------->

![](img/05bb177e4677c4f91c7540147feb965d.png)

[主播李](https://unsplash.com/@anchorlee?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Quasar 是一个流行的 Vue UI 库，用于开发好看的 Vue 应用程序。

在本文中，我们将了解如何使用 Quasar UI 库创建 Vue 应用程序。

# 活动列表项目

我们可以通过向`q-item`组件添加`active`属性来添加活动列表项:

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
    <style>
      .example-item {
        height: 200px;
        width: 200px;
      }
    </style>
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
        <div class="q-pa-md">
          <q-list>
            <q-item clickable v-ripple active active-class="text-orange">
              <q-item-section avatar>
                <q-icon name="signal_wifi_off"></q-icon>
              </q-item-section>
              <q-item-section>Active</q-item-section>
              <q-item-section side>Side</q-item-section>
            </q-item> <q-item
              clickable
              v-ripple
              active
              active-class="bg-teal-1 text-grey-8"
            >
              <q-item-section avatar>
                <q-icon name="signal_wifi_off"></q-icon>
              </q-item-section>
              <q-item-section>Active class</q-item-section>
              <q-item-section side>Side</q-item-section>
            </q-item>
          </q-list>
        </div>
      </q-layout>
    </div>
    <script>
      new Vue({
        el: "#q-app"
      });
    </script>
  </body>
</html>
```

`active`道具激活列表项。

`active-class`设置为活动列表项的样式。

`q-item-label`组件让我们将文本添加到列表项中:

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
    <style>
      .example-item {
        height: 200px;
        width: 200px;
      }
    </style>
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
        <div class="q-pa-md">
          <q-list>
            <q-item>
              <q-item-section top avatar>
                <q-avatar
                  color="primary"
                  text-color="white"
                  icon="bluetooth"
                ></q-avatar>
              </q-item-section> <q-item-section>
                <q-item-label>Single line item</q-item-label>
                <q-item-label caption lines="2">
                  Secondary line text
                </q-item-label>
              </q-item-section> <q-item-section side top>
                <q-item-label caption>5 min ago</q-item-label>
                <q-icon name="star" color="yellow"></q-icon>
              </q-item-section>
            </q-item>
          </q-list>
        </div>
      </q-layout>
    </div>
    <script>
      new Vue({
        el: "#q-app"
      });
    </script>
  </body>
</html>
```

# 标记表

我们可以用`q-markup-table`组件添加一个表。

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
    <style>
      .example-item {
        height: 200px;
        width: 200px;
      }
    </style>
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
        <div class="q-pa-md">
          <q-markup-table>
            <template>
              <thead>
                <tr>
                  <th class="text-left">Dessert (100g serving)</th>
                  <th class="text-right">Calories</th>
                  <th class="text-right">Fat (g)</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td class="text-left">Frozen Yogurt</td>
                  <td class="text-right">159</td>
                  <td class="text-right">6</td>
                </tr>
                <tr>
                  <td class="text-left">Ice cream</td>
                  <td class="text-right">237</td>
                  <td class="text-right">9</td>
                </tr>
              </tbody>
            </template>
          </q-markup-table>
        </div>
      </q-layout>
    </div>
    <script>
      new Vue({
        el: "#q-app"
      });
    </script>
  </body>
</html>
```

UMD 构建需要`template`标签来正确显示表格。

这是因为浏览器会在 Vue 呈现表格之前解析 HTML。

# 结论

我们可以突出显示活动列表项，并使用 Quasar 添加带有我们自己标记的表格。