# BootstrapVue —复选框和数据列表

> 原文：<https://blog.devgenius.io/bootstrapvue-checkboxes-and-data-lists-5c12556fe047?source=collection_archive---------11----------------------->

![](img/fd91e14f65d76165d867f8158b5c8873.png)

照片由[约尔·温克勒](https://unsplash.com/@yoel100?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

为了制作好看的 Vue 应用，我们需要设计组件的样式。

为了使我们的生活更容易，我们可以使用内置样式的组件。

在本文中，我们将看看如何用 BootstrapVue 创建更多的复选框和数据列表。

# 数据列表

我们可以添加一个 datalist 元素，让我们创建一个本地的 autocomplete 输入。

要添加它，我们可以使用`b-form-datalist`组件。

例如，我们可以写:

```
<template>
  <div id="app">
    <label for="fruit">fruit</label>
    <b-form-input list="fruit-list" id="fruit"></b-form-input>
    <b-form-datalist id="fruit-list" :options="options"></b-form-datalist>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
     options: ['apple', 'orange', 'grape']
    };
  },
};
</script>
```

我们有一个`b-form-input`，它为我们提供了一个输入文本框。

然后我们使用`b-form-datalist`来添加一个自动完成列表，我们可以从中选择`options`数组中列出的条目。

我们将`options`道具添加到`b-form-datalist`来设置可用选项。

`b-form-datalist`的`id`值必须与`b-form-input`中的`list`属性值相同，这样`b-form-datalist`将用于填充选项。

# 检验盒

要向表单添加复选框，我们可以使用`b-form-checkbox`组件。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-checkbox v-model="status" name="checkbox" value="yes" unchecked-value="no">accept?</b-form-checkbox> <div>
      State:
      {{ status }}
    </div>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      status: ""
    };
  }
};
</script>
```

我们有`b-form-checkbox`组件。

它有`v-model`指令将值绑定到我们想要的状态。

同样，如果复选框被选中，我们设置`value`属性来设置`status`模型的值。

同样，如果复选框未选中，我们可以用`unchecked-value`来设置`status`模型的值。

然后，当我们在选中和未选中之间切换复选框时，我们会看到下面的文本在“是”或“否”之间切换。

# 多选复选框

我们可以用`b-form-checkbox-group`组件创建多个复选框。

例如，我们可以编写以下代码来创建一个:

```
<template>
  <div id="app">
    <b-form-group label="fruits">
      <b-form-checkbox-group v-model="selected" :options="options" name="fruits"></b-form-checkbox-group>
    </b-form-group>
    <div>{{ selected }}</div>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      options: ["apple", "orange", "grape"],
      selected: []
    };
  }
};
</script>
```

`label`道具有标签内容。

我们有`selected`状态来保存选择的值。它必须是一个数组。

用于呈现带有给定选项的复选框的`options`数组。

现在，当我们单击复选框时，我们会看到选定的项目。

同样，我们可以将`b-form-checkbox-group`和`b-form-checkbox`结合使用，以获得更大的灵活性。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-group label="fruit:">
      <b-form-checkbox-group id="friots" v-model="selected" name="fruits">
        <b-form-checkbox value="orange">orange</b-form-checkbox>
        <b-form-checkbox value="apple">apple</b-form-checkbox>
        <b-form-checkbox value="grape">grape</b-form-checkbox>
      </b-form-checkbox-group>
    </b-form-group>
    <div>{{ selected }}</div>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      selected: []
    };
  }
};
</script>
```

我们添加了`b-form-checkbox`组件，而不是使用`options`道具来渲染复选框。

`value`道具有一个值，如果我们选中一个复选框，该值将被设置。

因此，我们得到了与前面例子相同的结果。

# 选项数组

选项数组的每个条目最多可以有 3 个属性。

可具有`text`、`value`、`disabled`特性。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-group label="fruits">
      <b-form-checkbox-group v-model="selected" :options="options" name="fruits"></b-form-checkbox-group>
    </b-form-group>
    <div>{{ selected }}</div>
  </div>
</template>
<script>
export default {
  name: "App",
  data() {
    return {
      options: [
        { text: "orange", value: "orange", disabled: false },
        { text: "apple", value: "apple", disabled: false },
        { text: "grape", value: { grape: "grape" }, disabled: true }
      ],
      selected: []
    };
  }
};
</script>
```

代替字符串数组，我们有一个具有`text`、`value`和`disabled`属性的对象数组。

如果`disabled`为`true`，则该复选框将被禁用。

`value`属性可以有任何东西。如果我们选中复选框，该值将出现在`selected`数组中。

![](img/73161a356d7f387e7e6017e2bc9a0caa.png)

照片由[达尼洛·奥布拉多维奇](https://unsplash.com/@tamentali?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 结论

我们可以用`b-form-checkbox`和相关组件创建复选框。

它非常灵活，我们可以根据需要更改值和文本。

此外，我们可以选择启用或禁用复选框

数据列表让我们创建一个简单的自动完成输入。