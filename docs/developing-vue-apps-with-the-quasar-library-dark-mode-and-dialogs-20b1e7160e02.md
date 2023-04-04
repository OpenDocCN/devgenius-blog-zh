# 使用 Quasar 库开发 Vue 应用程序——黑暗模式和对话框

> 原文：<https://blog.devgenius.io/developing-vue-apps-with-the-quasar-library-dark-mode-and-dialogs-20b1e7160e02?source=collection_archive---------3----------------------->

![](img/2982707a45a8b4f1f6067e3af6056f52.png)

由[埃利奥特·恩格尔曼](https://unsplash.com/@elliottengelmann?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Quasar 是一个流行的 Vue UI 库，用于开发好看的 Vue 应用程序。

在本文中，我们将了解如何使用 Quasar UI 库创建 Vue 应用程序。

# 深色模式

我们可以使用`$q.dark.set`方法启用黑暗模式:

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
          this.$q.dark.set(true);
        }
      });
    </script>
  </body>
</html>
```

`true`将开启黑暗模式，`false`将关闭黑暗模式。

# 对话

我们可以用`$q.dialog`方法添加一个对话框:

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
      <div class="q-pa-md">
        <q-btn label="Confirm" color="primary" @click="confirm"></q-btn>
        <q-btn label="Prompt" color="primary" @click="prompt"></q-btn>
      </div>
    </div>
    <script>
      new Vue({
        el: "#q-app",
        data: {},
        methods: {
          confirm() {
            this.$q
              .dialog({
                title: "Confirm",
                message: "Would you like to turn on the wifi?",
                cancel: true,
                persistent: true
              })
              .onOk(() => {
                console.log("OK");
              })
              .onOk(() => {
                console.log("Second OK catcher");
              })
              .onCancel(() => {
                console.log("Cancel");
              })
              .onDismiss(() => {
                console.log("I am triggered on both OK and Cancel");
              });
          }, prompt() {
            this.$q
              .dialog({
                title: "Prompt",
                message: "What is your name?",
                prompt: {
                  model: "",
                  type: "text"
                },
                cancel: true,
                persistent: true
              })
              .onOk((data) => {
                console.log("OK, received", data);
              })
              .onCancel(() => {
                console.log("Cancel");
              })
              .onDismiss(() => {
                console.log("I am triggered on both OK and Cancel");
              });
          }
        }
      });
    </script>
  </body>
</html>
```

`title`属性设置标题。

`message`设置对话框消息。

`cancel`设置为`true`时增加取消按钮。

`persistent`使对话持久化。

我们可以用`onOK`、`onCancel`和`onDismiss`方法观察对话框发出的事件。

属性让我们显示一个带有提示的输入。

我们从带有提示对话框的`onOK`回调中的`data`参数获取数据。

我们可以将`dark`属性设置为`true`来启用黑暗模式。

# 单选、复选框和切换

我们可以添加对话框，让用户用单选按钮、复选框和开关来选择选项。

为了添加它们，我们写:

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
      <div class="q-pa-md">
        <q-btn label="Radio" color="primary" @click="radio"></q-btn>
        <q-btn label="Checkbox" color="primary" @click="checkbox"></q-btn>
        <q-btn label="Toggle" color="primary" @click="toggle"></q-btn>
      </div>
    </div>
    <script>
      new Vue({
        el: "#q-app",
        data: {},
        methods: {
          radio() {
            this.$q
              .dialog({
                title: "Options",
                message: "Choose an option:",
                options: {
                  type: "radio",
                  model: "opt1",
                  items: [
                    { label: "Option 1", value: "opt1", color: "secondary" },
                    { label: "Option 2", value: "opt2" },
                    { label: "Option 3", value: "opt3" }
                  ]
                },
                cancel: true,
                persistent: true
              })
              .onOk((data) => {
                console.log("OK, received", data);
              });
          },checkbox() {
            this.$q
              .dialog({
                title: "Options",
                message: "Choose your options:",
                options: {
                  type: "checkbox",
                  model: [],
                  items: [
                    { label: "Option 1", value: "opt1", color: "secondary" },
                    { label: "Option 2", value: "opt2" },
                    { label: "Option 3", value: "opt3" }
                  ]
                },
                cancel: true,
                persistent: true
              })
              .onOk((data) => {
                console.log("OK, received", data);
              });
          },toggle() {
            this.$q
              .dialog({
                title: "Options",
                message: "Choose your options:",
                options: {
                  type: "toggle",
                  model: [],
                  items: [
                    { label: "Option 1", value: "opt1", color: "secondary" },
                    { label: "Option 2", value: "opt2" },
                    { label: "Option 3", value: "opt3" }
                  ]
                },
                cancel: true,
                persistent: true
              })
              .onOk((data) => {
                console.log("OK, received", data);
              });
          }
        }
      });
    </script>
  </body>
</html>
```

我们只需添加`options.items`属性来添加选项。

我们将控件类型更改为用`options.type`属性显示。

我们从来自`onOK`回调的`data`参数中获取选中的项目。

# 结论

我们可以将黑暗模式和与类星体库的对话添加到我们的 Vue 应用程序中。