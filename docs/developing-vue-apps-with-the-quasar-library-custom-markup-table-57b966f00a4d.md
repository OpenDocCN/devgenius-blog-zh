# 使用 Quasar 库开发 Vue 应用程序—自定义标记表

> 原文：<https://blog.devgenius.io/developing-vue-apps-with-the-quasar-library-custom-markup-table-57b966f00a4d?source=collection_archive---------6----------------------->

![](img/88e1d4b3700aa83375d4f5803fbd1f72.png)

本·赫尔希[在](https://unsplash.com/@benhershey?utm_source=medium&utm_medium=referral) [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Quasar 是一个流行的 Vue UI 库，用于开发好看的 Vue 应用程序。

在本文中，我们将了解如何使用 Quasar UI 库创建 Vue 应用程序。

# 标记表

我们可以用`separator`属性改变添加到单元格中的分隔符。

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
        <div class="q-pa-md">
          <q-markup-table separator="cell">
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

将`separator`道具添加到`q-markup-table`组件中。

`cell`为单元格添加垂直和水平边框。

其他值包括`horizontal`、`vertical`和`none`。

我们可以添加`dark`道具，将文本和分隔符改为白色，这样它们就可以显示在深色背景上:

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
        <div class="q-pa-md">
          <q-markup-table dark class="bg-indigo-8">
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

我们可以用 Quasar 的内置类定制表头:

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
        <div class="q-pa-md">
          <q-markup-table>
            <template>
              <thead class="bg-teal">
                <tr>
                  <th colspan="5">
                    <div class="row no-wrap items-center">
                      <q-img
                        style="width: 70px;"
                        :ratio="1"
                        class="rounded-borders"
                        src="https://cdn.quasar.dev/img/donuts.png"
                      >
                      </q-img> <div class="text-h4 q-ml-md text-white">Sweets</div>
                    </div>
                  </th>
                </tr>
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

我们在`th`元素中有一个 div，我们将`colspan`设置为 5，使其跨越整行。

# 结论

我们可以用 Quasar 的`q-markup-table`组件给我们的 Vue 应用添加一个表格。