# BootstrapVue —自定义工具提示

> 原文：<https://blog.devgenius.io/bootstrapvue-customizing-tooltips-ef5c4837d720?source=collection_archive---------16----------------------->

![](img/60a26fcc78f9d2bd4f2edd4e8df00cd8.png)

玛丽-米歇尔·布沙尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

为了制作好看的 Vue 应用，我们需要设计组件的样式。

为了使我们的生活更容易，我们可以使用内置样式的组件。

在本文中，我们看看如何自定义工具提示。

# 配置

我们可以用修改器改变`v-b-tooltip`的位置来改变位置。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-button v-b-tooltip.hover.bottomright title="Tooltip title">hover</b-button>
  </div>
</template><script>
export default {
  name: "App"
};
</script><style>
#app {
  height: 100vh;
  width: 100vw;
  margin: 50vw 50vh;
}
</style>
```

我们有`bottomright`道具，所以工具提示被放在按钮的右下角。

# 聚焦触发器

`focus`触发器呈现的是`a`标签，而不是`button`标签。

所以`tabindex='0'`必须包含在内。

因此，我们不得不写:

```
<template>
  <div id="app">
    <b-button href="#" tabindex="0" v-b-tooltip.focus title="Tooltip">Link button</b-button>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

# 禁用元素

我们可以从禁用按钮触发工具提示，而是从其包装器元素触发。

例如，我们可以写:

```
<template>
  <div id="app">
    <span id="disabled" tabindex="0">
      <b-button disabled>Disabled button</b-button>
    </span>
    <b-tooltip target="disabled">Disabled</b-tooltip>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们将`id`而不是`b-button`放在 span 上，这样当我们悬停在按钮上时就可以触发工具提示。

# b-工具提示组件用法

如果我们使用`b-tooltip`组件，我们可以对工具提示中的内容进行样式化。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-button id="tooltip">button</b-button>
    <b-tooltip target="tooltip">Hello
      <b>World</b>
    </b-tooltip>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们用`b`标签将单词“World”加粗。

# 变体和自定义类别

`b-tooltip`拿着`variant`道具让我们换变体。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-button id="tooltip">button</b-button>
    <b-tooltip target="tooltip" variant="info">hello</b-tooltip>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们将`variant`设置为`'info'`，所以工具提示是绿色的。

此外，我们可以添加`custom-class`属性来设置自定义类。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-button id="tooltip">button</b-button>
    <b-tooltip target="tooltip" custom-class="tooltip">hello</b-tooltip>
  </div>
</template><script>
export default {
  name: "App"
};
</script><style>
.tooltip {
  font-weight: bold;
}
</style>
```

那么工具提示中的文本是粗体的，因为我们将`custom-class`设置为`'tooltip'`。

# 以编程方式显示和隐藏工具提示

我们可以设置`show`道具来以编程方式显示和隐藏工具提示。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-button @click="show = !show">button</b-button>
    <b-button id="tooltip">button with tooltip</b-button>
    <b-tooltip :show='show' target="tooltip" custom-class="tooltip">hello</b-tooltip>
  </div>
</template><script>
export default {
  name: "App",
  data(){
    return {
      show: false
    }
  }
};
</script>
```

我们有一个按钮来切换`show`值。

`show`道具被添加到`b-tooltip`中，这样我们就可以看到`show`在`true`时的提示。

我们还可以提交`'open'`和`'close'`事件来打开和关闭工具提示。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-button @click="open">open</b-button>
    <b-button @click="close">close</b-button>
    <b-button id="tooltip">tooltip button</b-button>
    <b-tooltip target="tooltip" ref="tooltip">hello</b-tooltip>
  </div>
</template><script>
export default {
  name: "App",
  methods: {
    open() {
      this.$refs.tooltip.$emit("open");
    },
    close() {
      this.$refs.tooltip.$emit("close");
    }
  }
};
</script>
```

我们给`b-tooltip`组件分配了一个引用。

此外，我们添加了`open`和`close`方法来分别发出打开和关闭事件。

这将打开或关闭工具提示。

然后，当我们单击“打开”按钮时，工具提示将在“工具提示按钮”按钮旁边打开。

当我们单击“关闭”时，我们会关闭工具提示。

# 以编程方式禁用工具提示

我们可以用一个 prop 或 ref 事件来禁用工具提示。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-button id="tooltip">tooltip button</b-button>
    <b-tooltip target="tooltip" disabled>hello</b-tooltip>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

用`disable`道具禁用工具提示。

我们也可以用 ref 禁用工具提示。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-button [@click](http://twitter.com/click)="toggleDisable">toggle disable</b-button>
    <b-button id="tooltip">tooltip button</b-button>
    <b-tooltip ref="tooltip" target="tooltip">hello</b-tooltip>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      disabled: false
    };
  },
  methods: {
    toggleDisable() {
      this.disabled = !this.disabled;
      if (this.disabled) {
        this.$refs.tooltip.$emit("disable");
      } else {
        this.$refs.tooltip.$emit("enable");
      }
    }
  }
};
</script>
```

我们可以分别使用`this.$refs.tooltip.$emit(“disable”);`和`this.$refs.tooltip.$emit(“enable”);`调用来禁用或启用工具提示。

因此,“切换禁用”按钮将切换工具提示的禁用状态。

![](img/7b2facb0e358b16f74b51df5e43a81da.png)

照片由 [Abdulla Faiz](https://unsplash.com/@afaiz?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以通过编程来启用或禁用工具提示。

为此，我们可以发出事件或设置道具。

工具提示的位置也可以改变。