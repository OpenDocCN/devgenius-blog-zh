# 使用 Quasar 库 Stepper 开发 Vue 应用程序

> 原文：<https://blog.devgenius.io/developing-vue-apps-with-the-quasar-library-stepper-e4b2d481676a?source=collection_archive---------5----------------------->

![](img/4f2e7d3bd0011cc46806208cc3e35634.png)

[Jukan Tateisi](https://unsplash.com/@tateisimikito?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Quasar 是一个流行的 Vue UI 库，用于开发好看的 Vue 应用程序。

在本文中，我们将了解如何使用 Quasar UI 库创建 Vue 应用程序。

# 跳舞者

Quasar 带有`q-stepper`组件，让我们可以创建多步内容。

例如，我们可以这样使用它:

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
            <q-step :name="1" title="Step 1" icon="settings" :done="step > 1">
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

`v-model`绑定到步骤号。

我们将`q-step`组件放在`q-stepper`中来显示步骤内容。

`name`道具具有步骤号，该步骤号与`step`反应属性进行比较，以确定显示哪个步骤。

标题栏中显示了`title`道具。

`caption`是步骤的副标题。

`icon`有图标名称。

而`done`是步骤图标和文本高亮显示时的状态。

`disable`支柱禁用步进器。

`navigation`槽有显示在步进器底部的内容，让我们可以导航这些步骤。

`$ref.stepper.next()`移动到下一步，`$ref.stepper.previous()`移动到上一步。

我们可以用`vertical`道具让步进器垂直显示:

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
            vertical
            v-model="step"
            ref="stepper"
            color="primary"
            animated
          >
            <q-step :name="1" title="Steo 1" icon="settings" :done="step > 1">
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

# 结论

我们可以使用 Quasar 的`q-stepper`组件将步进器组件添加到我们的 Vue 应用程序中。