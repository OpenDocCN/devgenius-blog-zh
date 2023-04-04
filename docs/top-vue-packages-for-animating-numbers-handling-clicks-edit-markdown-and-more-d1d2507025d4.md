# 用于制作数字动画、处理点击、编辑降价等的顶级 Vue 包

> 原文：<https://blog.devgenius.io/top-vue-packages-for-animating-numbers-handling-clicks-edit-markdown-and-more-d1d2507025d4?source=collection_archive---------30----------------------->

![](img/4198e67fb022bb6dc15dbccd2cae5a8e.png)

照片由[梅丽莎·艾斯丘](https://unsplash.com/@melissaaskew?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在这篇文章中，我们将看看最好的软件包，用于制作数字动画、处理元素外的点击、添加 Markdown 编辑器和可定制的下拉菜单。

# vue-计数-v2

vue-countup-v2 允许我们添加一个数字动画显示。

要使用它，我们通过运行以下命令来安装它:

```
npm install --save countup.js vue-countup-v2
```

我们必须添加`countup.js`来使用 vue-countup-v2。

然后我们可以通过写来使用它:

```
<template>
  <div id="app">
    <ICountUp :delay="delay" :endVal="endVal" :options="options" @ready="onReady"/>
  </div>
</template><script>
import ICountUp from "vue-countup-v2";
export default {
  components: {
    ICountUp
  },
  data() {
    return {
      delay: 1000,
      endVal: 45373,
      options: {
        useEasing: true,
        useGrouping: true,
        separator: ",",
        decimal: ".",
        prefix: "",
        suffix: ""
      }
    };
  },
  methods: {
    onReady(instance, CountUp) {
      const that = this;
      instance.update(that.endVal + 100);
    }
  }
};
</script>
```

我们可以设置很多选项。

我们设置`endVal`来设置动画的结束值。

`delay`是动画的延迟。

`useEasing`让我们以非线性方式制作动画。

`useGrouping`分组数字。

`decimal`是十进制数字分隔符。

`prefix`和`suffix`是加在数字上的前缀和后缀。

# vue-简单的

Vue-SimpleMDE 是一个简单的 Vue 应用程序降价编辑器组件。

要使用它，我们首先通过运行以下命令来安装它:

```
npm i vue-simplemde
```

然后我们可以通过写来使用它:

```
<template>
  <vue-simplemde v-model="content" ref="markdownEditor"/>
</template>

<script>
import VueSimplemde from "vue-simplemde";
import "simplemde/dist/simplemde.min.css";export default {
  components: {
    VueSimplemde
  },
  data() {
    return {
      content: ""
    };
  }
};
</script>
```

我们只是将`vue-simplemde`组件添加到我们的应用程序中。

此外，我们为包导入了 CSS。

输入值通过`v-model`绑定到`content`。

# vue-搜索-选择

vue-search-select 为我们提供了一个可定制的下拉列表。

为了使用它，我们运行:

```
npm i vue-search-select
```

来安装它。

然后我们可以通过写来使用它:

```
<template>
  <model-select :options="options" v-model="item" placeholder="select item"></model-select>
</template>

<script>
import { ModelSelect } from "vue-search-select";
import "vue-search-select/dist/VueSearchSelect.css";export default {
  data() {
    return {
      options: [
        { value: "1", text: "foo" },
        { value: "2", text: "bar" },
        { value: "3", text: "baz" }
      ],
      item: {
        value: "",
        text: ""
      }
    };
  },
  components: {
    ModelSelect
  }
};
</script>
```

我们使用`model-select`组件。

`options`道具设置下拉选项。

`v-model`将项目绑定到`item`。

# 电话输入

vue-tel-input 允许我们在 vue 应用程序中添加电话号码。

要使用它，我们通过运行以下命令来安装它:

```
npm i vue-tel-input
```

然后我们可以通过写来使用它:

```
<template>
  <div>
    <vue-tel-input v-model="phone"></vue-tel-input>
  </div>
</template>

<script>
export default {
  data() {
    return {
      phone: ""
    };
  }
};
</script>
```

我们看到一个下拉列表，其中列出了电话号码的国家前缀。

我们可以在`vue-tel-input`组件中输入我们想要的内容。

`v-model`让我们将输入值绑定到`phone`状态。

# vue-点击-外部

vue-on-click-outside 是一个 vue 指令，它允许我们在元素外部触发单击时调用一个方法。

要安装它，我们运行:

```
npm i vue-on-click-outside
```

然后我们可以通过写来使用它:

```
<template>
  <div>
    <button type="button" @click="open">open</button>
    <div v-if="showPopover" v-on-click-outside="close">
      <span>hello</span>
    </div>
  </div>
</template>

<script>
import { mixin as onClickOutside } from "vue-on-click-outside";
export default {
  mixins: [onClickOutside],
  data() {
    return { showPopover: false };
  },
  methods: {
    open() {
      this.showPopover = true;
    },
    close() {
      this.showPopover = false;
    }
  }
};
</script>
```

我们有一个打开弹出窗口的按钮。

当我们在弹出窗口外单击时，弹出窗口有关闭它的`v-on-click-outside`指令。

关闭是通过调用`close`方法完成的。

![](img/0ee05995af52e671c5e392ea8daaa68f.png)

照片由[伯纳德·赫曼特](https://unsplash.com/@bernardhermant?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

vue-countup-v2 让我们将数字动画化。

Vue-SimpleMDE 允许我们在我们的 Vue 应用程序中添加一个 Markdown 编辑器。

vue-tel-input 是一个有用的电话输入。

vue-on-click-outside 允许我们处理元素外部的点击。