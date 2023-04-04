# 使用 PrimeVue 框架开发 vue 3——表单和网格布局

> 原文：<https://blog.devgenius.io/vue-3-development-with-the-primevue-framework-form-layout-789b541e6f3?source=collection_archive---------4----------------------->

![](img/93185d0f61ce0c2fb65e6a4a2fbcc3d9.png)

照片由 [Venu Gopinath Nukavarapu](https://unsplash.com/@venu_gopinath?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

PrimeVue 是一个与 Vue 3 兼容的 UI 框架。

在本文中，我们将了解如何开始使用 PrimeVue 开发 Vue 3 应用程序。

# 表单布局

PrimeFlex 库是 PrimeVue 框架的一部分，它附带了用于创建表单布局的类。

例如，我们可以通过编写以下代码来使用`p-field`类:

```
<template>
  <div class="p-fluid">
    <div class="p-field">
      <label for="firstname1">First name</label>
      <InputText id="firstname1" type="text" />
    </div>
    <div class="p-field">
      <label for="lastname1">Last name</label>
      <InputText id="lastname1" type="text" />
    </div>
  </div>
</template><script>
export default {
  name: "App",
};
</script>
```

我们用它来设计场地。

此外，我们可以写:

```
<template>
  <div class="p-fluid p-formgrid p-grid">
    <div class="p-field p-col">
      <label for="firstname">First name</label>
      <InputText id="firstname" type="text" />
    </div>
    <div class="p-field p-col">
      <label for="lastname">Last name</label>
      <InputText id="lastname" type="text" />
    </div>
  </div>
</template><script>
export default {
  name: "App",
};
</script>
```

用列分隔屏幕，并为每个列添加一个字段。

`p-col`让我们把屏幕分成列。

我们可以通过编写以下内容来添加内联项目:

```
<template>
  <div class="p-formgroup-inline">
    <div class="p-field">
      <label for="firstname" class="p-sr-only">First name</label>
      <InputText id="firstname" type="text" placeholder="Firstname" />
    </div>
    <div class="p-field">
      <label for="lastname" class="p-sr-only">Last name</label>
      <InputText id="lastname" type="text" placeholder="Lastname" />
    </div>
    <Button type="button" label="Submit" />
  </div>
</template><script>
export default {
  name: "App",
};
</script>
```

`p-formgroup-inline`让我们添加内嵌表单字段。

`p-field-checkbox`类让我们垂直布局复选框:

```
<template>
  <div class="p-field-checkbox">
    <Checkbox id="city1" name="city1" value="San Francisco" />
    <label for="city1">San Francisco</label>
  </div>
  <div class="p-field-checkbox">
    <Checkbox id="city2" name="city1" value="Los Angeles" />
    <label for="city2">Los Angeles</label>
  </div>
</template><script>
export default {
  name: "App",
};
</script>
```

# 网格系统

PrimeVue 自带网格系统。

要添加网格，我们可以写:

```
<template>
  <div class="p-grid">
    <div class="p-col">1</div>
    <div class="p-col">2</div>
    <div class="p-col">3</div>
  </div>
</template><script>
export default {
  name: "App",
};
</script>
```

`p-grid`类创建一个网格布局。

`p-col`使 div 成为网格的列。

如果它们溢出了屏幕的宽度，那么溢出的将被移到下一行。

我们可以设置每列的宽度:

```
<template>
  <div class="p-grid">
    <div class="p-col-6">6</div>
    <div class="p-col-6">6</div>
    <div class="p-col-6">6</div>
    <div class="p-col-6">6</div>
  </div>
</template><script>
export default {
  name: "App",
};
</script>
```

此外，我们可以根据断点设置列宽:

```
<template>
  <div class="p-grid">
    <div class="p-col-12 p-md-6 p-lg-3">A</div>
    <div class="p-col-12 p-md-6 p-lg-3">B</div>
    <div class="p-col-12 p-md-6 p-lg-3">C</div>
    <div class="p-col-12 p-md-6 p-lg-3">D</div>
  </div>
</template><script>
export default {
  name: "App",
};
</script>
```

可用断点包括:

*   `sm` —最小宽度 576 像素
*   `md` —最小宽度 768 像素
*   `lg` —最小宽度 992 像素
*   `xl` —最小宽度 1200 像素

# 结论

我们可以用 PrimeVue 框架添加各种布局来构建我们的 Vue 3 应用程序。