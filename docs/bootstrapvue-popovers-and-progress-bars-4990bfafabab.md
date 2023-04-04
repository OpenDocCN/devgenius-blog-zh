# BootstrapVue —弹出框和进度条

> 原文：<https://blog.devgenius.io/bootstrapvue-popovers-and-progress-bars-4990bfafabab?source=collection_archive---------38----------------------->

![](img/93f8b749a0cdfc72d26fd19dabb21dbb.png)

玛丽·丽贝卡·艾略特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

为了制作好看的 Vue 应用，我们需要设计组件的样式。

为了使我们的生活更容易，我们可以使用内置样式的组件。

我们看看如何自定义弹出窗口和添加进度条。

# 隐藏和显示爆米花

我们可以用`this.$root.emit`方法打开和隐藏我们想要显示的弹出窗口。

该方法让我们可以全局地发出事件，因此它可以用来打开多个弹出窗口。

例如，我们可以写:

```
<template>
  <div id="app" class="text-center">
    <b-button v-b-popover.hover.top="'Popover 1'" title="Title 1">Button 1</b-button>
    <b-button v-b-popover.hover.top="'Popover 2'" title="Title 2">Button 2</b-button>
    <b-button @click='showAll'>Show All</b-button>
  </div>
</template><script>
export default {
  methods: {
    showAll() {
      this.$root.$emit("bv::show::popover");
    }
  }
};
</script><style>
#app {
  margin: 200px;
}
</style>
```

我们有两个带松饼的按钮。

我们有“全部显示”按钮。

我们用`showAll`方法发出`'bv::show::popover'`事件来显示所有的 popovers。

此外，我们可以写:

```
<template>
  <div id="app" class="text-center">
    <b-button id="popover-1" v-b-popover.hover.top="'Popover 1'" title="Title 1">Button 1</b-button>
    <b-button v-b-popover.hover.top="'Popover 2'" title="Title 2">Button 2</b-button>
    <b-button @click="showOne">Show 1</b-button>
  </div>
</template><script>
export default {
  methods: {
    showOne() {
      this.$root.$emit("bv::show::popover", "popover-1");
    }
  }
};
</script><style>
#app {
  margin: 200px;
}
</style>
```

我们有第二个参数为`'popover-1'`的`showOne`方法。

这是我们要打开的 popover 的 ID。

现在，当我们单击 Show 1 按钮时，我们会看到第一个弹出窗口打开。

同样，还有隐藏 popover 的`bv::hide::popover`。

有一个`bv::disable::popover`可以禁用它们，使它们无法打开。

并且有`bv::enable::popover`来启用它们，所以它们可以被打开。

我们可以用`this.$root.$on`来听这些事件。

例如，我们可以写:

```
export default {
  mounted() {
    this.$root.$on('bv::popover::show', bvEventObj => {
      console.log('bvEventObj:', bvEventObj)
    })
  }
}
```

当这些事件被触发时，将运行回调。

# 进步

BootstrapVue 为我们提供了 progress 组件，让我们可以显示某件事情的进度。

要添加一个，我们可以使用`b-progress`和`b-progress-bar`组件。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-progress class="mt-2" :max="max" show-value>
      <b-progress-bar :value="value * (6 / 10)" variant="success"></b-progress-bar>
      <b-progress-bar :value="value * (2.5 / 10)" variant="warning"></b-progress-bar>
      <b-progress-bar :value="value * (1.5 / 10)" variant="danger"></b-progress-bar>
    </b-progress><b-button class="mt-3" @click="randomValue">random number</b-button>
  </div>
</template><script>
export default {
  data() {
    return {
      max: 100,
      value: 0
    };
  },
  methods: {
    randomValue() {
      this.value = Math.random() * this.max;
    }
  }
};
</script>
```

我们有容纳`b-progress-bar`组件的`b-progress`组件。

我们可以有多个值的条形。

在上面的例子中，我们有 6 / 10 是绿色的，因为`variant`被设置为`'success'`。

然后四分之一的条是黄色的，因为我们在第二个`b-progress-bar`中从`variant`到`warning`。

那么我们有 15%是红色的，因为在第三个条中`variant`被设置为`danger`。

`value`决定填充的栏的部分。

`max`是最大值。

`show-value`表示我们是否要显示该值。

# 标签

我们可以使用`show-progress`来显示最大值的百分比。

或者我们可以使用`show-value`道具来显示当前的绝对值。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-progress show-value :value="value" :max="max"></b-progress>
  </div>
</template><script>
export default {
  data() {
    return {
      max: 100,
      value: 75
    };
  }
};
</script>
```

然后我们在进度条上看到绝对值。

# 自定义进度标签

我们可以按照自己喜欢的方式定制进度标签。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-progress :value="value" :max="max" height="2rem">
      <b-progress-bar :value="value">
        <strong>{{ value.toFixed(2) }} / {{ max }}</strong>
      </b-progress-bar>
    </b-progress>
  </div>
</template><script>
export default {
  data() {
    return {
      max: 100,
      value: 75
    };
  }
};
</script>
```

我们有`b-progress-bar`，它将`value`四舍五入到小数点后两位。

我们还获取`max`值并在它之后显示。

![](img/75d7ffda3d7c88c6f43641d771203b36.png)

照片由 [Kosuke Noma](https://unsplash.com/@kkk7799?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

通过发出事件，可以显示、隐藏、启用或禁用弹出窗口。

此外，我们可以用`b-progress-bar`组件创建进度条。