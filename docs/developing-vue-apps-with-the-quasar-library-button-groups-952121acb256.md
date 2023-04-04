# 使用 Quasar 库开发 Vue 应用程序—按钮组

> 原文：<https://blog.devgenius.io/developing-vue-apps-with-the-quasar-library-button-groups-952121acb256?source=collection_archive---------9----------------------->

![](img/1e14d5f0e8e7c1a55e4b2e1637ed7f68.png)

照片由[普里西拉·杜·普里兹](https://unsplash.com/@priscilladupreez?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Quasar 是一个流行的 Vue UI 库，用于开发好看的 Vue 应用程序。

在本文中，我们将了解如何使用 Quasar UI 库创建 Vue 应用程序。

# 按钮组

我们可以添加按钮组来将按钮组合在一起。

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
        <q-btn-group push>
          <q-btn push label="First" icon="timeline"></q-btn>
          <q-btn push label="Second" icon="visibility"></q-btn>
          <q-btn push label="Third" icon="update"></q-btn>
        </q-btn-group>
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

我们在`q-btn-group`组件中添加按钮。

`push`变更设计。

我们必须在`q-btn-group`和`q-btn`上使用相同的设计道具。

此外，我们可以使用`spread`道具在屏幕上展开按钮:

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
        <q-btn-group spread>
          <q-btn color="purple" label="First" icon="timeline"></q-btn>
          <q-btn color="purple" label="Second" icon="visibility"></q-btn>
        </q-btn-group>
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

此外，我们可以在按钮旁边添加一个下拉菜单，方法是:

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
        <q-btn-group rounded>
          <q-btn rounded color="primary" label="One" ></q-btn> <q-btn rounded color="primary" label="Two" ></q-btn> <q-btn-dropdown
            auto-close
            rounded
            color="primary"
            label="Three"
            split
          >
            <q-list padding style="width: 250px;">
              <q-item clickable>
                <q-item-section avatar>
                  <q-avatar icon="folder" color="purple" text-color="white" ></q-avatar>
                </q-item-section>
                <q-item-section>
                  <q-item-label>Photos</q-item-label>
                  <q-item-label caption>February 22, 2016</q-item-label>
                </q-item-section>
                <q-item-section side>
                  <q-icon name="info" color="amber" ></q-avatar>
                </q-item-section>
              </q-item> <q-item clickable>
                <q-item-section avatar>
                  <q-avatar icon="folder" color="purple" text-color="white" ></q-avatar>
                </q-item-section>
                <q-item-section>
                  <q-item-label>Videos</q-item-label>
                  <q-item-label caption>London</q-item-label>
                </q-item-section>
                <q-item-section side>
                  <q-icon name="info" color="amber" ></q-icon>
                </q-item-section>
              </q-item> <q-separator inset ></q-separator>
              <q-item-label header>Files</q-item-label> <q-item clickable>
                <q-item-section avatar>
                  <q-avatar icon="assignment" color="teal" text-color="white" />
                </q-item-section>
                <q-item-section>
                  <q-item-label>London</q-item-label>
                  <q-item-label caption>March 1st, 2020</q-item-label>
                </q-item-section>
                <q-item-section side>
                  <q-icon name="info" color="amber"></q-icon>
                </q-item-section>
              </q-item> <q-item clickable>
                <q-item-section avatar>
                  <q-avatar
                    icon="assignment"
                    color="teal"
                    text-color="white"
                  ></q-avatar>
                </q-item-section>
                <q-item-section>
                  <q-item-label>Paris</q-item-label>
                  <q-item-label caption>January 22nd, 2020</q-item-label>
                </q-item-section>
                <q-item-section side>
                  <q-icon name="info" color="amber"></q-icon>
                </q-item-section>
              </q-item>
            </q-list>
          </q-btn-dropdown>
        </q-btn-group>
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

我们添加了`q-btn-dropdown`组件来添加一个带有下拉菜单的按钮。

我们添加`q-list`组件来添加下拉列表。

`q-separator`分隔列表中的项目。

# 结论

我们可以使用 Quasar 将按钮组添加到 Vue 应用程序中。