# Buefy —数字滑块

> 原文：<https://blog.devgenius.io/buefy-numeric-slider-1db7039302d2?source=collection_archive---------4----------------------->

![](img/c5d1f01470b4fd11193b320a0cc06430.png)

[拉利斯 T](https://unsplash.com/@im_laleeth?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

Buefy 是一个基于布尔玛的 UI 框架。

在本文中，我们将了解如何在我们的 Vue 应用程序中使用 Buefy。

# 数字滑块

我们可以添加一个滑块，用`b-slider`组件设置一个数值。

例如，我们可以写:

```
<template>
  <section>
    <b-field label="number">
      <b-slider v-model="value"></b-slider>
    </b-field>
  </section>
</template><script>
export default {
  data() {
    return {
      value: 5
    };
  }
};
</script>
```

我们添加`v-model`将值绑定到一个状态。

我们可以用`disabled`道具禁用它。

可以用`size`支柱改变尺寸:

```
<template>
  <section>
    <b-field label="number">
      <b-slider size="is-large" :value="20"></b-slider>
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

样式可以用`type`道具改变:

```
<template>
  <section>
    <b-field label="number">
      <b-slider type="is-success" :value="20"></b-slider>
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

我们可以使用`custom-formatter`道具定制标签:

```
<template>
  <section>
    <b-field label="number">
      <b-slider type="is-success" :custom-formatter="val => `${val}%`"></b-slider>
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

现在工具提示在数字后面显示了`%`符号，因为我们将`custom-formatter`属性设置为我们自己的函数。

# 勾选并标记

我们可以通过用`b-slider-tick`组件填充默认插槽来添加刻度:

```
<template>
  <section>
    <b-field label="Custom tick and label">
      <b-slider size="is-medium" :min="0" :max="10">
        <template v-for="val in [2,4,6,8]">
          <b-slider-tick :value="val" :key="val">{{ val }}</b-slider-tick>
        </template>
      </b-slider>
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

# 范围滑块

我们可以用`min`和`max`道具添加一个范围滑块来限制我们可以选择的范围。

那么我们绑定到`v-model`的值将是一个包含最小值和最大值的数组:

```
<template>
  <section>
    <b-field>
      <b-slider v-model="numbers" :min="0" :max="15" :step="0.5"></b-slider>
    </b-field>
  </section>
</template><script>
export default {
  data() {
    return {
      numbers: [1, 5]
    };
  }
};
</script>
```

# 延迟更新

我们可以添加`lazy`属性，以便仅在拖动完成时更新`v-model`值:

```
<template>
  <section>
    <b-field>
      <b-slider v-model="value" lazy></b-slider>
    </b-field>
    {{value}}
  </section>
</template><script>
export default {
  data() {
    return {
      value: 20
    };
  }
};
</script>
```

# 结论

Buefy 的`b-slider`组件是 Vue 的一个有用的滑块组件。