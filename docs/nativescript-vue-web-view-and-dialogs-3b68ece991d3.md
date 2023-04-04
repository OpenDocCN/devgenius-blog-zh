# NativeScript Vue — Web 视图和对话框

> 原文：<https://blog.devgenius.io/nativescript-vue-web-view-and-dialogs-3b68ece991d3?source=collection_archive---------5----------------------->

![](img/2e186ef9c2db472082bfee2216584066.png)

梅根·克拉克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Vue 是一个易于使用的构建前端应用的框架。

NativeScript 是一个移动应用程序框架，它允许我们使用流行的前端框架构建原生移动应用程序。

在本文中，我们将了解如何使用 NativeScript Vue 构建一个应用程序。

# 网络视图

我们可以添加一个`WebView`组件来显示应用程序中的 web 内容。

例如，我们可以写:

```
<template>
  <Page>
    <ActionBar title="NativeScript App"></ActionBar>
    <FlexboxLayout flexDirection="column">
      <WebView src="https://www.yahoo.com/" />
    </FlexboxLayout>
  </Page>
</template><script>
export default {};
</script>
```

我们将网页的 URL 设置为显示为`src`属性的值。

此外，我们可以通过编写以下代码来显示本地 HTML 页面:

```
<template>
  <Page>
    <ActionBar title="NativeScript App"></ActionBar>
    <FlexboxLayout flexDirection="column">
      <WebView src="~/assets/hello.html" />
    </FlexboxLayout>
  </Page>
</template><script>
export default {};
</script>
```

我们还可以用它显示一个呈现的 HTML 字符串:

```
<template>
  <Page>
    <ActionBar title="NativeScript App"></ActionBar>
    <FlexboxLayout flexDirection="column">
      <WebView src="<div><h1>hello world</h1></div>" />
    </FlexboxLayout>
  </Page>
</template><script>
export default {};
</script>
```

我们将`src`道具设置为一个 HTML 字符串，它就会显示出来。

# 动作对话框

我们可以用`action`方法显示一个动作对话框。

例如，我们可以写:

```
<template>
  <Page>
    <ActionBar title="NativeScript App"></ActionBar>
    <FlexboxLayout flexDirection="column">
      <Button text="Open Dialog" @tap="openDialog" />
    </FlexboxLayout>
  </Page>
</template><script>
export default {
  methods: {
    async openDialog() {
      const result = await action("Your message", "Cancel", [
        "Option1",
        "Option2",
      ]);
      console.log(result);
    },
  },
};
</script>
```

我们添加一个按钮来调用`openAlert`方法，当我们点击它时打开动作对话框。

`action`函数是一个全局函数，被调用来打开动作对话框。

第一个参数是消息，显示为标题。

第二个参数是取消按钮文本。

第三个参数是我们可以点击的选项文本数组。

# 警报对话框

我们可以用`alert`全局函数显示一个警告对话框。

例如，我们可以写:

```
<template>
  <Page>
    <ActionBar title="NativeScript App"></ActionBar>
    <FlexboxLayout flexDirection="column">
      <Button text="Open Dialog" @tap="openDialog" />
    </FlexboxLayout>
  </Page>
</template><script>
export default {
  methods: {
    async openDialog() {
      await alert({
        title: "Your title",
        message: "Your message",
        okButtonText: "OK",
      });
      console.log("Alert dialog closed");
    },
  },
};
</script>
```

我们添加了一个按钮，以便在我们点击按钮时显示警告。

`alert`函数接受一个让我们设置对话框标题的对象。

`message`是对话框的内容文本。

`okButtonText`是确定按钮的文本。

此外，我们可以只传入一个字符串来显示:

```
<template>
  <Page>
    <ActionBar title="NativeScript App"></ActionBar>
    <FlexboxLayout flexDirection="column">
      <Button text="Open Dialog" @tap="openDialog" />
    </FlexboxLayout>
  </Page>
</template><script>
export default {
  methods: {
    async openDialog() {
      await alert("hello world");
      console.log("Alert dialog closed");
    },
  },
};
</script>
```

# 确认对话

`confirm`全局函数让我们打开一个确认对话框。

例如，我们可以写:

```
<template>
  <Page>
    <ActionBar title="NativeScript App"></ActionBar>
    <FlexboxLayout flexDirection="column">
      <Button text="Open Dialog" @tap="openDialog" />
    </FlexboxLayout>
  </Page>
</template><script>
export default {
  methods: {
    async openDialog() {
      const result = await confirm({
        title: "Your title",
        message: "Your message",
        okButtonText: "OK",
        cancelButtonText: "Cancel",
      });
      console.log(result);
    },
  },
};
</script>
```

我们通过调用`confirm`函数来添加打开确认对话框的按钮。

`title`属性有标题文本。

`message`有对话框提示信息。

`okButtonText`有确定按钮的文本。

并且`cancelButtonText`具有取消按钮文本。

我们还可以通过编写以下内容来添加一个简单的对话框:

```
<template>
  <Page>
    <ActionBar title="NativeScript App"></ActionBar>
    <FlexboxLayout flexDirection="column">
      <Button text="Open Dialog" @tap="openDialog" />
    </FlexboxLayout>
  </Page>
</template><script>
export default {
  methods: {
    async openDialog() {
      const result = await confirm("Your message");
      console.log(result);
    },
  },
};
</script>
```

该字符串是确认对话框的消息文本。

# 结论

我们可以用 NativeScript Vue 添加 web 视图和各种对话框。