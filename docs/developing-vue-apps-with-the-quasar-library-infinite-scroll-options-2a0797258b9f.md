# 使用 Quasar 库开发 Vue 应用程序——无限滚动选项

> 原文：<https://blog.devgenius.io/developing-vue-apps-with-the-quasar-library-infinite-scroll-options-2a0797258b9f?source=collection_archive---------4----------------------->

![](img/7c8af589e4df946bac3d7ebb553024be.png)

格伦·卡斯滕斯-彼得斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Quasar 是一个流行的 Vue UI 库，用于开发好看的 Vue 应用程序。

在本文中，我们将了解如何使用 Quasar UI 库创建 Vue 应用程序。

# 反向无限滚动

我们可以用`reverse`道具反转无限卷轴的方向:

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
          <q-infinite-scroll @load="onLoad" reverse>
            <template slot="loading">
              <div class="row justify-center q-my-md">
                <q-spinner color="primary" name="dots" size="40px" />
              </div>
            </template> <div
              v-for="(item, index) in items"
              :key="index"
              class="caption q-py-sm"
            >
              <q-badge class="shadow-1">
                {{ items.length - index }}
              </q-badge>
              Lorem ipsum
            </div>
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

我们还可以在菜单中添加一个无限滚动容器:

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
          <q-btn color="brown" label="Menu">
            <q-menu
              anchor="bottom middle"
              self="top middle"
              :offset="[ 0, 8 ]"
              @show="scrollTarget = $refs.scrollTargetRef"
            >
              <q-item-label header>
                Notifications
              </q-item-label>
              <q-list
                ref="scrollTargetRef"
                class="scroll"
                style="max-height: 230px;"
              >
                <q-infinite-scroll
                  @load="onLoad"
                  :offset="250"
                  :scroll-target="scrollTarget"
                >
                  <q-item v-for="(item, index) in items" :key="index">
                    <q-item-section>
                      {{ index + 1 }}. Lorem ipsum
                    </q-item-section>
                  </q-item> <template v-slot:loading>
                    <div class="text-center q-my-md">
                      <q-spinner-dots
                        color="primary"
                        size="40px"
                      ></q-spinner-dots>
                    </div>
                  </template>
                </q-infinite-scroll>
              </q-list>
            </q-menu>
          </q-btn>
        </div>
      </q-layout>
    </div>
    <script>
      new Vue({
        el: "#q-app",
        data: {
          scrollTarget: undefined,
          items: [{}, {}, {}, {}, {}, {}, {}]
        },
        methods: {
          onLoad(index, done) {
            if (index > 1) {
              setTimeout(() => {
                if (this.items) {
                  this.items.push({}, {}, {}, {}, {}, {}, {});
                  done();
                }
              }, 2000);
            } else {
              setTimeout(() => {
                done();
              }, 200);
            }
          }
        }
      });
    </script>
  </body>
</html>
```

我们将`q-infinite-scroll`组件添加到`q-list`中，而`q-list`在`q-menu`中。

# 结论

我们可以用 Quasar 在我们的 Vue 应用程序中添加一个无限滚动容器。