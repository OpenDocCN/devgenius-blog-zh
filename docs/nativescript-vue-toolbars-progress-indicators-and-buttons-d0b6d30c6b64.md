# NativeScript Vue —工具栏、进度指示器和按钮

> 原文：<https://blog.devgenius.io/nativescript-vue-toolbars-progress-indicators-and-buttons-d0b6d30c6b64?source=collection_archive---------8----------------------->

![](img/b26e15a266cc3c80bfe8172775d89408.png)

照片由[卡拉·伍兹](https://unsplash.com/@kharaoke?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Vue 是一个易于使用的构建前端应用的框架。

NativeScript 是一个移动应用程序框架，它允许我们使用流行的前端框架构建原生移动应用程序。

在本文中，我们将了解如何使用 NativeScript Vue 构建一个应用程序。

# ActionItem

我们可以使用`ActionItem`组件向`ActionBar`组件添加动作按钮。

例如，我们可以写:

```
<template>
  <Page>
    <ActionBar title="NativeScript App">
      <ActionItem
        @tap="onTapShare"
        android.systemIcon="ic_menu_share"
        android.position="actionBar"
      />
      <ActionItem @tap="onTapDelete" text="delete" android.position="popup" />
    </ActionBar>
    <WrapLayout></WrapLayout>
  </Page>
</template><script >
export default {
  methods: {
    onTapShare() {
      console.log("shared tapped");
    },
    onTapDelete() {
      console.log("delete tapped");
    },
  },
};
</script>
```

第一个`ActionItem`是显示在工具栏上的图标。

我们将图标设置为与`android.systemIcon`道具一起显示。

`android.position`道具设置图标显示的位置。

第二个`ActionItem`具有用于菜单项文本的`text`道具。

我们将`android.position`设置为`'popup'`，以便在右上角的弹出菜单中显示“删除”项。

当我们点击按钮时,`tap`事件监听器运行。

# 导航按钮

`NavigationButton`组件允许我们在应用程序中添加导航按钮。

例如，我们可以写:

```
<template>
  <Page>
    <ActionBar title="NativeScript App">
      <NavigationButton
        text="Go back"
        android.systemIcon="ic_menu_back"
        @tap="goBack"
      />
    </ActionBar>
    <WrapLayout></WrapLayout>
  </Page>
</template><script >
export default {
  methods: {
    goBack() {
      console.log("go back tapped");
    },
  },
};
</script>
```

我们将`NavigationButton`添加到我们的`ActionBar`中。

我们为导航按钮设置了`text`，它显示在图标旁边。

`android.systemIcon`设置图标。

我们在`NavigationItem`上有`tap`事件监听器，所以当我们点击导航按钮时，就会运行`goBack`方法。

# 活动指示器

`ActivityIndicator`组件让我们为用户显示一个进度指示器。

例如，我们可以写:

```
<template>
  <Page>
    <ActionBar title="NativeScript App"> </ActionBar>
    <WrapLayout>
      <ActivityIndicator :busy="busy" @busyChange="onBusyChanged" />
    </WrapLayout>
  </Page>
</template><script >
export default {
  data() {
    return { busy: false };
  },
  mounted() {
    this.busy = true;
    setTimeout(() => {
      this.busy = false;
    }, 3000);
  },
  methods: {
    onBusyChanged() {
      console.log("busy changed");
    },
  },
};
</script>
```

我们将`ActivityIndicator`添加到我们的应用程序中。

`busy`控制显示活动指示器的时间。

如果是`true`就会显示出来。

同样，我们用`onBusyChanged`方法监听`busyChange`事件。

每当`busy`道具改变时，它就运行。

# 纽扣

我们可以使用`Button`组件在屏幕上添加一个按钮。

为此，我们写道:

```
<template>
  <Page>
    <ActionBar title="NativeScript App"></ActionBar>
    <FlexboxLayout
      justifyContent="space-around"
      flexDirection="column"
      backgroundColor="#3c495e"
    >
      <Button text="Button" @tap="onButtonTap" />
    </FlexboxLayout>
  </Page>
</template><script >
export default {
  methods: {
    onButtonTap() {
      console.log("button tapped");
    },
  },
};
</script>
```

我们添加带有`text`属性的`Button`组件来设置按钮文本。

我们用`onButtonTap`方法收听`tap`事件。

我们可以用`Span`组件来设计我们的按钮。

例如，我们可以写:

```
<template>
  <Page>
    <ActionBar title="NativeScript App"></ActionBar>
    <FlexboxLayout
      justifyContent="space-around"
      flexDirection="column"
      backgroundColor="#3c495e"
    >
      <Button @tap="onButtonTap">
        <Span text="Red and Bold" fontWeight="bold" style="color: red" />
      </Button>
    </FlexboxLayout>
  </Page>
</template><script >
export default {
  methods: {
    onButtonTap() {
      console.log("button tapped");
    },
  },
};
</script>
```

我们在`Button`里面加了`Span`。

`text`属性设置按钮文本。

`fontWeight`设置字体粗细。

`style`道具设置文本样式。

# 结论

我们可以用 NativeScript Vue 添加顶栏项目、进度指示器和按钮。