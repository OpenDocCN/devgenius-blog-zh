# BootstrapVue —表定制

> 原文：<https://blog.devgenius.io/bootstrapvue-table-customizations-c351b927e239?source=collection_archive---------13----------------------->

![](img/4c44a673047ee0751d1530e0efbc8f25.png)

照片由[南安](https://unsplash.com/@bepnamanh?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

为了制作好看的 Vue 应用，我们需要设计组件的样式。

为了使我们的生活更容易，我们可以使用内置样式的组件。

我们来看看如何定制表格内容。

# 列组

我们可以将表格列分组到列组中。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-table :items="items">
      <template v-slot:table-colgroup="scope">
        <col
          v-for="field in scope.fields"
          :key="field.key"
          :style="{ width: field.key === 'firstName' ? '120px' : '180px' }"
        >
      </template>
    </b-table>
  </div>
</template><script>
export default {
  data() {
    return {
      items: [
        { firstName: "alex", lastName: "green" },
        {
          firstName: "may",
          lastName: "smith"
        },
        { firstName: "james", lastName: "jones" }
      ]
    };
  }
};
</script>
```

我们有填充`table-colgroup`槽的`col`组件来遍历字段。

这样，我们可以用`style`道具来设计每一列的样式。

# 忙碌状态

`b-table`如果桌子忙，让我们传入`busy`道具来标记桌子。

这样，如果表很忙，我们可以显示一些内容。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-table :items="items" busy>
      <template v-slot:table-busy>
        <div>
          <b-spinner></b-spinner>
          <strong>Loading...</strong>
        </div>
      </template>
    </b-table>
  </div>
</template><script>
export default {
  data() {
    return {
      items: [
        { firstName: "alex", lastName: "green" },
        {
          firstName: "may",
          lastName: "smith"
        },
        { firstName: "james", lastName: "jones" }
      ]
    };
  }
};
</script>
```

我们在`b-table`上安装了`busy`道具，并用一个微调器和“加载”文本填充了`table-busy` slow。

因此，我们将看到槽中的内容，而不是表的内容。

# 自定义数据呈现

我们可以用自定义的方式呈现数据。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-table :items="items">
      <template v-slot:cell(firstName)="data">{{ data.value }}</template><template v-slot:cell(lastName)="data">
        <b>{{ data.value }}</b>
      </template>
    </b-table>
  </div>
</template><script>
export default {
  data() {
    return {
      items: [
        { firstName: "alex", lastName: "green" },
        {
          firstName: "may",
          lastName: "smith"
        },
        { firstName: "james", lastName: "jones" }
      ]
    };
  }
};
</script>
```

我们填充列的槽。

使用`data`可以获得该列的数据。

`v-slot:cell(firstName)=”data”`获取`firstName`属性并将其分配给`data`。

`v-slot:cell(lastName)=”data”`对`lastName`进行同样的操作。

而`value`属性有每个属性的值。

因此，我们可以按照自己的方式呈现每一段数据。

`data`上例中有许多属性。

`index`有行号。

`item`有每个条目的原始记录。

`value`是给定键的值。

`unformatted`在通过`formatter`函数之前有原始值。

`field`有规范化的字段定义对象。

如果`row-details`作用域插槽可见，则`detailsShowing`为`true`。

可以调用`toggleDetails`来切换`row-details`范围槽的可见性。

如果行被选中，则`rowSelected`为`true`。

`selectRow`调用时选择当前行。

`unselectRow`调用时取消选择当前行。

# 显示原始 HTML

我们可以显示原始的 HTML。

要做到这一点，我们只需用一个有`v-html`指令的元素填充插槽。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-table :items="items">
      <template v-slot:cell(lastName)="data">
        <span v-html="data.value"></span>
      </template>
    </b-table>
  </div>
</template><script>
export default {
  data() {
    return {
      items: [{ firstName: "alex", lastName: "<b>green</b>" }]
    };
  }
};
</script>
```

我们有一个 span，它将`v-html`指令设置为`data.value`，其中包含了我们的`lastName`值。

因此 HTML 代码将在没有净化的情况下呈现。

但是，我们必须小心，以免受到跨站点脚本攻击。

# 格式化程序回调

我们可以添加一个格式化的 to out `fields`数组条目来格式化我们的单元格。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-table :items="items" :fields="fields"></b-table>
  </div>
</template>
<script>
export default {
  data() {
    return {
      fields: [
        {
          key: "name",
          label: "Full Name",
          formatter: "fullName"
        }
      ],
      items: [
        { firstName: "alex", lastName: "green" },
        {
          firstName: "may",
          lastName: "smith"
        },
        { firstName: "james", lastName: "jones" }
      ]
    };
  },
  methods: {
    fullName(value, key, item) {
      return `${item.firstName} ${item.lastName}`;
    }
  }
};
</script>
```

我们有`fullName`方法。

它接受`item`参数，该参数包含`items`中的对象，并返回组合在一起的`firstName`和`lastName`属性。

它的名称被设置为我们字段的数组条目中的值`formatter`。

因此，现在我们有了一个全名列，同时显示了两个字段的值。

我们也可以写:

```
<template>
  <div id="app">
    <b-table :items="items" :fields="fields"></b-table>
  </div>
</template>
<script>
export default {
  data() {
    return {
      fields: [
        {
          key: "name",
          label: "Full Name",
          formatter(value, key, item) {
            return `${item.firstName} ${item.lastName}`;
          }
        }
      ],
      items: [
        { firstName: "alex", lastName: "green" },
        {
          firstName: "may",
          lastName: "smith"
        },
        { firstName: "james", lastName: "jones" }
      ]
    };
  }
};
</script>
```

我们将格式化的函数移到了`fields`入口对象中，而不是放在`methods`中。

![](img/9778dae560d5decdf9383ed3a3e495ae.png)

[Marylou Fortier](https://unsplash.com/@rylouma?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以用格式化的函数来格式化表格单元格。

此外，我们可以用 slots 或 raw HTML 定制单元格布局和格式。

我们可以将 busy 状态设置为在加载表数据时显示一些内容。