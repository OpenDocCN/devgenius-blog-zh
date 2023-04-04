# 使用 Quasar 库开发 Vue 应用程序—步进器标题选项

> 原文：<https://blog.devgenius.io/developing-vue-apps-with-the-quasar-library-stepper-header-options-90f7b19c007c?source=collection_archive---------9----------------------->

![](img/82001401247b8bf3bf27d35382e00150.png)

由[塞巴斯蒂安·塞克](https://unsplash.com/@sebaseck?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Quasar 是一个流行的 Vue UI 库，用于开发好看的 Vue 应用程序。

在本文中，我们将了解如何使用 Quasar UI 库创建 Vue 应用程序。

# 步进器标题选项

我们可以将步进器标题更改为不同的样式。

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
          <q-stepper v-model="step" ref="stepper" color="primary" animated>
            <q-step
              :name="1"
              :error="step < 3"
              title="Step 1"
              icon="settings"
              :done="step > 1"
            >
              step 1
            </q-step> <q-step
              :name="2"
              title="Step 2"
              caption="Optional"
              icon="create_new_folder"
              :done="step > 2"
            >
              step 2
            </q-step> <q-step :name="3" title="Step 3" icon="assignment" disable>
              This step won't show up because it is disabled.
            </q-step> <q-step :name="4" title="Step 4" icon="add_comment">
              step 4
            </q-step> <template v-slot:navigation>
              <q-stepper-navigation>
                <q-btn
                  @click="$refs.stepper.next()"
                  color="primary"
                  :label="step === 4 ? 'Finish' : 'Continue'"
                >
                </q-btn>
                <q-btn
                  v-if="step > 1"
                  flat
                  color="primary"
                  @click="$refs.stepper.previous()"
                  label="Back"
                  class="q-ml-sm"
                >
                </q-btn>
              </q-stepper-navigation>
            </template>
          </q-stepper>
        </div>
      </q-layout>
    </div>
    <script>
      new Vue({
        el: "#q-app",
        data: {
          step: 1
        }
      });
    </script>
  </body>
</html>
```

将`error`支柱加入到`q-step`组件中。

`error`道具让我们设置何时显示错误图标和红色文本的条件。

我们还可以使用`alternative-labels`道具将标签样式更改为图标堆叠在标签文本上方显示:

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
          <q-stepper
            alternative-labels
            v-model="step"
            ref="stepper"
            color="primary"
            animated
          >
            <q-step :name="1" title="Step 1" icon="settings" :done="step > 1">
              step 1
            </q-step><q-step
              :name="2"
              title="Step 2"
              icon="create_new_folder"
              :done="step > 2"
            >
              step 2
            </q-step> <q-step :name="3" title="Step 3" icon="assignment" disable>
              This step won't show up because it is disabled.
            </q-step> <q-step :name="4" title="Step 4" icon="add_comment">
              step 4
            </q-step> <template v-slot:navigation>
              <q-stepper-navigation>
                <q-btn
                  @click="$refs.stepper.next()"
                  color="primary"
                  :label="step === 4 ? 'Finish' : 'Continue'"
                >
                </q-btn>
                <q-btn
                  v-if="step > 1"
                  flat
                  color="primary"
                  @click="$refs.stepper.previous()"
                  label="Back"
                  class="q-ml-sm"
                >
                </q-btn>
              </q-stepper-navigation>
            </template>
          </q-stepper>
        </div>
      </q-layout>
    </div>
    <script>
      new Vue({
        el: "#q-app",
        data: {
          step: 1
        }
      });
    </script>
  </body>
</html>
```

我们还可以用`inactive-color`、`active-color`和`done-color`道具改变图标的颜色，以改变这些状态下的标签颜色:

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
          <q-stepper v-model="step" ref="stepper" color="primary" animated>
            <q-step
              done-color="deep-orange"
              active-color="purple"
              inactive-color="secondary"
              :name="1"
              title="Step 1"
              icon="settings"
              :done="step > 1"
            >
              step 1
            </q-step> <q-step
              :name="2"
              title="Step 2"
              icon="create_new_folder"
              :done="step > 2"
            >
              step 2
            </q-step> <q-step :name="3" title="Step 3" icon="assignment" disable>
              This step won't show up because it is disabled.
            </q-step> <q-step :name="4" title="Step 4" icon="add_comment">
              step 4
            </q-step> <template v-slot:navigation>
              <q-stepper-navigation>
                <q-btn
                  @click="$refs.stepper.next()"
                  color="primary"
                  :label="step === 4 ? 'Finish' : 'Continue'"
                >
                </q-btn>
                <q-btn
                  v-if="step > 1"
                  flat
                  color="primary"
                  @click="$refs.stepper.previous()"
                  label="Back"
                  class="q-ml-sm"
                >
                </q-btn>
              </q-stepper-navigation>
            </template>
          </q-stepper>
        </div>
      </q-layout>
    </div>
    <script>
      new Vue({
        el: "#q-app",
        data: {
          step: 1
        }
      });
    </script>
  </body>
</html>
```

# 结论

我们可以用 Quasar 的`q-stepper`组件添加具有各种选项的步进器。