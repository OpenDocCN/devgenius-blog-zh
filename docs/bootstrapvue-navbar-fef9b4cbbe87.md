# BootstrapVue —导航条

> 原文：<https://blog.devgenius.io/bootstrapvue-navbar-fef9b4cbbe87?source=collection_archive---------1----------------------->

![](img/c62bd42a0ef5267960eab8e53551da18.png)

照片由[Antonio magri](https://unsplash.com/@antonio_m89?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

为了制作好看的 Vue 应用，我们需要设计组件的样式。

为了使我们的生活更容易，我们可以使用内置样式的组件。

在本文中，我们将看看如何在我们的应用程序中添加导航条。

# 导航条

组件让我们添加一个带有商标的导航条。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-navbar toggleable="lg" type="dark" variant="info">
      <b-navbar-brand href="#">brand</b-navbar-brand> <b-navbar-toggle target="nav-collapse"></b-navbar-toggle> <b-collapse id="nav-collapse" is-nav>
        <b-navbar-nav>
          <b-nav-item href="#">foo</b-nav-item>
          <b-nav-item href="#" disabled>bar</b-nav-item>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script>
```

我们有一个添加了`b-navbar`组件的导航条。

`b-collapse`让我们添加响应的链接。

因此，如果我们有一个小屏幕，它们将被压缩成一个汉堡菜单。

`b-nav-item`是导航项。

`toggleable`道具是用来设置断点的，这样导航条就会从菜单扩展到完整的导航条。

`variant`是造型变体。

`type`是文本颜色样式。

# 下拉菜单

我们可以在导航条上添加下拉菜单。

我们使用`b-nav-item-dropdown`和`b-dropdown-item`来完成这项工作:

```
<template>
  <div id="app">
    <b-navbar toggleable="lg" type="dark" variant="info">
      <b-navbar-nav class="ml-auto">
        <b-nav-item-dropdown text="Menu" right>
          <b-dropdown-item href="#">foo</b-dropdown-item>
          <b-dropdown-item href="#">bar</b-dropdown-item>
        </b-nav-item-dropdown>
      </b-navbar-nav>
    </b-navbar>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script>
```

我们有一个下拉菜单。

它反应灵敏，适合任何屏幕。

`right`道具将下拉列表向右对齐。

# 自定义下拉按钮

我们可以自定义带槽的下拉按钮。

例如，我们可以填充`button-content`槽来添加我们自己的内容:

```
<template>
  <div id="app">
    <b-navbar toggleable="lg" type="dark" variant="info">
      <b-navbar-nav class="ml-auto">
        <b-nav-item-dropdown right>          
          <template v-slot:button-content>
            <strong>foo</strong>
          </template>
          <b-dropdown-item href="#">bar</b-dropdown-item>
          <b-dropdown-item href="#">baz</b-dropdown-item>
        </b-nav-item-dropdown>
      </b-navbar-nav>
    </b-navbar>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script>
```

现在下拉按钮是粗体的。

# 内嵌表单

我们可以添加内嵌形式。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-navbar toggleable="lg" type="dark" variant="info">
      <b-navbar-nav class="ml-auto">
        <b-nav-form>
          <b-form-input size="sm" class="mr-sm-2" placeholder="Input"></b-form-input>
          <b-button size="sm" class="my-2 my-sm-0" type="submit">Submit</b-button>
        </b-nav-form>
      </b-navbar-nav>
    </b-navbar>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script>
```

我们有容纳`b-nav-form`的`b-navbar-nav`。

在它里面，我们有表单里面的`b-form-input`。

# b-导航条-品牌

品牌被添加到导航栏的左上方。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-navbar variant="faded" type="light">
      <b-navbar-brand href="#">Brand</b-navbar-brand>
    </b-navbar>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script>
```

我们用`variant`改变了样式变量，用`type`改变了文本颜色。

我们还可以添加图像:

```
<template>
  <div id="app">
    <b-navbar>
      <b-navbar-brand href="#">
        <img src="https://placekitten.com/30/30" alt="cat">
      </b-navbar-brand>
    </b-navbar>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script>
```

我们在`b-navbar-brand`中添加了一个 img 元素。

我们必须确保图像符合我们自己的风格。

# 自定义导航栏开关

为了定制导航条开关，我们可以使用`b-navbar-toggle`组件。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-navbar toggleable >
      <b-navbar-brand href="#">brand</b-navbar-brand> <b-navbar-toggle target="navbar-toggle-collapse">
        <template v-slot:default="{ expanded }">
          <b-icon v-if="expanded" icon="chevron-bar-up"></b-icon>
          <b-icon v-else icon="chevron-bar-down"></b-icon>
        </template>
      </b-navbar-toggle> <b-collapse id="navbar-toggle-collapse" is-nav>
        <b-navbar-nav class="ml-auto">
          <b-nav-item href="#">foo</b-nav-item>
          <b-nav-item href="#">bar</b-nav-item>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script>
```

我们有`b-navbar-toggle`组件来定制切换。

默认插槽已填充，以便我们可以对其进行自定义。

![](img/a86998c5c27c0b16d60ecf619e86269e.png)

玛丽塔·卡维拉什维利在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以创建一个带有各种定制的导航条。

可以添加菜单、表单和链接。