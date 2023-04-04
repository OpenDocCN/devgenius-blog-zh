# 使用 Quasar 库开发 Vue 应用程序——响应图像和无限滚动

> 原文：<https://blog.devgenius.io/developing-vue-apps-with-the-quasar-library-responsive-images-and-infinite-scroll-7c9075789672?source=collection_archive---------6----------------------->

![](img/e4e1e79d2cbdab542d7ac74710cc63c3.png)

照片由[伊尔努尔·卡利穆林](https://unsplash.com/@kalimullin?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

Quasar 是一个流行的 Vue UI 库，用于开发好看的 Vue 应用程序。

在本文中，我们将了解如何使用 Quasar UI 库创建 Vue 应用程序。

# 响应图像

我们可以用`srcset`道具添加响应图像:

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
          <q-img
            src="https://cdn.quasar.dev/img/image-src.png"
            srcset="https://cdn.quasar.dev/img/image-1x.png 300w,
                  https://cdn.quasar.dev/img/image-2x.png 2x,
                  https://cdn.quasar.dev/img/image-3x.png 3x,
                  https://cdn.quasar.dev/img/image-4x.png 4x"
            style="height: 280px; max-width: 300px;"
          >
          </q-img>
        </div>
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

我们将`srcset`属性设置为一个逗号分隔的图像 URL 列表。

# 无限卷轴

我们可以用`q-infinite-scroll`组件添加一个无限滚动容器。

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
          <q-infinite-scroll @load="onLoad" :offset="250">
            <div v-for="(item, index) in items" :key="index" class="caption">
              <p>
                Lorem ipsum
              </p>
            </div>
            <template v-slot:loading>
              <div class="row justify-center q-my-md">
                <q-spinner-dots color="primary" size="40px" />
              </div>
            </template>
          </q-infinite-scroll>
        </div>
      </q-layout>
    </div>
    <script>
      new Vue({
        el: "#q-app",
        data: {
          items: [{}, {}, {}, {}, {}, {}, {}]
        },
        methods: {
          onLoad(index, done) {
            setTimeout(() => {
              if (this.items) {
                this.items.push({}, {}, {}, {}, {}, {}, {});
                done();
              }
            }, 2000);
          }
        }
      });
    </script>
  </body>
</html>
```

当我们滚动到容器底部时，我们监听`load`事件来运行代码以加载更多数据。

`offset`有超过底部的距离来触发装载。

我们填充了`loading`槽来显示项目加载时的微调器。

我们可以通过设置`scroll-target`道具来改变滚动容器:

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
          <div
            id="scroll-target-id"
            class="q-pa-md"
            style="max-height: 248px; overflow: auto;"
          >
            <q-infinite-scroll
              @load="onLoad"
              :offset="250"
              scroll-target="#scroll-target-id"
            >
              <div v-for="(item, index) in items" :key="index" class="caption">
                <p>
                  Lorem ipsum
                </p>
              </div>
              <template v-slot:loading>
                <div class="row justify-center q-my-md">
                  <q-spinner-dots color="primary" size="40px" />
                </div>
              </template>
            </q-infinite-scroll>
          </div>
        </div>
      </q-layout>
    </div>
    <script>
      new Vue({
        el: "#q-app",
        data: {
          items: [{}, {}, {}, {}, {}, {}, {}]
        },
        methods: {
          onLoad(index, done) {
            setTimeout(() => {
              if (this.items) {
                this.items.push({}, {}, {}, {}, {}, {}, {});
                done();
              }
            }, 2000);
          }
        }
      });
    </script>
  </body>
</html>
```

我们将它设置为我们想要设置为滚动容器的容器的选择器。

# 结论

我们可以使用 Quasar 在 Vue 应用程序中添加一个无限滚动容器。