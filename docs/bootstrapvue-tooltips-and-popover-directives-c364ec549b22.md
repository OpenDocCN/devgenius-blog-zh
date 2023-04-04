# BootstrapVue —工具提示和弹出指令

> 原文：<https://blog.devgenius.io/bootstrapvue-tooltips-and-popover-directives-c364ec549b22?source=collection_archive---------18----------------------->

![](img/26713b0798313695ddf2270cb974acfb.png)

Mx Moritz 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

为了制作好看的 Vue 应用，我们需要设计组件的样式。

为了使我们的生活更容易，我们可以使用内置样式的组件。

在本文中，我们看看如何使用`v-b-tooltip`和`v-b-popover`指令。

# v-b-工具提示指令

我们可以使用`html`修饰符让`v-b-tooltip`指令呈现 HTML。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-button v-b-tooltip.html title="Hello <b>World!</b>">html tooltip</b-button>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们有带有`v-b-tooltip`指令的`b-button`组件。

它添加了`html`修饰符，这样我们就可以呈现设置为`title`值的 HTML。

# 隐藏和显示带有$root 事件的工具提示

我们可以通过在`this.$root`上发出事件来隐藏和显示所有的工具提示。

例如，我们可以写:

```
this.$root.$emit('bv::hide::tooltip')
```

隐藏工具提示。

此外，我们可以写:

```
this.$root.$emit('bv::hide::tooltip', 'trigger-button-id')
```

通过 ID 隐藏工具提示。

要显示工具提示，我们可以写:

```
this.$root.$emit('bv::show::tooltip', 'trigger-button-id')
```

按 ID 显示工具提示。

# 使用$root 事件禁用和启用工具提示

同样，我们可以用`$root`事件禁用或启用工具提示。

例如，我们可以写:

```
this.$root.$emit('bv::disable::tooltip')
```

禁用所有工具提示。

此外，我们可以写:

```
this.$root.$emit('bv::disable::tooltip', 'trigger-button-id')
```

禁用具有特定 ID 的工具提示。

要启用它，我们可以写:

```
this.$root.$emit('bv::enable::tooltip', 'trigger-button-id')
```

# 使用$root 事件监听工具提示更改

如果我们在`mounted`钩子中添加一个监听器，我们可以监听工具提示事件。

例如，我们可以写:

```
export default {
  mounted() {
    this.$root.$on('bv::tooltip::show', bvEvent => {
      console.log('bvEvent:', bvEvent)
    })
  }
}
```

# 盘旋

我们可以使用`v-b-hover`指令在 hover 上运行回调。

例如，我们可以写:

```
<div v-b-hover="callback">content</div>
```

然后，当我们将鼠标悬停在 div 上时，回调就会运行。

我们可以编写如下回调函数:

```
<template>
  <div id="app">
    <div v-b-hover="callback">content</div>
  </div>
</template><script>
export default {
  name: "App",
  methods: {
    callback(hovered){
      console.log(hovered)
    }
  }
};
</script>
```

`callback`接受一个`hovered`参数，当我们将鼠标悬停在 div 上时是`true`，否则是`false`。

# 松饼

我们可以用`v-b-popover`指令给我们的应用程序添加一个弹出窗口。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-button v-b-popover.hover="'popover content'" title="Popover">Hover Me</b-button>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们设置`title`道具来设置标题。

内容是`v-b-popover`指令的值。

当我们悬停在按钮上时,`hover`修饰符会显示出来。

# 弹出定位

我们可以改变 popover 的位置。

定位的可能值有`top`、`topleft`、`topright`、`right`、`righttop`、`rightbottom`、`bottom`、`bottomleft`、`bottomright`、`left`、`lefttop`和`leftbottom`。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-button v-b-popover.hover.bottomright="'popover content'" title="Popover">Hover Me</b-button>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

由于`bottomright`修改器，弹出框将显示在右下角。

# 扳机

我们可以在除悬停之外的事件上触发弹出窗口的显示。

我们可以在点击、悬停或聚焦时显示它，

为了改变触发弹出窗口显示的事件，我们可以改变`v-b-popover`的修饰符。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-button v-b-popover.click="'popover content'" title="Popover">Click Me</b-button>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

现在，由于`click`修改器的原因，当我们点击按钮时弹出窗口就会显示出来。

# 下次点击时关闭弹出窗口

我们可以添加`blur`和`click`修改器，在下一次点击时关闭弹出窗口。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-button v-b-popover.click.blur="'popover content'" title="Popover">Click Me</b-button>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

现在，当我们点击“点击我”按钮时，弹出窗口将会打开和关闭。

# 标题和内容

我们可以用一个设置为`v-b-popover`值的对象来改变标题和内容。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-button v-b-popover="options">Hover Me</b-button>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      options: {
        title: "This is the <b>title</b>",
        content: "This is the <i>content<i>",
        html: true
      }
    };
  }
};
</script>
```

`title`和`content`设置在`options`对象中，而不是道具中。

`html`让我们显示 HTML 内容。

![](img/3d31ec06d59e427bf46c92d46a717d48.png)

林赛·莫伊在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以触发根事件来启用/禁用或切换工具提示。

此外，我们可以添加弹出窗口来显示消息。