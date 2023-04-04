# vue-多选—标签和模板

> 原文：<https://blog.devgenius.io/vue-multiselect-tags-and-templates-afc1843704e?source=collection_archive---------6----------------------->

![](img/75b4fe84ec95e454d29a56bef693417d.png)

科林·梅纳德在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

下拉菜单通常是我们想要添加到 Vue 应用中的东西。

为了让我们的生活更轻松，我们可以使用一个组件库来添加它。

在这篇文章中，我们将看看如何创建一个带有标签和模板的下拉菜单。

# 磨尖

我们可以用`taggable`道具让下拉列表变得可标记。

此外，我们可以添加一个带有`tag-placeholder`属性的标记占位符。

`tag-position`可以改变标记的位置。

例如，我们可以写:

```
<template>
  <div id="app">
    <multiselect
      v-model="value"
      taggable
      multiple
      tag-placeholder="Add this as new tag"
      tag-position="bottom"
      :options="options"
      @tag="addTag"
    ></multiselect>
  </div>
</template><script>
export default {
  data() {
    return {
      value: null,
      options: ["Vue.js", "React", "Rails"]
    };
  },
  methods: {
    addTag(newTag) {
      this.options.push(newTag);
    }
  }
};
</script>
```

我们有`tag`事件处理程序，它被设置为`addTag`函数。

`newTag`参数有了新的标签。

如果我们键入列表中没有的选项，我们会看到“将此作为新标签添加”的消息。

在我们按下回车键或单击消息后，我们也可以选择新选项。

# 自定义选项模板

我们可以按照自己的方式显示选项。

例如，我们可以写:

```
<template>
  <div id="app">
    <multiselect v-model="value" :options="options">
      <template slot="singleLabel" slot-scope="props">
        <span>{{ props.option.name }}</span>
      </template>
      <template slot="option" slot-scope="props">
        <span>{{ props.option.name }} - {{ props.option.lang }}</span>
      </template>
    </multiselect>
  </div>
</template><script>
export default {
  data() {
    return {
      value: null,
      options: [
        { name: "Vue.js", lang: "JavaScript" },
        { name: "React", lang: "JavaScript" },
        { name: "Rails", lang: "Ruby" }
      ]
    };
  }
};
</script>
```

在下拉列表中，我们看到了`name`和`lang`值，因为我们用这两个值填充了`option`槽。

然而，当我们选择一个选项时，我们只能看到`name`值，因为我们用它填充了`singleLabel`槽。

# 选项组

我们可以在下拉列表中添加选项组。

例如，我们可以写:

```
<template>
  <div id="app">
    <multiselect
      v-model="value"
      :options="options"
      group-values="libs"
      group-label="language"
      track-by="name"
      group-select
      :custom-label="customLabel"
    >
      <template slot="noResult">
        <span>no result found.</span>
      </template>
    </multiselect>
  </div>
</template><script>
export default {
  data() {
    return {
      value: null,
      options: [
        {
          language: "Javascript",
          libs: [
            { name: "Vue.js", category: "Front-end" },
            { name: "React", category: "Backend" }
          ]
        },
        {
          language: "Ruby",
          libs: [
            { name: "Rails", category: "Backend" },
            { name: "Sinatra", category: "Backend" }
          ]
        },
        {
          language: "Other",
          libs: [{ name: "Laravel", category: "Backend" }]
        }
      ]
    };
  },
  methods: {
    customLabel({ name, category }) {
      return `${name} – ${category}`;
    }
  }
};
</script>
```

我们可以用`group-select`道具选择整个组。

`group-label`让我们选择显示为组标签的字段。

`groyup-values`让我们选择分组字段。

`options`拥有所有选项。

`custom-label`具有为每个条目显示自定义标签的功能。

# 自定义配置

我们可以为我们的`multiselect` 组件添加定制配置。

`max-height`以像素为单位设置它的高度。

`max`设置最大选择数。

`allow-empty`如果是`false`，不允许用户删除最后一个选项(如果存在)。

`block-keys`是我们想要禁止触发行为的按键。

让我们传入一个当下拉菜单关闭时运行的事件处理程序。

例如，我们可以写:

```
<template>
  <div id="app">
    <multiselect v-model="value" multiple :max="3" :options="options"></multiselect>
  </div>
</template><script>
export default {
  data() {
    return {
      value: null,
      options: ["foo", "bar", "baz", "qux", "apple"]
    };
  }
};
</script>
```

那么最多可以选择 3 个选项。

否则，我们会看到一条错误消息。

# 事件

`multiselect` 组件发出各种事件。

当选择值改变时，发出`input`。

`select`当用户选择一个选项时发出。

`remove`是去掉一个选项后发出的。

当搜索查询改变时发出`SearchChangew`。

当用户试图添加标签时发出`tag`。

当下拉菜单打开时发出`open`。

当下拉菜单关闭时发出`close`。

![](img/2117f342ec96ef151c34d468f1e7ca83.png)

由 [Behzad Ghaffarian](https://unsplash.com/@behz?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

# 结论

Vue-Multiselect 非常灵活。

我们可以更改许多选项，如最大高度、可以选择的选项数量等等。

我们还可以添加标签，并通过填充槽来改变选项的显示方式。