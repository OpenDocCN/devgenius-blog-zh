# 使用 PrimeVue 框架开发 vue 3—定制按钮

> 原文：<https://blog.devgenius.io/vue-3-development-with-the-primevue-framework-customized-buttons-b7e7b7c73e0a?source=collection_archive---------5----------------------->

![](img/388a39a6a03d7d6e258f0f5dbf3417a1.png)

[船员](https://unsplash.com/@crew?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

PrimeVue 是一个与 Vue 3 兼容的 UI 框架。

在本文中，我们将了解如何开始使用 PrimeVue 开发 Vue 3 应用程序。

# 按钮事件

我们可以监听像点击按钮这样的事件。

例如，我们可以写:

```
<template>
  <div>
    <Button label="alert" @click="onClick" />
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {};
  },
  methods: {
    onClick($event) {
      alert(JSON.stringify($event));
    },
  },
};
</script>
```

用`@click`指令监听点击事件。

# 按钮样式

我们可以通过添加以下类之一来添加具有预设样式的按钮:

*   。p 按钮-辅助
*   。按钮-成功
*   。按钮信息
*   。p 按钮-警告
*   。按钮-帮助
*   。按钮-危险

例如，我们可以写:

```
<template>
  <div>
    <Button label="Secondary" class="p-button-secondary" />
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {};
  },
  methods: {},
};
</script>
```

添加样式。

# 文本按钮

我们可以通过添加`p-button-text`类来添加文本按钮:

```
<template>
  <div>
    <Button label="Submit" class="p-button-text" />
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {};
  },
  methods: {},
};
</script>
```

该按钮没有背景色，文本的颜色与普通按钮的背景色相同。

# 轮廓按钮

我们可以用`p-button-outlined`类添加一个轮廓按钮:

```
<template>
  <div>
    <Button label="Primary" class="p-button-outlined" />
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {};
  },
  methods: {},
};
</script>
```

# 链接按钮

`p-button-link`类使按钮成为一个链接:

```
<template>
  <div>
    <Button label="Link" class="p-button-link" />
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {};
  },
  methods: {},
};
</script>
```

# 带徽章的纽扣

我们可以添加带有徽章和一些道具的按钮:

```
<template>
  <div>
    <Button
      type="button"
      label="Messages"
      icon="pi pi-users"
      class="p-button-warning"
      badge="8"
      badgeClass="p-badge-info"
    />
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {};
  },
  methods: {},
};
</script>
```

`badge`有物为徽。

`icon`有图标类。

`badgeClass`有适用于徽章的职业。

# 按钮组

我们可以用`p-buttonset`类添加一个按钮集:

```
<template>
  <div>
    <span class="p-buttonset">
      <Button label="Save" icon="pi pi-check" />
      <Button label="Delete" icon="pi pi-trash" />
      <Button label="Cancel" icon="pi pi-times" />
    </span>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {};
  },
  methods: {},
};
</script>
```

# 按钮尺寸

我们可以使用以下类之一来更改按钮大小:

*   `p-button-sm`
*   `p-button`
*   `p-button-lg`

`sm`小`lg`大。没有后缀是正常尺寸。

例如，我们可以通过编写

```
<template>
  <div>
    <Button label="Large" icon="pi pi-check" class="p-button-lg" />
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {};
  },
  methods: {},
};
</script>
```

# 自定义按钮内容

我们可以将自定义按钮内容添加到默认槽中:

```
<template>
  <div>
    <Button> Custom Content </Button>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {};
  },
  methods: {},
};
</script>
```

# 结论

我们可以使用 PrimeVue 将各种风格的按钮添加到我们的 Vue 3 应用程序中。