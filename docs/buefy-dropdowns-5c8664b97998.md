# buefy-drop downs

> 原文：<https://blog.devgenius.io/buefy-dropdowns-5c8664b97998?source=collection_archive---------3----------------------->

![](img/b2da99d85f1f10837aa799f5a9b68b02.png)

[张凯夫](https://unsplash.com/@zhangkaiyv?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Buefy 是一个基于布尔玛的 UI 框架。

在本文中，我们将了解如何在我们的 Vue 应用程序中使用 Buefy。

# 选择下拉菜单

我们可以用`b-select`组件添加一个选择下拉菜单。

例如，我们可以写:

```
<template>
  <section>
    <b-field label="Simple">
      <b-select placeholder="Select a fruit">
        <option v-for="option in data" :value="option.id" :key="option.id">{{ option.name }}</option>
      </b-select>
    </b-field>
  </section>
</template><script>
export default {
  data() {
    return {
      data: [
        { id: 1, name: "apple" },
        { id: 2, name: "orange" },
        { id: 3, name: "grape" }
      ]
    };
  }
};
</script>
```

来补充一下。

我们可以使用`v-model`将选择的值绑定到一个状态:

```
<template>
  <section>
    <b-field label="Simple">
      <b-select placeholder="Select a fruit" v-model="val">
        <option v-for="option in data" :value="option.id" :key="option.id">{{ option.name }}</option>
      </b-select>
      {{val}}
    </b-field>
  </section>
</template><script>
export default {
  data() {
    return {
      val: 0,
      data: [
        { id: 1, name: "apple" },
        { id: 2, name: "orange" },
        { id: 3, name: "grape" }
      ]
    };
  }
};
</script>
```

此外，我们可以设置`type`和`message`道具来设置样式和添加消息:

```
<template>
  <section>
    <b-field label="Simple" type="is-danger" message="error">
      <b-select placeholder="Select a fruit" v-model="val">
        <option v-for="option in data" :value="option.id" :key="option.id">{{ option.name }}</option>
      </b-select>
    </b-field>
  </section>
</template><script>
export default {
  data() {
    return {
      val: 0,
      data: [
        { id: 1, name: "apple" },
        { id: 2, name: "orange" },
        { id: 3, name: "grape" }
      ]
    };
  }
};
</script>
```

`type`为类型，下拉菜单下方显示`message`。

`loading`支柱显示装载指示器。

并且`disabled` prop 禁用下拉菜单。

我们将它们都添加到`b-select`组件中。

# 多次选择

此外，我们可以使用`multiple`属性启用多重选择:

```
<template>
  <section>
    <b-field>
      <b-select multiple placeholder="Select fruits" v-model="fruits">
        <option v-for="option in data" :value="option.id" :key="option.id">{{ option.name }}</option>
      </b-select>
    </b-field>
    {{fruits}}
  </section>
</template><script>
export default {
  data() {
    return {
      fruits: 0,
      data: [
        { id: 1, name: "apple" },
        { id: 2, name: "orange" },
        { id: 3, name: "grape" }
      ]
    };
  }
};
</script>
```

我们在一个数组中看到选定的值。

# 核标准情报中心

我们可以在下拉列表的左侧添加一个图标，方法是:

```
<template>
  <section>
    <b-field>
      <b-select placeholder="Country" icon="address-book" icon-pack="fa">
        <option value="1">Option 1</option>
        <option value="2">Option 2</option>
      </b-select>
    </b-field>
  </section>
</template><script>
export default {
  data() {
    return {};
  }
};
</script>
```

`icon-pack`道具设置要使用的图标库。

`fa`代表字体牛逼。

`icon`具有我们想要添加的库中图标的名称。

要添加字体 Awesome 4.7.0，我们添加:

```
<link
      rel="stylesheet"
      href="https://stackpath.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css"
      integrity="sha384-wvfXpqpZZVQGK6TAh5PVlGOfQNHSoD2xbE+QkPxCAFlNEevoEH3Sl0sibVcOQVnN"
      crossorigin="anonymous"
    />
```

在`public/index.html`的`head`标签中。

# 大小

我们可以用`size`道具改变尺寸:

```
<template>
  <section>
    <b-field>
      <b-select placeholder="Country" size="is-large">
        <option value="1">Option 1</option>
        <option value="2">Option 2</option>
      </b-select>
    </b-field>
  </section>
</template><script>
export default {
  data() {
    return {};
  }
};
</script>
```

# 结论

我们将 Buefy 下拉菜单添加到我们的 Vue 应用程序中。