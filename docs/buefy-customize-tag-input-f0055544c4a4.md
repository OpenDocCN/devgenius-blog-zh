# Buefy —自定义标签输入

> 原文：<https://blog.devgenius.io/buefy-customize-tag-input-f0055544c4a4?source=collection_archive---------4----------------------->

![](img/9d327b7369d47a97780bf8e2712fa57e.png)

照片由 [Daria Shevtsova](https://unsplash.com/@daria_shevtsova?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Buefy 是一个基于布尔玛的 UI 框架。

在本文中，我们将了解如何在我们的 Vue 应用程序中使用 Buefy。

# 自定义标签输入

我们可以用各种方式定制我们的标签输入。

我们可以限制可以添加的标签数量以及每个标签可以包含的字符数量。

属性限制了标签中的字符数。

`maxtags`道具限制了我们可以输入的标签数量。

例如，我们可以写:

```
<template>
  <section>
    <div class="field">
      <b-taginput v-model="tags" maxlength="10" placeholder="Add a tag"></b-taginput>
      {{tags}}
    </div>
  </section>
</template><script>
export default {
  data() {
    return { tags: [] };
  }
};
</script>
```

将每个标签的字符数限制为最多 10 个。

我们可以写:

```
<template>
  <section>
    <div class="field">
      <b-taginput v-model="tags" maxtags="5" placeholder="Add a tag"></b-taginput>
      {{tags}}
    </div>
  </section>
</template><script>
export default {
  data() {
    return { tags: [] };
  }
};
</script>
```

将标签的最大数量限制为 5。

我们可以用`b-field`上的`type`道具改变轮廓颜色:

```
<template>
  <section>
    <b-field type="is-success">
      <b-taginput v-model="tags" placeholder="Add a tag"></b-taginput>
      {{tags}}
    </b-field>
  </section>
</template><script>
export default {
  data() {
    return { tags: [] };
  }
};
</script>
```

现在轮廓是绿色的。

可以将`type`道具添加到`b-taginput`来改变标签的背景颜色:

```
<template>
  <section>
    <b-field>
      <b-taginput type="is-dark" v-model="tags" placeholder="Add a tag"></b-taginput>
      {{tags}}
    </b-field>
  </section>
</template><script>
export default {
  data() {
    return { tags: [] };
  }
};
</script>
```

`size`道具改变标签输入的大小:

```
<template>
  <section>
    <b-field>
      <b-taginput size="is-large" v-model="tags" placeholder="Add a tag"></b-taginput>
      {{tags}}
    </b-field>
  </section>
</template><script>
export default {
  data() {
    return { tags: [] };
  }
};
</script>
```

`rounded`道具使标签输入变圆:

```
<template>
  <section>
    <b-field>
      <b-taginput rounded v-model="tags" placeholder="Add a tag"></b-taginput>
      {{tags}}
    </b-field>
  </section>
</template><script>
export default {
  data() {
    return { tags: [] };
  }
};
</script>
```

`attched`道具使标签呈矩形:

```
<template>
  <section>
    <b-field>
      <b-taginput attached v-model="tags" placeholder="Add a tag"></b-taginput>
      {{tags}}
    </b-field>
  </section>
</template><script>
export default {
  data() {
    return { tags: [] };
  }
};
</script>
```

我们可以用`before-adding`道具添加验证:

```
<template>
  <section>
    <b-field>
      <b-taginput :before-adding="beforeAdding" v-model="tags" placeholder="Add a tag"></b-taginput>
      {{tags}}
    </b-field>
  </section>
</template><script>
export default {
  data() {
    return { tags: [] };
  },
  methods: {
    beforeAdding(tag) {
      return tag.length === 3;
    }
  }
};
</script>
```

在添加标签之前将运行`beforeAdding`方法。

标签只有在返回`true`时才会被添加。

`tag`具有输入的值。

# 结论

我们可以用 Buefy 的标签输入来定制标签输入。