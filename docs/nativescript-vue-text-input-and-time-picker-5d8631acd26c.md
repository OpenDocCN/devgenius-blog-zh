# NativeScript Vue —文本输入和时间选择器

> 原文：<https://blog.devgenius.io/nativescript-vue-text-input-and-time-picker-5d8631acd26c?source=collection_archive---------3----------------------->

![](img/32becb1895bd7d891f1c2ff5d8c5ffb2.png)

杰克·特洛特曼在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Vue 是一个易于使用的构建前端应用的框架。

NativeScript 是一个移动应用程序框架，它允许我们使用流行的前端框架构建原生移动应用程序。

在本文中，我们将了解如何使用 NativeScript Vue 构建一个应用程序。

# 文本字段

组件让我们在应用程序中添加文本输入。

例如，我们可以这样使用它:

```
<template>
  <Page>
    <ActionBar title="NativeScript App"></ActionBar>
    <FlexboxLayout flexDirection="column">
      <TextField v-model="textFieldValue" />
      <Label :text="textFieldValue" style="text-align: center" />
    </FlexboxLayout>
  </Page>
</template><script >
export default {
  data() {
    return {
      textFieldValue: "",
    };
  },
};
</script>
```

我们添加了`TextField`组件和`v-model`指令来将输入值绑定到`textFieldValue`反应属性。

然后我们在`Label`中显示这个值。

所以当我们输入一些东西时，输入的值就会显示出来。

我们还可以添加`hint`道具来添加一个输入占位符。

而`secure`道具在`true`时隐藏输入的文本。

还需要使用`autocorrect`属性来启用或禁用自动更正。

# 文本视图

我们可以向 NativeScript Vue 应用程序添加一个`TextView`来显示一个可编辑或只读的多行文本容器。

要添加一个可编辑的`TextView`，我们可以写:

```
<template>
  <Page>
    <ActionBar title="NativeScript App"></ActionBar>
    <FlexboxLayout flexDirection="column">
      <TextView v-model="textViewValue" />
      <Label :text="textViewValue" style="text-align: center" />
    </FlexboxLayout>
  </Page>
</template><script >
export default {
  data() {
    return {
      textViewValue: "",
    };
  },
};
</script>
```

我们用`v-model`指令将输入值绑定到`textViewValue`。

此外，我们可以用它来显示多种样式的文本:

```
<template>
  <Page>
    <ActionBar title="NativeScript App"></ActionBar>
    <FlexboxLayout flexDirection="column">
      <TextView :editable="false">
        <FormattedString>
          <Span text="You can use text attributes such as " />
          <Span text="bold, " fontWeight="Bold" />
          <Span text="italic " fontStyle="Italic" />
          <Span text="and " />
          <Span text="underline." textDecoration="Underline" />
        </FormattedString>
      </TextView>
    </FlexboxLayout>
  </Page>
</template><script >
export default {
  data() {
    return {
      textViewValue: "",
    };
  },
};
</script>
```

我们将`editable`属性设置为`false`来禁用编辑。

然后我们添加`FormattedString`和`Span`组件来添加样式化的文本。

# 时间选择器

我们可以使用`TimePicker`组件将时间选择器添加到我们的 NativeScript Vue 应用程序中。

例如，我们可以写:

```
<template>
  <Page>
    <ActionBar title="NativeScript App"></ActionBar>
    <FlexboxLayout flexDirection="column">
      <TimePicker v-model="selectedTime" />
      <Label :text="selectedTime" style="text-align: center" />
    </FlexboxLayout>
  </Page>
</template><script >
export default {
  data() {
    return {
      selectedTime: undefined,
    };
  },
};
</script>
```

我们用`v-model`将选择的时间值绑定到`selectedTime`反应属性。

我们用`Label`显示选中的项目。

我们可以设置以下道具来配置`TimePicker`:

*   `hourNumber` —获取或设置选定的小时。
*   `minuteNumber` —获取或设置选定的分钟。
*   `timeDate` —获取或设置选定的时间。
*   `minHourNumber` —获取或设置最小可选小时。
*   `maxHourNumber` —获取或设置最大可选小时数。
*   `minMinuteNumber` —获取或设置最小可选分钟数。
*   `maxMinuteNumber` —获取或设置最大可选分钟数。
*   `minuteIntervalNumber` —获取或设置可选的分钟间隔。

# 结论

我们可以在 NativeScript Vue 移动应用程序中添加各种输入控件。