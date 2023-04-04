# BootstrapVue —对可见性做出反应

> 原文：<https://blog.devgenius.io/bootstrapvue-reacting-on-visibility-e7183617fde8?source=collection_archive---------11----------------------->

![](img/1249c1a088f5fd3c6ea92c543040713c.png)

彼得·奥斯拉内克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

为了制作好看的 Vue 应用，我们需要设计组件的样式。

为了使我们的生活更容易，我们可以使用内置样式的组件。

在本文中，我们看看如何使用`v-b-visible`指令来运行可见或不可见的东西。

# 指令语法

我们可以使用`v-b-visible`指令，通过编写:

```
<div v-b-visible.[mod1].[mod2]="callback">content</div>
```

`mod1`和`mod2`是修饰语。

`callback`是可见度状态改变时运行的功能。

它有一个指示可见性的参数。

参数可以是`true`或`false`。

除了使用了`once`修饰符之外，每次元素的可见性改变时都会运行回调。

所有修饰符都是可选的。

# 例子

我们可以通过书写来使用它:

```
<template>
  <div v-b-visible="visibleHandler">...</div>
</template><script>
export default {
  methods: {
    visibleHandler(isVisible) {
      if (isVisible) {
        // Do something
      } else {
        // Do something else
      }
    }
  }
};
</script>
```

我们有`visibleHandler`功能。

`isVisible`表示 div 是否可见。

然后我们可以根据能见度状况做一些事情。

# 将 v-b-visible 与 once 修饰符一起使用

我们可以在`v-b-visible`中使用`once`修饰符。

例如，我们可以写:

```
<template>
  <div v-b-visible.once="visibleHandler"> ... </div>
</template><script>
export default {
  methods: {
    visibleHandler(isVisible) {
      if (isVisible) {
        // ...
      } else {
        // ...
      }
    }
  }
}
</script>
```

那么`visibleHandler`只被调用一次。

所以当元素第一次可见时,`isVisible`可以为真。

`isVisible`可以是`false`零次或多次。

在元素变得可见之后，它将永远不会出现。

# 与一次和偏移修改器一起使用

我们还可以添加一个偏移修改器。

例如，我们可以写:

```
<template>
  <div v-b-visible.once.350="visibleHandler">...</div>
</template>
<script>
export default {
  methods: {
    visibleHandler(isVisible) {
      if (isVisible) {
        //...
      } else {
        //...
      }
    }
  }
};
</script>
```

偏移量是距视口边缘的像素数，用于确定元素何时被视为在视口中。

因此，如果它在视窗的 350 像素范围内，它将被视为在视窗中。

例如，我们写道:

```
<template>
  <div
    v-b-visible.once.350="visibleHandler"
    :class="[isVisible ? 'bg-info' : 'bg-light', 'text-center']"
  >
    <p v-for="n in 100" :key="n">{{n}}</p>
    <b-badge v-b-visible="visibleHandler">Element with v-b-visible directive</b-badge>
    <p v-for="n in 100" :key="n">{{n}}</p>
  </div>
</template>
<script>
export default {
  data() {
    return {
      isVisible: false
    };
  },
  methods: {
    visibleHandler(isVisible) {
      this.isVisible = isVisible;
    }
  }
};
</script>
```

如果我们看到徽章，就会看到绿色背景。

否则，我们会看到浅色背景。

这是因为我们在`b-badge`中添加了`v-b-visible`指令。

当我们滚动时徽章变得可见或不可见时，运行`visibilityHandler`。

div 上的`class`道具将根据`isVisible`状态改变类，这是我们在`visibleHandler`中更新的。

# CSS 显示可见性检测

我们可以用 CSS 检测可见性。

例如，我们可以写:

```
<template>
  <div>
    <b-button @click="show = !show">Toggle display</b-button>
    <p>Visible: {{ isVisible }}</p>
    <div class="border p-3">
      <div v-show="show" class="bg-info p-3">
        <b-badge v-b-visible="handleVisibility">Element with v-b-visible directive</b-badge>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      show: false,
      isVisible: false
    };
  },
  methods: {
    handleVisibility(isVisible) {
      this.isVisible = isVisible;
    }
  }
};
</script>
```

`v-show`用 CSS 显示和隐藏元素和组件。

当它被显示或隐藏时，`handleVisibility`就会被触发。

因此，`isVisible`将与`show`同时被触发。

![](img/e7fbe6b56e9717b3c5595ef435cb52f2.png)

照片由 [Khanh Steven](https://unsplash.com/@khanhkhanhlv?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以使用`v-b-visible`指令来检测元素或组件的可见性变化。

然后，当某些东西的可见性改变时，我们可以运行回调来做一些事情。