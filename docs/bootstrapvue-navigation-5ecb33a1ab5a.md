# BootstrapVue —导航

> 原文：<https://blog.devgenius.io/bootstrapvue-navigation-5ecb33a1ab5a?source=collection_archive---------32----------------------->

![](img/7e2dfa29a4181d57ffc1ab1e65731617.png)

[迪潘卡尔·戈戈伊](https://unsplash.com/@dipankargogoi55?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

为了制作好看的 Vue 应用，我们需要设计组件的样式。

为了使我们的生活更容易，我们可以使用内置样式的组件。

在本文中，我们将看看如何添加导航组件。

# nav

BootstrapVue 附带了一个`b-nav`组件，允许我们在应用程序中添加导航栏。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-nav>
      <b-nav-item active>foo</b-nav-item>
      <b-nav-item>bar</b-nav-item>
      <b-nav-item disabled>baz</b-nav-item>
    </b-nav>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们有`b-nav`组件，里面有`b-nav-item`组件。

`active`道具激活导航项目。

`disabled`使其变灰，不可点击。

# 标签样式

我们可以添加`tabs`道具，让导航条看起来像标签。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-nav tabs>
      <b-nav-item active>foo</b-nav-item>
      <b-nav-item>bar</b-nav-item>
      <b-nav-item disabled>baz</b-nav-item>
    </b-nav>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script>
```

现在`active`标签周围有了线条。

其余都一样。

# 药丸样式

道具使导航项目看起来像按钮。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-nav pills>
      <b-nav-item active>foo</b-nav-item>
      <b-nav-item>bar</b-nav-item>
      <b-nav-item disabled>baz</b-nav-item>
    </b-nav>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script>
```

# 小尺寸

我们可以使用`small`道具使导航项目小于默认项目:

```
<template>
  <div id="app">
    <b-nav small>
      <b-nav-item active>foo</b-nav-item>
      <b-nav-item>bar</b-nav-item>
      <b-nav-item disabled>baz</b-nav-item>
    </b-nav>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script>
```

# 充满

`fill`支柱使导航伸展到其最大宽度:

```
<template>
  <div id="app">
    <b-nav fill>
      <b-nav-item active>foo</b-nav-item>
      <b-nav-item>bar</b-nav-item>
      <b-nav-item disabled>baz</b-nav-item>
    </b-nav>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script>
```

# 调整

`justified`道具让我们使导航项目等宽，并且导航出现在所有水平空间:

```
<template>
  <div id="app">
    <b-nav justified>
      <b-nav-item active>foo</b-nav-item>
      <b-nav-item>bar</b-nav-item>
      <b-nav-item disabled>baz</b-nav-item>
    </b-nav>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script>
```

# 对齐

我们可以添加`align`道具来按照我们喜欢的方式对齐导航项目。

该值可以是`left`、`center`或`right`。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-nav align="right">
      <b-nav-item active>foo</b-nav-item>
      <b-nav-item>bar</b-nav-item>
      <b-nav-item disabled>baz</b-nav-item>
    </b-nav>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script>
```

因为我们将`align`设置为`'right'`，所以导航项目在右边。

# 铅垂变化

导航条可以与`vertical`支柱垂直；

```
<template>
  <div id="app">
    <b-nav vertical>
      <b-nav-item active>foo</b-nav-item>
      <b-nav-item>bar</b-nav-item>
      <b-nav-item disabled>baz</b-nav-item>
    </b-nav>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script>
```

# 下拉菜单

我们可以在导航栏中添加下拉菜单。

为此，我们使用`b-nav-item-dropdown`组件:

```
<template>
  <div id="app">
    <b-nav>
      <b-nav-item active>foo</b-nav-item>
      <b-nav-item>bar</b-nav-item>
      <b-nav-item-dropdown text="fruit" toggle-class="nav-link-custom" right>
        <b-dropdown-item>apple</b-dropdown-item>
        <b-dropdown-divider></b-dropdown-divider>
        <b-dropdown-item>banana</b-dropdown-item>
      </b-nav-item-dropdown>
    </b-nav>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script>
```

我们只需将`b-nav-item-dropdown`组件放入`b-nav`中，就会显示一个下拉列表。

# 文本内容

我们可以用`b-nav-text`组件向导航栏添加文本内容:

```
<template>
  <div id="app">
    <b-nav>
      <b-nav-item active>foo</b-nav-item>
      <b-nav-item>bar</b-nav-item>
      <b-nav-text>text</b-nav-text>
    </b-nav>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script>
```

![](img/e1bdc39d9f51ef11c6ee7bf100b16633.png)

[约翰·西门子](https://unsplash.com/@johannsiemens?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

我们可以用`b-nav`组件给应用程序添加导航条。

`b-nav-item`是显示在导航栏内的一个项目。

我们也可以在导航栏中添加下拉菜单和表单。