# NativeScript Vue — Flexbox 和网格

> 原文：<https://blog.devgenius.io/nativescript-vue-flexbox-and-grid-8fad14f33b4f?source=collection_archive---------6----------------------->

![](img/7d144fbbe960a4ce9e3168d03b1ed962.png)

[卡梅隆·威特尼](https://unsplash.com/@camwitney?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Vue 是一个易于使用的构建前端应用的框架。

NativeScript 是一个移动应用程序框架，它允许我们使用流行的前端框架构建原生移动应用程序。

在本文中，我们将了解如何使用 NativeScript Vue 构建一个应用程序。

# FlexboxLayout

我们可以使用`FlexboxLayout`来添加基于 flexbox 的布局。

例如，我们可以写:

```
<template>
  <Page>
    <ActionBar title="Welcome to NativeScript-Vue!" />
    <FlexboxLayout backgroundColor="#3c495e">
      <Label text="first" width="90" backgroundColor="red" />
      <Label text="second" width="90" backgroundColor="green" />
      <Label text="third" width="90" backgroundColor="blue" />
    </FlexboxLayout>
  </Page>
</template><script >
export default {};
</script>
```

我们添加了`backgroundColor`道具来设置`Label`的背景颜色

`text`有正文。`width`设定宽度。

所以我们并排得到了`Label` s。

我们还可以将`flexDirection`属性设置为`'column'`来创建一个列布局:

```
<template>
  <Page>
    <ActionBar title="Welcome to NativeScript-Vue!" />
    <FlexboxLayout flexDirection="column" backgroundColor="#3c495e">
      <Label text="first" height="90" backgroundColor="red" />
      <Label text="second" height="90" backgroundColor="green" />
      <Label text="third" height="90" backgroundColor="blue" />
    </FlexboxLayout>
  </Page>
</template><script >
export default {};
</script>
```

`Label`现在将显示在一列中。

我们可以设置`alignItems`道具来按照我们的方式排列物品。

例如，我们可以写:

```
<template>
  <Page>
    <ActionBar title="Welcome to NativeScript-Vue!" />
    <FlexboxLayout alignItems="flex-start" backgroundColor="#3c495e">
      <Label text="first" width="90" height="90" backgroundColor="red" />
      <Label text="second" width="90" height="90" backgroundColor="green" />
      <Label text="third" width="90" height="90" backgroundColor="blue" />
    </FlexboxLayout>
  </Page>
</template><script >
export default {};
</script>
```

我们设置`alignItems`道具来设置`flex-start`将项目向左对齐。

组件的顺序可以改变。

例如，我们可以写:

```
<template>
  <Page>
    <ActionBar title="Welcome to NativeScript-Vue!" />
    <FlexboxLayout alignItems="flex-start" backgroundColor="#3c495e">
      <Label
        text="first"
        order="2"
        width="90"
        height="90"
        backgroundColor="red"
      />
      <Label
        text="second"
        order="3"
        width="90"
        height="90"
        backgroundColor="green"
      />
      <Label
        text="third"
        order="1"
        width="90"
        height="90"
        backgroundColor="blue"
      />
    </FlexboxLayout>
  </Page>
</template><script >
export default {};
</script>
```

我们设置`order`属性来切换 flex 容器中`Label`的顺序。

如果超出屏幕宽度，`FlexboxLayout`内的组件将自动换行。

例如，如果我们有:

```
<template>
  <Page>
    <ActionBar title="Welcome to NativeScript-Vue!" />
    <FlexboxLayout flexWrap="wrap" backgroundColor="#3c495e">
      <Label text="first" width="30%" backgroundColor="red" />
      <Label text="second" width="30%" backgroundColor="green" />
      <Label text="third" width="30%" backgroundColor="blue" />
      <Label text="fourth" width="30%" backgroundColor="yellow" />
    </FlexboxLayout>
  </Page>
</template><script >
export default {};
</script>
```

然后第四个`Label`将被推到第二排。

这是因为我们将`flexWrap`道具设置为`'wrap'`。

# 网格布局

`GridLayout`组件让我们以类似表格的方式排列子元素。

例如，我们可以写:

```
<template>
  <Page>
    <ActionBar title="Welcome to NativeScript-Vue!" />
    <GridLayout columns="115, 115" rows="115, 115">
      <Label text="0,0" row="0" col="0" backgroundColor="red" />
      <Label text="0,1" row="0" col="1" backgroundColor="green" />
      <Label text="1,0" row="1" col="0" backgroundColor="blue" />
      <Label text="1,1" row="1" col="1" backgroundColor="yellow" />
    </GridLayout>
  </Page>
</template><script >
export default {};
</script>
```

我们添加带有`row`和`col`道具的`Label`组件来设置行和列。

`columns`有列宽。

和`rows`具有每排的高度。

# 结论

我们可以用 NativeScript Vue 添加 flexbox 布局和网格布局。