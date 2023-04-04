# 使用 Quasar 库开发 Vue 应用程序—加载指示器

> 原文：<https://blog.devgenius.io/developing-vue-apps-with-the-quasar-library-loading-indicator-dd249cf5e25b?source=collection_archive---------7----------------------->

![](img/eb67c85afd38fa91c5d9d8add461c6a6.png)

由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上 [Battlecreek 咖啡烘焙师](https://unsplash.com/@battlecreekcoffeeroasters?utm_source=medium&utm_medium=referral)拍摄的照片

Quasar 是一个流行的 Vue UI 库，用于开发好看的 Vue 应用程序。

在本文中，我们将了解如何使用 Quasar UI 库创建 Vue 应用程序。

# 装载指示器

我们可以用`$q.loading`对象添加一个装载指示器:

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
      <div class="q-pa-md"></div>
    </div>
    <script>
      new Vue({
        el: "#q-app",
        data: {},
        beforeMount() {
          this.$q.loading.show({
            delay: 400
          });
          setTimeout(() => {
            this.$q.loading.hide();
          }, 3000);
        }
      });
    </script>
  </body>
</html>
```

我们调用带有`delay`属性的`$q.loading.show`方法来延迟加载指示器的显示。

数字以毫秒为单位。

然后我们用`$q.loading.hide`的方法隐藏它。

我们可以用`message`属性添加一条消息:

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
      <div class="q-pa-md"></div>
    </div>
    <script>
      new Vue({
        el: "#q-app",
        data: {},
        beforeMount() {
          this.$q.loading.show({
            delay: 400,
            message:
              'Some important <b>process</b> is in progress.<br/><span class="text-primary">Hang on...</span>'
          });
          setTimeout(() => {
            this.$q.loading.hide();
          }, 3000);
        }
      });
    </script>
  </body>
</html>
```

我们可以将其设置为 HTML。

我们可以将`sanitize`属性添加到对象中来转义 HTML 代码。

我们可以添加具有更多属性的更多自定义:

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
      <div class="q-pa-md"></div>
    </div>
    <script>
      new Vue({
        el: "#q-app",
        data: {},
        beforeMount() {
          this.$q.loading.show({
            spinner: Quasar.QSpinnerFacebook,
            spinnerColor: "yellow",
            spinnerSize: 140,
            backgroundColor: "purple",
            message: "Some important process is in progress. Hang on...",
            messageColor: "black"
          });
          setTimeout(() => {
            this.$q.loading.hide();
          }, 3000);
        }
      });
    </script>
  </body>
</html>
```

`spinner`有装载微调器的图标，

`spinnerColor`设置微调器颜色。

`spinnerSize`设置微调器大小。

`backgroundColor`设置覆盖的背景颜色。

`messageColor`设置消息的颜色。

# 装载杆

我们可以用`$q.loadingBar`对象添加一个装载栏。

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
      <div class="q-pa-md"></div>
    </div>
    <script>
      new Vue({
        el: "#q-app",
        data: {},
        beforeMount() {
          this.$q.loadingBar.start(); setTimeout(() => {
            this.$q.loadingBar.stop();
          }, 3000);
        }
      });
    </script>
  </body>
</html>
```

我们调用`start`来显示它，调用`stop`来停止它。

我们也可以调用`this.$q.loadingBar.increment(value)`来改变进度值。

此外，我们可以使用`LoadingBar.setDefaults`方法更改选项:

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
      <div class="q-pa-md"></div>
    </div>
    <script>
      new Vue({
        el: "#q-app",
        data: {},
        beforeMount() {
          Quasar.LoadingBar.setDefaults({
            color: "purple",
            size: "15px",
            position: "bottom"
          });
          this.$q.loadingBar.start(); setTimeout(() => {
            this.$q.loadingBar.stop();
          }, 3000);
        }
      });
    </script>
  </body>
</html>
```

我们设置`color`、`size`和`position`来设置这些样式。

# 结论

我们可以通过 Quasar 的加载栏插件在我们的 Vue 应用程序中添加各种风格的加载指示器。