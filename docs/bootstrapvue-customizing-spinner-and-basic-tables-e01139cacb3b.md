# BootstrapVue —自定义微调器和基本表格

> 原文：<https://blog.devgenius.io/bootstrapvue-customizing-spinner-and-basic-tables-e01139cacb3b?source=collection_archive---------12----------------------->

![](img/150a81b282e93498e952784912b4bac2.png)

安妮·斯普拉特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

为了制作好看的 Vue 应用，我们需要设计组件的样式。

为了使我们的生活更容易，我们可以使用内置样式的组件。

我们看看如何定制微调器和添加基本表。

# 对齐旋转器

我们可以添加类或对齐。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-spinner class="m-5"></b-spinner>
  </div>
</template><script>
export default {};
</script>
```

添加`m-5`类，在微调器周围添加一些边距。

# 安置

微调器的位置可以改变。

例如，我们可以写:

```
<template>
  <div id="app">
    <div class="d-flex align-items-center">
      <b-spinner></b-spinner>
    </div>
  </div>
</template><script>
export default {};
</script>
```

居中对齐微调器。

# 漂浮物

我们可以应用 Bootstrap 附带的 float 类将微调器放在左边或右边。

所以我们可以写:

```
<template>
  <div id="app">
    <div class="clearfix">
      <b-spinner class="float-right"></b-spinner>
    </div>
  </div>
</template><script>
export default {};
</script>
```

使用`clearfix`类，这样我们可以将微调器浮动到右边。

# 文本对齐

我们可以使用`text-center`类将微调器与页面中心对齐:

```
<template>
  <div id="app">
    <div class="text-center">
      <b-spinner></b-spinner>
    </div>
  </div>
</template><script>
export default {};
</script>
```

# 按钮中的微调器

我们可以把旋转器放在纽扣里。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-button variant="primary" disabled>
      <b-spinner small></b-spinner>Loading...
    </b-button>
  </div>
</template><script>
export default {};
</script>
```

我们有一个按钮里面的微调器和一些文本。

# 桌子

BootstrapVue 附带了`b-table`组件，让我们可以显示表格。

重量更轻的备选方案有 2 个，分别是`b-table-lite`和`b-table-simple`。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-table striped hover :items="items"></b-table>
  </div>
</template><script>
export default {
  data() {
    return {
      items: [
        { firstName: "alex", lastName: "green" },
        { firstName: "may", lastName: "smith" },
        { firstName: "james", lastName: "jones" }
      ]
    };
  }
};
</script>
```

我们有`items`属性，它将一组对象呈现到一个表中。

`striped`使各行颜色交替。

`hover`悬停时高亮显示行。

属性名自动映射到单个单词，并使每个单词大写。

这种转换是针对 kebab case、snake case 或 camelCase 属性名称进行的。

我们可以添加`_rowVariant`或`_cellVariant`属性来改变各种单元格的样式。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-table striped hover :items="items"></b-table>
  </div>
</template><script>
export default {
  data() {
    return {
      items: [
        { firstName: "alex", lastName: "green", _rowVariant: "success" },
        {
          firstName: "may",
          lastName: "smith",
          _cellVariants: { lastName: "info", firstName: "warning" }
        },
        { firstName: "james", lastName: "jones" }
      ]
    };
  }
};
</script>
```

`_rowVariant`使整行颜色相同。

`'success'`使背景变成绿色

`_cellVariants`使单个细胞呈现不同的颜色。

所以`lastName`是浅蓝色的，因为它是 hs 变体`'info'`。

而`firstName`的变体是`'warning'`所以是黄色的。

`_cellVariants`和`_rowVariants`只适用于它们所在的条目。

# 菲尔茨

我们可以明确指定字段的显示方式。

`b-table`可以使用一个带有字段数组的`fields`道具来显示。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-table striped hover :items="items" :fields='fields'></b-table>
  </div>
</template><script>
export default {
  data() {
    return {
      fields: ["firstName"],
      items: [
        { firstName: "alex", lastName: "green" },
        {
          firstName: "may",
          lastName: "smith"
        },
        { firstName: "james", lastName: "jones" }
      ]
    };
  }
};
</script>
```

因为`fields`是`[“firstName”]`，所以只显示名字列。

# 作为对象数组的字段

我们可以为`fields`数组准备一个对象数组。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-table striped hover :items="items" :fields="fields"></b-table>
  </div>
</template><script>
export default {
  data() {
    return {
      fields: [
        {
          key: "lastName",
          sortable: true
        },
        {
          key: "firstName",
          sortable: false,
          variant: "success"
        }
      ],
      items: [
        { firstName: "alex", lastName: "green" },
        {
          firstName: "may",
          lastName: "smith"
        },
        { firstName: "james", lastName: "jones" }
      ]
    };
  }
};
</script>
```

我们有一个`fields`数组，其中的条目是`key`、`sortable`和`variant`。

`key`是`items`条目的属性名。

`variant`是类似于`primary`或`success`的造型变体，

`sortable`指示列是否可排序。

因此，最后一列是可排序的，而名字是不可排序的。

![](img/ad1f44cc581ccdcd5ddb971a788c5145.png)

[zo Reeve](https://unsplash.com/@zoeeee_?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

# 结论

对于某些类，我们可以按照自己喜欢的方式设计和放置微调器。

此外，我们可以通过将一些数组作为道具传入`b-table`组件来创建简单的表。