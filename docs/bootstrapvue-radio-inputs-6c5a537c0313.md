# BootstrapVue —无线电输入

> 原文：<https://blog.devgenius.io/bootstrapvue-radio-inputs-6c5a537c0313?source=collection_archive---------19----------------------->

![](img/e8c9b28fa823156289cc87a5275593bd.png)

安德烈科·波迪尔尼克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

为了制作好看的 Vue 应用，我们需要设计组件的样式。

为了使我们的生活更容易，我们可以使用内置样式的组件。

在本文中，我们将看看如何添加单选按钮输入。

# 单选按钮

BootstrapVue 附带了`b-form-radio-group`组件，允许我们在表单中添加单选按钮。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-radio v-model="selected" name="fruit" value="apple">apple</b-form-radio>
    <b-form-radio v-model="selected" name="fruit" value="orange">orange</b-form-radio>
    <p>{{selected}}</p>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      selected: ""
    };
  }
};
</script>
```

我们有两个单选按钮具有相同的`name`属性值，并绑定到相同的状态。

因此，当我们点击一个按钮时，`selected`状态就会更新。

# 分组无线电

我们可以通过使用`b-form-radio-group`组件将它们组合在一起。

这样，我们不必单独添加每个单选按钮。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-radio-group v-model="selected" :options="options" name="fruit"></b-form-radio-group>
    <p>{{selected}}</p>
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

然后我们可以在一个组件中指定所有的单选按钮。

`options`有我们想要渲染的选项。

`selected`是我们要将值绑定到的状态。

我们可以在`b-form-radio-group`中混搭`options`道具和`b-form-radio`组件。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-radio-group
      id="radio-slots"
      v-model="selected"
      :options="options"
      name="radio-options-slots"
    >
      <template v-slot:first>
        <b-form-radio value="grape">Grape</b-form-radio>
      </template> <b-form-radio :value="{ banana: 'banana' }">Banana</b-form-radio>      
    </b-form-radio-group>
    <p>{{selected}}</p>
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

我们添加了各种单选按钮。

我们通过指定`options`属性添加了苹果和桔子。

香蕉是用`value`道具指定的。

我们添加了一个模板来添加`Grape`选项。我们指定要填充`first`槽，所以它会先出现。

现在，当我们单击按钮时，我们将看到选项。

我们还可以用`html`属性向标签添加 HTML 内容。

我们不使用`text`属性，而是使用`html`属性。

然而，这可能会使我们的应用程序容易受到跨站点脚本攻击，因为文本没有被净化。

我们可以写:

```
<template>
  <div id="app">
    <b-form-radio-group
      v-model="selected"
      :options="options"
      name="fruits"
    >      
    </b-form-radio-group>
    <p>{{selected}}</p>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      selected: "",
      options: [
        { html: "<b>Apple</b>", value: "apple" },
        { text: "Orange", value: "orange" }
      ]
    };
  }
};
</script>
```

# 更改选项字段名称

字段属性名称可以根据我们的偏好进行更改。

有一个`text-field`属性，它将选项 objecrt 的属性名显示为标签文本。

并且`value-field`属性让我们为值字段设置属性名。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-radio-group
      v-model="selected"
      :options="options"
      value-field="item"
      text-field="name"
      name="fruits"
    >      
    </b-form-radio-group>
    <p>{{selected}}</p>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      selected: "",
      options: [
        { name: "Apple", item: "apple" },
        { name: "Orange", item: "orange" }
      ]
    };
  }
};
</script>
```

然后我们用道具改变字段名。

显示`name`，`item`被设定为`selected`的值。

# 堆叠或内嵌

我们可以将单选按钮组堆叠起来或排成一行。

默认情况下，它是内联的。

我们可以加上`stacked`道具让它们叠起来。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-radio-group
      v-model="selected"
      :options="options"
      stacked
      name="fruits"
    >      
    </b-form-radio-group>
    <p>{{selected}}</p>
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

现在它们叠在一起了。

![](img/960dbf7538d907a8c03a3eb1a78093ab.png)

马丁·莫雷诺在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

我们可以单独或作为一个组来创建单选按钮。

只要每个的`name`值相同，我们就可以将其绑定到同一个状态字段。

还有许多其他带有标签和文本的定制选项。