# BootstrapVue —打开和关闭模式

> 原文：<https://blog.devgenius.io/bootstrapvue-open-and-close-modals-489a6cd0f908?source=collection_archive---------8----------------------->

![](img/62b3631724ae2d63fcac41025c0438a9.png)

由 [Wolfgang Hasselmann](https://unsplash.com/@wolfgang_hasselmann?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

为了制作好看的 Vue 应用，我们需要设计组件的样式。

为了使我们的生活更容易，我们可以使用内置样式的组件。

在本文中，我们将看看如何打开和关闭模态组件。

# `The v-b-modal`指令

我们可以使用`v-b-modal`来打开一个模态。

它可以接受一个修饰符或 ID 字符串作为模态 ID。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-button v-b-modal.modal>open</b-button>
    <b-modal id="modal" title="Hello">
      <p>Hello!</p>
    </b-modal>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们也可以写:

```
<template>
  <div id="app">
    <b-button v-b-modal="'modal'">open</b-button>
    <b-modal id="modal" title="Hello">
      <p>Hello!</p>
    </b-modal>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们必须用引号将字符串作为`v-b-modal`的值传入。

两个示例中的按钮都打开了模态。

# 以编程方式打开和关闭模式

我们可以使用`this.$bvModal.show()`和`this.$bvModal.hide()`分别打开和关闭模态。

当我们注册 BootstrapVue 插件或`ModalPlugin`时，它们就可用了。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-button @click="$bvModal.show('modal')">Open</b-button> <b-modal id="modal" hide-footer>
      <template v-slot:modal-title>Hello</template>
      <div>
        <h3>Hello!</h3>
      </div>
      <b-button block @click="$bvModal.hide('modal')">Close</b-button>
    </b-modal>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们有了用`$bvModal.show`方法打开的`b-modal`组件。

我们传入模态的 ID 来打开它。

同样，我们调用了`$bvModal.hide`来隐藏模态。

我们也在那里传递 ID。

# 显示、隐藏和切换方法

BootstrapVue 型号也可以用`show`方法打开。

可以用`hide`方法隐藏。

并且可以用`toggle`方法打开和关闭。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-button id="show-btn" @click="showModal">Open</b-button>
    <b-button id="toggle-btn" @click="toggleModal">Toggle</b-button> <b-modal ref="modal" hide-footer title="Hello">
      <div>
        <h3>Hello</h3>
      </div>
      <b-button block @click="hideModal">Close</b-button>
      <b-button block @click="toggleModal">Toggle</b-button>
    </b-modal>
  </div>
</template><script>
export default {
  name: "App",
  methods: {
    showModal() {
      this.$refs["modal"].show();
    },
    hideModal() {
      this.$refs["modal"].hide();
    },
    toggleModal() {
      this.$refs["modal"].toggle("#toggle-btn");
    }
  }
};
</script>
```

我们有一些方法叫做模态的 ref 来打开和关闭它。

`b-modal`的`ref`道具设置参考名称。

然后在方法中，我们可以使用`this.$refs`通过它的引用名来获取模态。

一旦我们访问了 ref，我们就可以调用 ref 上的`show`、`hide`和`toggle`方法。

对于`toggle`方法，我们必须为让我们切换模态的按钮传递选择器。

# v 型车

`b-modal`组件可以通过`v-model`指令绑定到一个模型。

它会将模态的可见状态绑定到状态变量。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-button @click="modalShow = !modalShow">Open</b-button> <b-modal v-model="modalShow">{{modalShow}}</b-modal>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      modalShow: false
    };
  }
};
</script>
```

我们有`modalShow`状态，我们用`v-modal`将它绑定到模态状态。

如果打开，`modalShow`将成为`true`，否则将成为`false`。

所以当我们打开模态的时候，内容会是‘真’的。

# 作用域插槽方法

我们可以在`$root`上发出`bv::show::modal`、`bv::hide::modal`和`bv::toggle::modal`来控制模态的打开和关闭。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-button @click="showModal" ref="btnShow">Open</b-button>
    <b-button @click="toggleModal" ref="btnToggle">Toggle</b-button> <b-modal id="modal">
      <div class="d-block">Hello</div>
      <b-button @click="hideModal">Close</b-button>
      <b-button @click="toggleModal">Toggle</b-button>
    </b-modal>
  </div>
</template><script>
export default {
  name: "App",
  methods: {
    showModal() {
      this.$root.$emit("bv::show::modal", "modal", "#btnShow");
    },
    hideModal() {
      this.$root.$emit("bv::hide::modal", "modal", "#btnShow");
    },
    toggleModal() {
      this.$root.$emit("bv::toggle::modal", "modal", "#btnToggle");
    }
  }
};
</script>
```

我们在模态之外有 2 个按钮来打开和切换模态。

我们在每个按钮上设置了 ref，这样我们就可以将它们用作`$emit`方法的第三个参数。

这样，我们可以控制模态的打开状态。

第二个参数是模态的 ref。

一旦事件被发出，我们就可以显示、隐藏或切换模态。

`bv::show::modal`事件显示了它发出时的模态。

`bv::hide::modal`事件在发出时隐藏了模态。

而`bv::toggle::modal`事件在发出时切换模态。

![](img/4627926718fa98072b64323c97f1df87.png)

照片由 [niko photos](https://unsplash.com/@niko_photos?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以控制模型和如何以各种方式打开和关闭，包括发出事件和调用内置方法。