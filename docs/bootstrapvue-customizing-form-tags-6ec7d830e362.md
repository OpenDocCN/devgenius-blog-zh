# BootstrapVue —自定义表单标签

> 原文：<https://blog.devgenius.io/bootstrapvue-customizing-form-tags-6ec7d830e362?source=collection_archive---------9----------------------->

![](img/627105c968f1bc22c049b7e0a8dd5420.png)

照片由[迈克·本纳](https://unsplash.com/@mbenna?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

为了制作好看的 Vue 应用，我们需要设计组件的样式。

为了使我们的生活更容易，我们可以使用内置样式的组件。

在本文中，我们将研究如何定制表单标签。

# 标签的自定义呈现

我们可以自定义标签的呈现。

为此，我们使用插槽。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-tags v-model="value" no-outer-focus class="mb-2">
      <template v-slot="{ tags, inputAttrs, inputHandlers, addTag, removeTag }">
        <b-input-group aria-controls="my-custom-tags-list">
          <input
            v-bind="inputAttrs"
            v-on="inputHandlers"
            placeholder="add tag"
            class="form-control"
          >
          <b-input-group-append>
            <b-button [@click](http://twitter.com/click)="addTag()" variant="primary">Add</b-button>
          </b-input-group-append>
        </b-input-group>
        <ul>
          <b-card v-for="tag in tags" :key="tag" tag="li">
            <strong>{{ tag }}</strong>
            <b-button [@click](http://twitter.com/click)="removeTag(tag)" variant="link" size="sm">remove</b-button>
          </b-card>
        </ul>
      </template>
    </b-form-tags>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: ["apple"]
    };
  }
};
</script>
```

我们有一个输入框。

该插槽有`addTag`和`removeTag`方法来添加和移除标签。

`inputAttrs`让我们将输入标签绑定到`inputAttrs`。

`inputHandlers`是运行时我们输入的一个标签。

`addTag`是在我们点击添加时运行的。

`removeTag`是在我们点击标签上的删除按钮时运行的。

我们有一个`b-card`组件，它显示为一个`li`元素来显示输入的标签。

# 自定义表单组件

我们还可以使用`b-form-tag`组件在输入之外显示我们的标签。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-tags v-model="value" no-outer-focus class="mb-2">
      <template v-slot="{ tags, inputAttrs, inputHandlers, addTag, removeTag }">
        <b-input-group aria-controls="my-custom-tags-list">
          <input
            v-bind="inputAttrs"
            v-on="inputHandlers"
            placeholder="add tag"
            class="form-control"
          >
          <b-input-group-append>
            <b-button @click="addTag()" variant="primary">Add</b-button>
          </b-input-group-append>
        </b-input-group>
        <b-form-tag v-for="tag in tags" [@remove](http://twitter.com/remove)="removeTag(tag)" :key="tag" :title="tag">{{ tag }}</b-form-tag>
      </template>
    </b-form-tags>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: ["apple"]
    };
  }
};
</script>
```

我们有`b-form-tag`组件来显示输入元素之外的项目。

此外，我们可以将`b-form-tags`组件显示为一个选择框。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-tags v-model="value" add-on-change>
      <template v-slot="{ tags, inputAttrs, inputHandlers, disabled, removeTag }">
        <ul v-if="tags.length > 0" class="list-inline d-inline-block mb-2">
          <li v-for="tag in tags" :key="tag">
            <b-form-tag
              [@remove](http://twitter.com/remove)="removeTag(tag)"
              :title="tag"
              :disabled="disabled"
              variant="info"
            >{{ tag }}</b-form-tag>
          </li>
        </ul>
        <b-form-select
          v-bind="inputAttrs"
          v-on="inputHandlers"
          :disabled="availableOptions.length === 0"
          :options="availableOptions"
        >
          <template v-slot:first>
            <option disabled value>Choose one...</option>
          </template>
        </b-form-select>
      </template>
    </b-form-tags>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      options: ["apple", "orange", "grape"],
      value: ["apple"]
    };
  },
  computed: {
    availableOptions() {
      return this.options.filter(opt => !this.value.includes(opt));
    }
  }
};
</script>
```

我们有`b-form-tags`组件，其中嵌套了一个`template`让我们以自己喜欢的方式显示标签。

像前面的例子一样，我们使用插槽中的变量和方法。

此外，我们有`b-form-select`组件来显示可以选择的项目。

我们有“选择一个……”占位符。

在`b-form-select`组件中。

当`availableOptions`没有选项时，我们有`disabled`属性来禁用选择元素。

`availableOptions`是从`options`数组中计算出来的计算属性。

我们过滤掉那些已经被选中的。

如果从下拉列表中选择了一个项目，那么`b-form-tags`组件上的`add-on-change` prop 将允许我们添加标签。

我们还可以在输入模板中添加任何发出`input`和`change`事件的定制输入组件。

例如，我们可以写:

```
<template v-slot:default="{ inputAttrs, inputHandlers, removeTag, tags }">
  <custom-input
    :id="inputAttrs.id"
    :vistom-value-prop="inputAttrs.value"
    @custom-input-event="inputHandlers.input($event)"
    @custom-change-event="inputHandlers.change($event)"
    @keydown.native="inputHandlers.keydown($event)"
  ></custom-input>
  <template v-for="tag in tags">
    <!-- Your custom tag list here -->
  </template>
</template>
```

![](img/288ba45bde28677c4e2b4c88e28417d6.png)

由[维克多·马尔尤舍夫](https://unsplash.com/@malyushev?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以用多种方式定制表单标签输入。

我们可以改变输入元素。

此外，我们可以改变输入标签的显示方式。

该槽提供了添加或删除标签的方法。