# BootstrapVue —模式和对话框

> 原文：<https://blog.devgenius.io/bootstrapvue-modals-and-dialogs-9f7c4dac9fd5?source=collection_archive---------7----------------------->

![](img/a82de67c919b16ed704e0ef33e644017.png)

费利克斯·m·多恩在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

为了制作好看的 Vue 应用，我们需要设计组件的样式。

为了使我们的生活更容易，我们可以使用内置样式的组件。

在本文中，我们将看看如何使用模态和对话框组件。

# 禁用页脚按钮

我们可以用`cancel-disabled`和`ok-disabled`属性禁用页脚按钮。

它们分别禁用“取消”和“确定”按钮。

我们可以用`busy`道具同时禁用两者。

所以如果我们有:

```
<template>
  <div id="app">
    <b-button v-b-modal.modal>Open</b-button> <b-modal id="modal" title=" Modal" busy>
      <p>hello</p>
    </b-modal>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

那么这两个按钮都将变灰。

# 带槽的自定义渲染

我们可以定制带有槽的模态部分的呈现。

可以通过填充`header`插槽来定制接头。

可以通过填充默认插槽来定制主体。

并且可以通过填充页脚槽来定制页脚。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-button v-b-modal.modal>Open</b-button> <b-modal id="modal" title=" Modal" busy>
      <template v-slot:modal-header="{ close }">
        <b-button @click="close()">Close Modal</b-button>
        <h5>Header</h5>
      </template> <template v-slot:default="{ hide }">
        <p>Body</p>
        <b-button @click="hide()">Hide</b-button>
      </template> <template v-slot:modal-footer="{ ok, cancel, hide }">
        <p>Footer</p>
        <b-button @click="ok()">OK</b-button>
        <b-button @click="cancel()">Cancel</b-button>
        <b-button @click="hide('hide')">Hide</b-button>
      </template>
    </b-modal>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

插槽为我们提供了可以调用每个部分的方法。

`close`方法关闭了头部的模态。

`hide`隐藏体内情态。

页脚有`ok`、`cancel`和`hide`方法来关闭模态。

# 多模态

我们可以在一个页面上有多个模型。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-button v-b-modal.modal-1>Open First Modal</b-button> <b-modal id="modal-1" size="lg" title="First Modal" ok-only no-stacking>
      <p>First</p>
      <b-button v-b-modal.modal-2>Open Second Modal</b-button>
    </b-modal> <b-modal id="modal-2" title="Second Modal" ok-only>
      <p class="my-2">Second</p>
      <b-button v-b-modal.modal-3>Open Third Modal</b-button>
    </b-modal> <b-modal id="modal-3" title="Third Modal" ok-only>
      <p class="my-1">Third</p>
    </b-modal>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们有三种调式，一个接一个地打开。

当我们单击打开第一个模态时，我们打开第一个模态。

它有一个打开第二模态按钮来打开第二模态。

该模态具有打开第三模态按钮来打开第三模态。

当我们关闭最上面的一个，其余的也关闭了。

# 模态消息框

我们可以用`msgBoxOk`和`msgBoxConfirm`方法创建简单的消息框。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-button @click='openMsgBox'>Open Message Box</b-button>
  </div>
</template><script>
export default {
  name: "App",
  methods: {
    async openMsgBox(){
      const value  = await this.$bvModal.msgBoxOk('hello') 
      console.log(value);
    }
  }
};
</script>
```

我们添加了一个按钮来调用`openMsgBox`方法。

它打开一个消息框，其中包含我们传递的字符串中的单词`'hello'`。

它回报一个承诺。

当我们关闭它时，它将解析为`true`。

此外，我们可以传入一个带有一些选项的对象作为第二个参数。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-button @click="openMsgBox">Open Message Box</b-button>
  </div>
</template><script>
export default {
  name: "App",
  methods: {
    async openMsgBox() {
      const value = await this.$bvModal.msgBoxOk("hello", {
        title: "Confirmation",
        size: "sm",
        buttonSize: "sm",
        okVariant: "success",
        headerClass: "p-2 border-bottom-0",
        footerClass: "p-2 border-top-0",
        centered: true
      });
      console.log(value);
    }
  }
};
</script>
```

我们给`title`属性添加了一个标题。

`size`设置模态的大小。

`buttonSize`设置按钮大小。

`okVariant`设置确定按钮的样式。

`headerClass`设置标题的类别。

`footerClass`设置页脚的类别。

`centered`使模态垂直居中。

# 确认对话框

确认对话框与此类似，只是它还有一个取消按钮。

我们调用`msgBoxConfirm`方法而不是`msgBoxOk`来打开确认对话框。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-button @click="openMsgBox">Open Confirm Box</b-button>
  </div>
</template><script>
export default {
  name: "App",
  methods: {
    async openMsgBox() {
      const value = await this.$bvModal.msgBoxConfirm("hello", {
        title: "Confirmation",
        size: "sm",
        buttonSize: "sm",
        okVariant: "danger",
        okTitle: "YES",
        cancelTitle: "NO",
        footerClass: "p-2",
        hideHeaderClose: false,
        centered: true
      });
      console.log(value);
    }
  }
};
</script>
```

我们在选项中添加了`okTitle`和`cancelTitle`属性。

当我们点击取消按钮时，承诺将解析为`false`。

如果我们单击右上角的“x”，它将解析为`null`。

如果我们单击 OK 按钮，它将解析为`true`。

![](img/e2f3e03af524638f626d0667163a8aad.png)

照片由 [Aaron Burden](https://unsplash.com/@aaronburden?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以通过填充插槽来自定义模型的呈现。

此外，我们可以显示返回承诺的简单对话框。

根据关闭它的操作，它会解析为不同的值。