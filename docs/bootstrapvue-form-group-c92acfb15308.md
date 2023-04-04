# BootstrapVue —表单组

> 原文：<https://blog.devgenius.io/bootstrapvue-form-group-c92acfb15308?source=collection_archive---------11----------------------->

![](img/1040fb79f7bf1a16e347f354e10f8a40.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上 [Louis Z S](https://unsplash.com/@ludwigschr98834234?utm_source=medium&utm_medium=referral) 拍摄的照片

为了制作好看的 Vue 应用，我们需要设计组件的样式。

为了使我们的生活更容易，我们可以使用内置样式的组件。

在本文中，我们将看看如何用表单组对输入进行分组。

# 形式组合

表单组让我们给表单添加一些结构。

`b-form-group`是一个表单组。

我们可以控制图例和标签的放置，并放置验证反馈文本。

例如，我们可以这样使用它:

```
<template>
  <div id="app">
    <b-form-group
      description="Your name."
      label="Your name"
      label-for="input-1"
      :invalid-feedback="invalidFeedback"
      :valid-feedback="validFeedback"
      :state="state"
    >
      <b-form-input trim id="input-1" v-model="name" :state="state"></b-form-input>
    </b-form-group>
  </div>
</template><script>
export default {
  computed: {
    state() {
      return this.name.length > 0;
    },
    invalidFeedback() {
      if (this.name.length > 0) {
        return "";
      } else {
        return "Please enter a name";
      }
    },
    validFeedback() {
      return this.state ? "Looks good" : "";
    }
  },
  data() {
    return {
      name: ""
    };
  }
};
</script>
```

我们使用`b-form-group`组件。

它有表单验证的反馈。

它还包括表单输入标签。

`description`道具让我们设置显示在表单输入下方的描述文本。

`label`具有表单输入的 labvel。

`invalid-feedback`设置为输入值无效时返回反馈文本的函数。

`valid-feedback`设置为当输入值有效时返回反馈文本的功能。

`state`被设置为验证状态。

然后，当我们输入有效的内容时，比如包含 1 个或更多字符的文本，它会以绿色显示有效文本。

否则，它会以红色显示无效值文本。

输入框的颜色也有相应的变化，因为我们为此设置了`state`道具。

# 水平布局

我们可以添加`label-cols-sm`和`label-cols-klg`道具来改变标签的列宽。

这样，它们可以小于视口的整个宽度。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-group
      description="Enter your name."
      label="Enter your name"
      label-for="input-horizontal"
      label-cols-sm="4"
      label-cols-lg="3"
    >
      <b-form-input id="input-horizontal"></b-form-input>
    </b-form-group>
  </div>
</template><script>
export default {};
</script>
```

`label-cols-sm`是屏幕较窄时标签的列大小。

`lable-cols-lg`是宽屏时的尺寸。

# 标签尺寸

标签大小也可以改变。

可以用`label-size`支柱更换。

该值可以是`'sm'`或`'lg'`。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-group description="Enter your name." label="Enter your name" label-size="sm">
      <b-form-input id="input-horizontal"></b-form-input>
    </b-form-group>
  </div>
</template><script>
export default {};
</script>
```

代码小于`label-size=”sm”`的默认尺寸。

# 标签文本对齐

我们可以用`label-align`支架对齐不同位置的标签。

适用于断点`xs`及以上。

该值可以是`left`、`center`或`right`。

`label-align-sm`适用于断点`sm`及以上。

`label-align-md`适用于断点`md`及以上。

`label-align-lg`适用于断点`lg`及以上。

`label-align-xl`适用于断点`xl`及以上。

# 嵌套表单组

表单组可以嵌套。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-group label-cols-lg="3" label="Person" label-size="lg" class="mb-0">
      <b-form-group
        label-cols-sm="3"
        label="Name:"
        label-align-sm="right"
        label-for="nested-name"
      >
        <b-form-input id="nested-name"></b-form-input>
      </b-form-group>
    </b-form-group>
  </div>
</template><script>
export default {};
</script>
```

我们需要`b-form-group`组件。

`label`为外组设置。

同样，我们在内部表单组上有`label`。

![](img/85feea18f70345a0ec05a8460999e20b.png)

[Jason Leung](https://unsplash.com/@ninjason?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

我们可以用这个组件呈现的标签和表单验证状态来创建表单组。

表单元素的大小可以更改。

此外，我们可以嵌套表单组。