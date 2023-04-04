# Buefy —表单字段

> 原文：<https://blog.devgenius.io/buefy-form-field-79553e79936d?source=collection_archive---------1----------------------->

![](img/c266a1951fdb25684adb19a7a7263a1f.png)

照片由[布鲁诺·马丁斯](https://unsplash.com/@brunus?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

Buefy 是一个基于布尔玛的 UI 框架。

在本文中，我们将了解如何在我们的 Vue 应用程序中使用 Buefy。

# 表单字段

Buefy 提供了用作表单控件容器的`b-field`组件。

它应该是以下对象的直接父对象:

*   自动完成
*   复选框按钮
*   日期选择器
*   投入
*   单选按钮
*   挑选
*   标签输入
*   时间选择器
*   上传
*   `.control`元素(HTML 类)

我们可以用`label-position`和`label`道具添加一个标签:

```
<template>
  <b-field label="Name" :label-position="labelPosition">
    <b-input value="james"></b-input>
  </b-field>
</template>
<script>
export default {
  data() {
    return {
      labelPosition: "on-border"
    };
  }
};
</script>
```

`label-position`让我们改变标签位置。

`on-border`将把标签放在标签的上边框。

我们可以设置一个字段的`type`和`message`来显示不同的内容。

例如，我们可以写:

```
<template>
  <b-field
    label="Username"
    :type="{ 'is-danger': hasError }"
    :message="{ 'name is taken': hasError }"
  >
    <b-input value="james" maxlength="30"></b-input>
  </b-field>
</template>
<script>
export default {
  data() {
    return {
      hasError: true
    };
  }
};
</script>
```

`type`决定风格。

`'is-danger'`是风格名称。

`message`将消息作为键，并有条件将其显示为值。

它将显示在字段下方。

# 表单域插件

我们可以在字段的左边或右边添加表单字段插件。

例如，我们可以写:

```
<template>
  <b-field message="search">
    <b-input placeholder="Search..." type="search"></b-input>
    <p class="control">
      <button class="button is-primary">Search</button>
    </p>
  </b-field>
</template>
<script>
export default {
  data() {
    return {};
  }
};
</script>
```

我们用 p 元素和`control`类将插件添加到输入框的右边。

# 组

我们可以用`grouped`道具将组件组合在一起。

例如，我们可以写:

```
<template>
  <b-field grouped message="search">
    <b-input placeholder="Search..."></b-input>
    <p class="control">
      <button class="button is-primary">Search</button>
    </p>
  </b-field>
</template>
<script>
export default {
  data() {
    return {};
  }
};
</script>
```

将按钮与输入框分组。

# 嵌套组

组可以嵌套。

例如，我们可以写:

```
<template>
  <b-field grouped>
    <b-field label="Name" expanded>
      <b-input></b-input>
    </b-field>
    <b-field label="Email" expanded>
      <b-input></b-input>
    </b-field>
  </b-field>
</template>
<script>
export default {
  data() {
    return {};
  }
};
</script>
```

然后输入框并排显示。

# 位置

对准可以用`position`支柱改变:

```
<template>
  <b-field position="is-centered">
    <b-input placeholder="Search..." type="search"></b-input>
    <p class="control">
      <button class="button is-info">Search</button>
    </p>
  </b-field>
</template>
<script>
export default {
  data() {
    return {};
  }
};
</script>
```

我们将`position`设置为`'is-centered'`以使组居中。

# 组合插件和组

我们可以组合插件和组:

```
<template>
  <b-field grouped>
    <b-field label="Name" expanded>
      <b-field>
        <b-input placeholder="Name" expanded></b-input>
      </b-field>
    </b-field>
    <b-field label="Email" expanded>
      <b-input placeholder="abc@email.com"></b-input>
    </b-field>
  </b-field>
</template>
<script>
export default {
  data() {
    return {};
  }
};
</script>
```

# 标签

我们可以填充`label`槽，用我们自己的样式填充标签:

```
<template>
  <b-field>
    <template slot="label">
      <span class="is-italic">custom label</span>
    </template>
    <b-input></b-input>
  </b-field>
</template>
<script>
export default {
  data() {
    return {};
  }
};
</script>
```

# 结论

我们可以用 Buefy `b-field`组件对表单控件进行分组。