# 使用 PrimeVue 框架开发 vue 3—偏移和文本样式

> 原文：<https://blog.devgenius.io/vue-3-development-with-the-primevue-framework-offset-and-text-styles-f9334c540d58?source=collection_archive---------1----------------------->

![](img/3cb06ec6ef7769937a6d3ba1bfbe7eea.png)

[Fas Khan](https://unsplash.com/@fasbytes?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

PrimeVue 是一个与 Vue 3 兼容的 UI 框架。

在本文中，我们将了解如何开始使用 PrimeVue 开发 Vue 3 应用程序。

# 抵消

我们可以给栏目增加页边空白。

为此，我们可以写:

```
<template>
  <div class="p-grid">
    <div class="p-col-4">4</div>
    <div class="p-col-4 p-offset-4">4</div>
  </div>
</template><script>
export default {
  name: "App",
};
</script>
```

`p-offset-4`类让我们将右列向右移动 4 列。

# 嵌套网格

网格可以用`nested-grid`类嵌套:

```
<template>
  <div class="p-grid nested-grid">
    <div class="p-col-8">
      <div class="p-grid">
        <div class="p-col-6">6</div>
        <div class="p-col-6">6</div>
        <div class="p-col-8">8</div>
      </div>
    </div>
    <div class="p-col-4">4</div>
  </div>
</template><script>
export default {
  name: "App",
};
</script>
```

# 排水沟

我们可以用`p-nogutter`类删除网格列的边距:

```
<template>
  <div class="p-grid p-nogutter">
    <div class="p-col">1</div>
    <div class="p-col p-nogutter">2</div>
    <div class="p-col">3</div>
  </div>
</template><script>
export default {
  name: "App",
};
</script>
```

# 边距和填充

PrimeVue 的 PrimeFlex 库附带了用于添加边距和填充的类。

该类的一般格式是`p-{property}{position}-{value}`

`property`可以是:

*   `m` —边距
*   `p` —填充

`position`可以是以下之一:

*   **t** :顶
*   **b** :底部
*   **l** :左
*   **r** :右
*   **x** :左右
*   **y** :顶部和底部
*   **空白**:所有边

`value`可以是以下之一:

*   **0** : $spacer * 0
*   **1** : $spacer * .25
*   **2** : $spacer * .5
*   **3** : $spacer * 1
*   **4** : $spacer * 1.5
*   **5** : $spacer * 2
*   **6** : $spacer * 3
*   **自动**:自动边距

例如，我们可以写:

```
<template>
  <div>
    <div class="p-mb-2">Margin bottom 2</div>
  </div>
</template><script>
export default {
  name: "App",
};
</script>
```

向网格添加下边距。

我们还可以通过编写以下代码根据断点添加填充和边距:

```
<template>
  <div>
    <div class="p-m-1 p-p-1 p-m-lg-3 p-b-lg-3">padding and margin</div>
  </div>
</template><script>
export default {
  name: "App",
};
</script>
```

`lg`是断点。当屏幕宽到足以触及`lg`断点时，应用带有它们的类。

# 文本对齐

PrimeFlex 还附带了对齐文本的类。

类的一般格式是`p-text-{value}`，其中`value`可以是以下之一:

*   左边的
*   中心
*   正确
*   证明合法

例如，我们可以写:

```
<template>
  <div>
    <div class="p-text-left">Left</div>
    <div class="p-text-center">Center</div>
    <div class="p-text-right">Right</div>
    <div class="p-text-justify">Justify</div>
  </div>
</template><script>
export default {
  name: "App",
};
</script>
```

# 文本滚卷

我们可以添加一些类，使用以下类之一来更改文本的换行方式:

*   `p-text-nowrap`
*   `p-text-wrap`
*   `p-text-truncate`

# 改变

我们可以用下面的类来改变文本的大小写:

*   `p-text-lowercase`
*   `p-text-uppercase`
*   `p-text-capitalize`

# 文本样式

我们可以用下面的类来改变文本样式:

*   `p-text-bold`
*   `p-text-normal`
*   `p-text-light`
*   `p-text-italic`

# 结论

我们可以使用 PrimeFlex 库来设计文本样式和更改列选项。