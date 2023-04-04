# NativeScript Vue —导航、切换和滑块

> 原文：<https://blog.devgenius.io/nativescript-vue-navigation-toggles-and-sliders-f91e09278a0?source=collection_archive---------7----------------------->

![](img/197c530d3443b1268f870e6d9472192d.png)

[卡梅隆·威特尼](https://unsplash.com/@camwitney?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Vue 是一个易于使用的构建前端应用的框架。

NativeScript 是一个移动应用程序框架，它允许我们使用流行的前端框架构建原生移动应用程序。

在本文中，我们将了解如何使用 NativeScript Vue 构建一个应用程序。

# 分段条形图

`SegmentedBar`组件让我们显示一组按钮，并让我们通过单击一个按钮来进行选择。

例如，我们可以写:

```
<template>
  <Page>
    <ActionBar title="NativeScript App"></ActionBar>
    <FlexboxLayout flexDirection="column">
      <SegmentedBar>
        <SegmentedBarItem title="First" />
        <SegmentedBarItem title="Second" />
        <SegmentedBarItem title="Third" />
      </SegmentedBar>
    </FlexboxLayout>
  </Page>
</template><script >
export default {};
</script>
```

再加上`SegmentedBar`。

将`flexDirection`设置为`'column'`，以便并排显示`SegmenteBarItem`。

我们还可以使用`v-model`指令将所选条形项目的索引绑定到一个反应性属性:

```
<template>
  <Page>
    <ActionBar title="NativeScript App"></ActionBar>
    <FlexboxLayout flexDirection="column">
      <SegmentedBar v-model="selectedItem">
        <SegmentedBarItem
          v-for="item in listOfItems"
          :key="item"
          :title="item.title"
        />
      </SegmentedBar>
      <Label :text="selectedItem" style="text-align: center" />
    </FlexboxLayout>
  </Page>
</template><script >
export default {
  data() {
    return {
      listOfItems: [
        { title: "apple" },
        { title: "orange" },
        { title: "grape" },
      ],
      selectedItem: 0,
    };
  },
};
</script>
```

我们使用`v-model`将`selectedItem`反应属性绑定到选定的索引。

所以当我们点击一个项目时，我们会看到它的索引显示在`Label`中。

# 滑块

`Slider`组件是一个 UI 组件，它显示了一个滑块控件，用于选取指定数值范围内的值。

例如，我们可以写:

```
<template>
  <Page>
    <ActionBar title="NativeScript App"></ActionBar>
    <FlexboxLayout flexDirection="column">
      <Slider v-model="value" />
      <Label :text="value" style="text-align: center" />
    </FlexboxLayout>
  </Page>
</template><script >
export default {
  data() {
    return {
      value: 0,
    };
  },
};
</script>
```

添加数字滑块并将所选值绑定到`value`反应属性。

我们可以设置`minValue`和`maxValue`道具来分别设置我们可以选择的最小值和最大值。

默认值分别为 0 和 100。

# 转换

然后`Switch`组件让我们在应用程序中添加一个拨动开关。

例如，我们可以这样使用它:

```
<template>
  <Page>
    <ActionBar title="NativeScript App"></ActionBar>
    <FlexboxLayout flexDirection="column">
      <Switch v-model="itemEnabled" />
      <Label :text="itemEnabled" style="text-align: center" />
    </FlexboxLayout>
  </Page>
</template><script >
export default {
  data() {
    return {
      itemEnabled: false,
    };
  },
};
</script>
```

我们用`v-model`将拨动开关的值绑定到`itemEnabled`反应属性。

选择的值显示在`Label`中。

# 选项卡视图

`TabView`是一个导航组件，显示分组到选项卡中的内容，并让用户在它们之间切换。

例如，我们可以这样使用它:

```
<template>
  <Page>
    <ActionBar title="NativeScript App"></ActionBar>
    <FlexboxLayout flexDirection="column">
      <TabView
        :selectedIndex="selectedIndex"
        @selectedIndexChange="indexChange"
      >
        <TabViewItem title="Tab 1">
          <Label text="Content for Tab 1" />
        </TabViewItem>
        <TabViewItem title="Tab 2">
          <Label text="Content for Tab 2" />
        </TabViewItem>
      </TabView>
    </FlexboxLayout>
  </Page>
</template><script >
export default {
  data() {
    return {
      selectedIndex: 0,
    };
  },
  methods: {
    indexChange({ value: newIndex }) {
      this.selectedIndex = newIndex;
      console.log(newIndex);
    },
  },
};
</script>
```

当发出`selectedIndexChange`事件时，我们添加`TabView`并将`selectedIndex`反应属性设置为参数对象中`value`属性的值。

选项卡内容由`TabViewItem`组件呈现。

`Label` s 就是内容。

# 结论

我们可以添加一个分段栏和标签视图，将导航添加到我们的 NativeScript Vue 移动应用程序中。

NativeScript Vue 还提供数字滑块和切换。