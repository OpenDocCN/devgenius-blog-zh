# BootstrapVue —文本区域定制和时间选择器

> 原文：<https://blog.devgenius.io/bootstrapvue-text-area-customization-and-time-picker-bd68e535fb5b?source=collection_archive---------32----------------------->

![](img/ecd0b78aee29d2f06905f55f356003f2.png)

丹尼尔·克莱因在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

为了制作好看的 Vue 应用，我们需要设计组件的样式。

为了使我们的生活更容易，我们可以使用内置样式的组件。

在本文中，我们将了解如何定制文本区域并添加时间选择器。

# `v-model`修饰语

`b-form-textarea`组件不支持`v-model`的`lazy`、`trim`、`number`修改器。

不过有`trim`、`number`、`lazy`道具可以用来替换那些修改器。

# 去抖

我们可以添加`debounce` prop 来延迟绑定到状态的输入值。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-textarea v-model="text" placeholder="Enter text" debounce="500" ></b-form-textarea>
    <p>{{text}}</p>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      text: ""
    };
  }
};
</script>
```

然后，我们将输入值绑定延迟 500 毫秒。

# 自（动）调焦装置

`autofocus`支柱可以添加到`b-form-textarea`上，使其在`keep-alive`组件加载或反应时聚焦。

# 时间选择器

我们可以使用`b-form-timepicker`组件来添加一个时间选择器。

例如，我们可以写:

```
<template>
  <div id="app">
<b-form-timepicker v-model="value" locale="en"></b-form-timepicker>
    <div>Value: {{ value }}</div>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: ""
    };
  }
};
</script>
```

然后我们可以从显示的输入框中选择时间。

这个`locale`道具让我们改变区域设置。

`value`有选择值，用`v-model`绑定到输入值。

# 禁用或只读状态

我们可以添加`disabled`道具来移除`b-form-timepicker`组件上的所有交互性。

`readonly`禁止选择时间，但将保持组件的交互性。

我们可以写:

```
<b-form-timepicker v-model="value" disabled></b-form-timepicker>
```

或者:

```
<b-form-timepicker v-model="value" readonly></b-form-timepicker>
```

去改变这两者。

# 验证状态

像其他输入注释一样，我们可以用它来显示验证状态。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-timepicker v-model="value" :state="!!value"></b-form-timepicker>
    <div>Value: {{ value }}</div>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: ""
    };
  }
};
</script>
```

我们将`state`属性设置为转换为布尔值的`value`的值。

如果没有选择时间，我们会看到一个红框。

否则，我们会看到一个绿色框。

# 设置秒数

我们可以通过添加`show-seconds`道具来设置秒数。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-timepicker v-model="value" show-seconds></b-form-timepicker>
    <div>Value: {{ value }}</div>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: ""
    };
  }
};
</script>
```

现在我们看到秒框，我们可以设置秒。

# 胶料

我们可以添加`size`道具来改变时间选择器的大小。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-timepicker v-model="value" size="sm"></b-form-timepicker>
    <div>Value: {{ value }}</div>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: ""
    };
  }
};
</script>
```

缩小时间选择器的大小。

我们还可以将该值更改为`'lg'`，使其大于默认值。

# 可选控件

我们可以用一些小道具给时间选择器添加可选控件。

例如，我们可以添加`now-button`让用户选择当前时间。

`reset-button`显示重置按钮，让用户重置时间。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-timepicker v-model="value" now-button reset-button></b-form-timepicker>
    <div>Value: {{ value }}</div>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: ""
    };
  }
};
</script>
```

然后我们可以看到时间选择器上的按钮。

# 仅按钮模式

`button-only`道具可以让我们将时间选择器更改为只有按钮的模式。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-input-group class="mb-3">
      <b-form-input v-model="value" type="text" placeholder="Select time"></b-form-input>
      <b-input-group-append>
        <b-form-timepicker v-model="value" right button-only></b-form-timepicker>
      </b-input-group-append>
    </b-input-group>
    <div>Value: {{ value }}</div>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: ""
    };
  }
};
</script>
```

然后我们有一个显示时间的输入框。

但是我们不能点击它来显示时间选择器。

输入框右边的按钮让我们选择时间。

我们需要`right`道具，以便日期选择器的右边缘与按钮的右侧对齐。

![](img/3139eb8b6d0c0b2e6fa1cd3961372fc9.png)

照片由[迈克·本纳](https://unsplash.com/@mbenna?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以在文本区域添加谴责。

此外，我们可以添加一个时间选择器，让我们添加一个时间选择器控件并对其进行自定义。