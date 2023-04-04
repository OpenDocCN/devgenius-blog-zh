# NativeScript Vue —页面、进度条、滚动视图和搜索栏

> 原文：<https://blog.devgenius.io/nativescript-vue-page-progress-bar-scroll-view-and-search-bar-d4642e259b7b?source=collection_archive---------5----------------------->

![](img/90b28cda713d3a5ba48f3580933c9c50.png)

帕特里克·托马索在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Vue 是一个易于使用的构建前端应用的框架。

NativeScript 是一个移动应用程序框架，它允许我们使用流行的前端框架构建原生移动应用程序。

在本文中，我们将了解如何使用 NativeScript Vue 构建一个应用程序。

# 页

组件让我们渲染应用程序的屏幕。

它是一个或多个组件的包装器。

例如，我们可以写:

```
<template>
  <Page>
    <ActionBar title="NativeScript App"></ActionBar>
    <FlexboxLayout flexDirection="column">
      <Label text="Foo" />
      <Label text="Bar" />
    </FlexboxLayout>
  </Page>
</template><script >
export default {};
</script>
```

向页面添加标签。

# 占位符

`Placeholder`组件让我们可以将本地小部件添加到应用程序中。

例如，我们可以写:

```
<template>
  <Page>
    <ActionBar title="NativeScript App"></ActionBar>
    <FlexboxLayout flexDirection="column">
      <Placeholder @creatingView="creatingView" />
    </FlexboxLayout>
  </Page>
</template><script >
export default {
  methods: {
    creatingView(args) {
      const nativeView = new android.widget.TextView(args.context);
      nativeView.setSingleLine(true);
       nativeView.setEllipsize(android.text.TextUtils.TruncateAt.END);
      nativeView.setText("Hello World");
      args.view = nativeView;
    },
  },
};
</script>
```

我们监听`creatingView`事件，并在它发出时运行`creatingView`方法。

然后我们用`android.widget.TextView`构造函数创建文本视图。

我们传入`args.context`属性来返回本地视图。

然后我们调用`setEllipseize`为文本视图设置省略号。

我们调用`setText`来设置文本视图的文本。

我们将它设置为`args.view`属性的值来设置视图。

# 进步

组件让我们显示一个条形图来显示任务的进度。

例如，我们可以写:

```
<template>
  <Page>
    <ActionBar title="NativeScript App"></ActionBar>
    <FlexboxLayout flexDirection="column">
      <Progress :value="currentProgress" />
    </FlexboxLayout>
  </Page>
</template><script >
export default {
  data() {
    return {
      currentProgress: 50,
    };
  },
};
</script>
```

我们添加了`Progress`组件来显示进度条。

`value`道具有进度值。它可以在 0 到 100 之间。

# 滚动视图

`ScrollView`组件允许我们在应用程序中添加一个可滚动的内容区域。

例如，我们可以写:

```
<template>
  <Page>
    <ActionBar title="NativeScript App"></ActionBar>
    <FlexboxLayout flexDirection="column">
      <ScrollView orientation="horizontal">
        <StackLayout orientation="horizontal">
          <Label :text="n" v-for="n in 100" :key="n" width='30' />
        </StackLayout>
      </ScrollView>
    </FlexboxLayout>
  </Page>
</template><script >
export default {};
</script>
```

添加一个内部带有`StackLayout`的`ScrollView`。

我们将两个`orientation`道具都设置为`'horizontal'`来显示水平滚动视图。

然后我们在`StackLayout`里面加上`Label`来显示数字。

现在我们可以滚动浏览这些数字。

# 搜索栏

`SearchBar`组件让我们添加一个输入框，让用户输入搜索查询。

例如，我们可以写:

```
<template>
  <Page>
    <ActionBar title="NativeScript App"></ActionBar>
    <FlexboxLayout flexDirection="column">
      <SearchBar v-model="searchQuery" @submit="onSubmit" />
    </FlexboxLayout>
  </Page>
</template><script >
export default {
  data() {
    return {
      searchQuery: "",
    };
  },
  methods: {
    onSubmit() {
      alert(this.searchQuery);
    },
  },
};
</script>
```

我们将`SearchBar`的输入值绑定到`searchQuery`的无功属性。

然后，当我们按 Enter 键时，就会发出`submit`事件。

然后调用`onSubmit`方法。

我们可以用`hint`道具添加一个搜索提示。

# 结论

我们可以使用 NativeScript Vue 在我们的移动应用程序中添加页面、进度条、滚动视图和搜索栏。