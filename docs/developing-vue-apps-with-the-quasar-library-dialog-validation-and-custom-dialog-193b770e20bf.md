# 使用 Quasar 库开发 Vue 应用程序——对话框验证和自定义对话框

> 原文：<https://blog.devgenius.io/developing-vue-apps-with-the-quasar-library-dialog-validation-and-custom-dialog-193b770e20bf?source=collection_archive---------4----------------------->

![](img/26ffb8c1cd055170cf0188185f25c9b0.png)

本·怀特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Quasar 是一个流行的 Vue UI 库，用于开发好看的 Vue 应用程序。

在本文中，我们将了解如何使用 Quasar UI 库创建 Vue 应用程序。

# 对话验证

我们可以用`prompt.isValid`属性为提示对话框添加验证:

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
        <q-btn label="Prompt" color="primary" @click="prompt"></q-btn>
      </div>
    </div>
    <script>
      new Vue({
        el: "#q-app",
        data: {},
        methods: {
          prompt() {
            this.$q
              .dialog({
                title: "Prompt",
                message: "What is your name? (Minimum 3 characters)",
                prompt: {
                  model: "",
                  isValid: (val) => val.length > 2,
                  type: "text"
                },
                cancel: true,
                persistent: true
              })
              .onOk((data) => {
                console.log(data);
              });
          }
        }
      });
    </script>
  </body>
</html>
```

我们将`isValid`设置为接受输入值并返回验证条件的函数。

这也可以用于单选按钮和复选框对话框。

# HTML 对话框

我们可以显示一个本地 HTML 对话框，其中的`html`属性设置为`true`。

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
      <div class="q-pa-md">
        <q-btn
          label="Show HTML Dialog"
          color="primary"
          @click="showDialog"
        ></q-btn>
      </div>
    </div>
    <script>
      new Vue({
        el: "#q-app",
        data: {},
        methods: {
          showDialog() {
            this.$q
              .dialog({
                title: "Alert<em>!</em>",
                message:
                  '<em>I can</em> <span class="text-red">use</span> <strong>HTML</strong>',
                html: true
              })
              .onOk(() => {
                console.log("OK");
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

我们只需将`title`和`message`属性设置为我们想要呈现的 HTML。

这意味着如果在 HTML 中植入恶意代码，就可以执行跨站点脚本攻击。

我们可以用`q-dialog`组件呈现一个自定义对话框:

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
        <q-btn label="Show Dialog" color="primary" @click="showDialog"></q-btn>
      </div>
    </div>
    <script>
      const CustomComponent = {
        template: `
        <q-dialog ref="dialog" @hide="onDialogHide">
          <q-card class="q-dialog-plugin">
            <q-card-section>
              Hello
            </q-card-section>
            <q-card-actions align="right">
              <q-btn color="primary" label="OK" @click="onOKClick"></q-btn>
              <q-btn color="primary" label="Cancel" @click="onCancelClick"></q-btn>
            </q-card-actions>
          </q-card>
        </q-dialog>
        `,
        methods: {
          show() {
            this.$refs.dialog.show();
          },
          hide() {
            this.$refs.dialog.hide();
          },
          onDialogHide() {
            this.$emit("hide");
          },
          onOKClick() {
            this.$emit("ok");
            this.hide();
          },
          onCancelClick() {
            this.hide();
          }
        }
      }; new Vue({
        el: "#q-app",
        data: {},
        methods: {
          showDialog() {
            this.$q
              .dialog({
                component: CustomComponent,
                parent: this,
                text: "something"
              })
              .onOk(() => {
                console.log("OK");
              })
              .onCancel(() => {
                console.log("Cancel");
              })
              .onDismiss(() => {
                console.log("Called on OK or Cancel");
              });
          }
        }
      });
    </script>
  </body>
</html>
```

我们添加了`CustomComponent`组件和`q-dialog`组件来添加一个对话框。

我们有`show`和`hide`方法来显示和隐藏对话框。

当我们关闭对话框时，我们发出`hide`事件来关闭它。

`onOKClick`发出`ok`事件来触发`onOK`回调。

# 结论

我们可以用 Quasar 库添加带有各种定制的对话框。