# 使用 Quasar 库开发 Vue 应用程序—页面和进度条

> 原文：<https://blog.devgenius.io/developing-vue-apps-with-the-quasar-library-page-and-progress-bar-832fc61bd7f1?source=collection_archive---------2----------------------->

![](img/5831802abb93fc30524ae35951a48b53.png)

Giorgio Trovato 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Quasar 是一个流行的 Vue UI 库，用于开发好看的 Vue 应用程序。

在本文中，我们将了解如何使用 Quasar UI 库创建 Vue 应用程序。

# 页面滚动器

我们可以用`q-page-container`组件添加一个可滚动的页面组件。

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
        <q-header elevated>
          <q-toolbar>
            <q-avatar>
              <img src="https://cdn.quasar.dev/logo/svg/quasar-logo.svg" />
            </q-avatar>
            <q-toolbar-title>
              App
            </q-toolbar-title>
          </q-toolbar>
        </q-header> <q-page-container>
          <q-page padding>
            <p v-for="n in 15" :key="n">
              Lorem ipsum
            </p> <q-page-scroller
              position="bottom-right"
              :scroll-offset="150"
              :offset="[18, 18]"
            >
              <q-btn fab icon="keyboard_arrow_up" color="accent" />
            </q-page-scroller>
          </q-page>
        </q-page-container>
      </q-layout>
    </div> <script>
      new Vue({
        el: "#q-app",
        data: {
          drawerLeft: true
        },
        methods: {}
      });
    </script>
  </body>
</html>
```

我们有用作布局容器的`q-layout`组件。

`q-header`组件添加了一个标题。

`q-toolbar`添加一个工具栏。

`q-page-container`拥有页面容器。`q-page`保存页面内容。

`q-page-scroller`观察滚动位置，显示`q-btn`是我们滚动到页面底部。

`position`是滚轮组件的位置。

此外，我们可以将`q-page-scroller`添加到顶部，并将`position`设置为`top`:

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
        <q-header elevated>
          <q-toolbar>
            <q-avatar>
              <img src="https://cdn.quasar.dev/logo/svg/quasar-logo.svg" />
            </q-avatar>
            <q-toolbar-title>
              App
            </q-toolbar-title>
          </q-toolbar>
        </q-header><q-page-container>
          <q-page padding>
            <p v-for="n in 15" :key="n">
              Lorem ipsum
            </p><q-page-scroller
              expand
              position="top"
              :scroll-offset="150"
              :offset="[0, 0]"
            >
              <div
                class="col cursor-pointer q-pa-sm bg-accent text-white text-center"
              >
                Scroll back up
              </div>
            </q-page-scroller>
          </q-page>
        </q-page-container>
      </q-layout>
    </div> <script>
      new Vue({
        el: "#q-app",
        data: {
          drawerLeft: true
        },
        methods: {}
      });
    </script>
  </body>
</html>
```

# Ajax 酒吧

我们可以用 ajax bar 组件显示 Ajax 请求的进度。

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
        style="height: 100vh;"
        class="shadow-2 rounded-borders"
      >
        <q-ajax-bar
          ref="bar"
          position="bottom"
          color="accent"
          size="10px"
          skip-hijack
        />
      </q-layout>
    </div> <script>
      new Vue({
        el: "#q-app",
        data: {},
        methods: {
          trigger() {
            const bar = this.$refs.bar;
            bar.start(); this.timer = setTimeout(() => {
              if (this.$refs.bar) {
                bar.stop();
              }
            }, Math.random() * 3000 + 1000);
          }
        },
        mounted() {
          this.trigger();
        }
      });
    </script>
  </body>
</html>
```

我们添加`q-ajax-bar`组件来添加加载栏。

`position`设置为`bottom`将横条置于底部。

`trigger`方法显示带有`bar.start`方法的条。

并且`setTimeout`回调调用`bar.stop`来停止显示该条。

# 结论

我们可以用 Quasar 库在我们的 Vue 应用程序中添加一个可滚动的页面和加载栏。