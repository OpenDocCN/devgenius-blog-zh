# NativeScript Vue —标签、列表视图和列表选取器

> 原文：<https://blog.devgenius.io/nativescript-vue-labels-listviews-and-list-pickers-a7032784f13e?source=collection_archive---------3----------------------->

![](img/72c37b9ea7610acaf8080e8760a89cd8.png)

丹尼斯·罗谢尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Vue 是一个易于使用的构建前端应用的框架。

NativeScript 是一个移动应用程序框架，它允许我们使用流行的前端框架构建原生移动应用程序。

在本文中，我们将了解如何使用 NativeScript Vue 构建一个应用程序。

# 标签

我们可以用`Label`组件给我们的应用添加一个标签。

例如，我们可以写:

```
<template>
  <Page>
    <ActionBar title="NativeScript App"></ActionBar>
    <FlexboxLayout flexDirection="column">
      <Label text="Label" />
    </FlexboxLayout>
  </Page>
</template><script >
export default {};
</script>
```

在我们的应用程序中添加标签。

`text`道具有要显示的文本。

我们还可以添加带格式的文本:

```
<template>
  <Page>
    <ActionBar title="NativeScript App"></ActionBar>
    <FlexboxLayout flexDirection="column">
      <Label textWrap>
        <FormattedString>
          <Span
            text="hello world"
            fontWeight="bold"
            fontStyle="italic"
            style="color: red"
          />
        </FormattedString>
      </Label>
    </FlexboxLayout>
  </Page>
</template><script >
export default {};
</script>
```

我们添加了`FormattedString`组件来添加格式化字符串。

`Span`有我们想要添加的文本、字体粗细、字体样式和其他样式。

# 列表选取器

我们可以添加一个组件，让用户用`ListPicker`组件进行选择。

例如，我们可以写:

```
<template>
  <Page>
    <ActionBar title="NativeScript App"></ActionBar>
    <FlexboxLayout flexDirection="column">
      <ListPicker
        :items="listOfItems"
        selectedIndex="0"
        @selectedIndexChange="selectedIndexChanged"
      />
    </FlexboxLayout>
  </Page>
</template><script >
export default {
  data() {
    return {
      listOfItems: ["apple", "orange", "grape"],
    };
  },
  methods: {
    selectedIndexChanged({ value }) {
      console.log(value);
    },
  },
};
</script>
```

我们添加带有`items`属性的`ListPicker`组件来设置显示的项目。

`selectedIndex`设置为 0 默认选择第一项。

我们监听组件发出的`selectedIndexChange`事件。

我们可以用`value`道具得到所选组件的索引。

我们可以用`v-model`指令来简化它:

```
<template>
  <Page>
    <ActionBar title="NativeScript App"></ActionBar>
    <FlexboxLayout flexDirection="column">
      <ListPicker :items="listOfItems" v-model="selectedItem" />
      <Label :text="selectedItem" style="text-align: center" />
    </FlexboxLayout>
  </Page>
</template><script >
export default {
  data() {
    return {
      listOfItems: ["apple", "orange", "grape"],
      selectedItem: "",
    };
  },
};
</script>
```

它将所选项目的索引绑定到`selectedItem`反应属性。

# 列表视图

我们可以用`ListView`组件添加一个垂直滚动的列表视图。

例如，我们可以写:

```
<template>
  <Page>
    <ActionBar title="NativeScript App"></ActionBar>
    <FlexboxLayout flexDirection="column">
      <ListView for="item in listOfItems" [@itemTap](http://twitter.com/itemTap)="onItemTap">
        <v-template>
          <Label :text="item.text" />
        </v-template>
      </ListView>
    </FlexboxLayout>
  </Page>
</template><script >
export default {
  data() {
    return {
      listOfItems: [
        {
          text: "apple",
        },
        { text: "orange" },
        {
          text: "grape",
        },
      ],
      selectedItem: "",
    };
  },
  methods: {
    onItemTap({ item }) {
      console.log(item);
    },
  },
};
</script>
```

添加`ListView`组件来显示我们代码中的对象。

我们在默认槽中添加了一个`Label`来以我们喜欢的方式显示项目。

当我们点击一个项目时，就会发出`itemTap`事件。

然后我们可以用`onItemTap`方法得到点击的项目。

此外，我们可以添加多个`v-template`块。

例如，我们可以写:

```
<template>
  <Page>
    <ActionBar title="NativeScript App"></ActionBar>
    <FlexboxLayout flexDirection="column">
      <ListView for="item in listOfItems" @itemTap="onItemTap">
        <v-template>
          <Label :text="item.text" />
        </v-template> <v-template if="$odd">
          <Label :text="item.text" color="red" />
        </v-template>
      </ListView>
    </FlexboxLayout>
  </Page>
</template><script >
export default {
  data() {
    return {
      listOfItems: [
        {
          text: "apple",
        },
        { text: "orange" },
        {
          text: "grape",
        },
      ],
      selectedItem: "",
    };
  },
  methods: {
    onItemTap({ item }) {
      console.log(item);
    },
  },
};
</script>
```

我们将`if`属性添加到`v-template`中，以显示具有奇数索引和`$odd`反应属性的项目的不同之处。

此外，我们可以使用`$even`来检查一个项目是否有一个偶数索引。

`$index`有该项的索引。

`ListView`不会像我们期望的`v-for`那样遍历列表项。

它只是创建所需的视图，以便在屏幕上显示当前可见的项目。

这些视图被屏幕外的项目重新使用，现在显示在屏幕上。

# 结论

我们可以使用 NativeScript Vue 在我们的移动应用程序中添加标签、列表选取器和列表视图。