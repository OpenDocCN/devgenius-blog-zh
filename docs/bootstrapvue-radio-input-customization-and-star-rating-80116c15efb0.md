# BootstrapVue —收音机输入定制和星级评定

> 原文：<https://blog.devgenius.io/bootstrapvue-radio-input-customization-and-star-rating-80116c15efb0?source=collection_archive---------15----------------------->

![](img/c1fae1ee79f43f0258498e5cb8e5f067.png)

[乔·凯恩](https://unsplash.com/@joeyc?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

为了制作好看的 Vue 应用，我们需要设计组件的样式。

为了使我们的生活更容易，我们可以使用内置样式的组件。

在本文中，我们将了解如何定制单选按钮输入和创建星级输入。

# 胶料

道具可以让我们改变尺寸。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-radio name="size" size="sm">Small</b-form-radio>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

让我们的单选按钮变小。

我们也可以设置为`'lg'`让它变大。

# 按钮样式单选按钮

我们可以让单选按钮看起来像一个按钮。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-radio-group v-model="selected" :options="options" buttons name="radios"></b-form-radio-group>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      selected: "",
      options: [
        { text: "Apple", value: "apple" },
        { text: "Orange", value: "orange" }
      ]
    };
  }
};
</script>
```

我们添加了`buttons`道具，使它们看起来像按钮。

选定的选项看起来像是被按下了。

# 呈现为本机单选按钮

我们可以用`plain`道具将单选按钮渲染为本地单选按钮。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-radio-group plain v-model="selected" :options="options" name="radios"></b-form-radio-group>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      selected: "",
      options: [
        { text: "Apple", value: "apple" },
        { text: "Orange", value: "orange" }
      ]
    };
  }
};
</script>
```

那么将不会有引导样式应用于它们。

# 显示验证状态

为了显示验证状态，我们可以添加`state`属性。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-radio-group :state="state" v-model="selected" :options="options" name="radios"></b-form-radio-group>
    <b-form-invalid-feedback :state="state">please choose one</b-form-invalid-feedback>
    <b-form-valid-feedback :state="state">looks good</b-form-valid-feedback>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      selected: "",
      options: [
        { text: "Apple", value: "apple" },
        { text: "Orange", value: "orange" }
      ]
    };
  },
  computed: {
    state() {
      return this.selected.length > 0;
    }
  }
};
</script>
```

我们有`state` computed 属性，它检查`selected`状态是否被选项填充。

然后我们将它设置为`b-form-invalid-feedback`和`b-form-valid-feedback`的`state`道具值。

现在，当我们单击一个选项时，我们会看到所有内容都显示为绿色，包括验证消息。

另一方面，如果我们没有选择任何内容，那么所有内容都显示为红色。

# 表格评级

如果我们想添加一个星级输入时，我们可以使用`b-form-rating`组件。

这是从 2.12.0 开始提供的新组件。

例如，我们可以这样使用它:

```
<template>
  <div id="app">
    <b-form-rating v-model="value"></b-form-rating>
    <p>{{ value }}</p>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: 0
    };
  }
};
</script>
```

我们只需添加组件，并使用`v-model`将输入值设置为我们想要的状态。

然后我们展示一下`value`。

# 只读评级

`readonly`道具可以添加到组件中。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-rating v-model="value" readonly></b-form-rating>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: 4
    };
  }
};
</script>
```

我们会看到四颗星星。我们不能改变它的价值。

# 评级风格

我们可以使用`variant`道具来改变它的造型。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-rating v-model="value" variant="success"></b-form-rating>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: 4
    };
  }
};
</script>
```

然后星星是绿色的。

`variant`的其他可能值包括`'warning'`、`'success'`、`'danger'`、`'primary'`和`'info'`。

要把星星换成其他颜色，可以用`color`道具。

例如，我们写道:

```
<template>
  <div id="app">
    <b-form-rating v-model="value" color="indigo"></b-form-rating>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: 4
    };
  }
};
</script>
```

然后星星会变成靛蓝色。

![](img/0eff4057fcbca40b10a8116f5f7f5dcc.png)

约翰·麦肯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以改变单选按钮输入的大小。单选按钮也可以显示为按钮。

此外，我们可以向单选按钮组添加验证状态显示。

我们还可以添加一个开始评级输入组件，让用户设置评级分数。