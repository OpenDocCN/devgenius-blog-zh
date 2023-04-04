# 用于添加代码编辑器和时间选择器的顶级 Vue 包

> 原文：<https://blog.devgenius.io/top-vue-packages-for-adding-code-editors-and-time-pickers-676cb466d776?source=collection_archive---------27----------------------->

![](img/3954d9390361add0d3ae56a2cbcce53d.png)

照片由[内森·格林](https://unsplash.com/@npglynn?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将看看添加代码编辑器和时间选择器的最佳包。

# Vue-Codemirror-Lite

Vue-Codemirror-Lite 是 Vue 应用程序的代码查看器。

它有语法

为了使用它，我们运行:

```
npm i vue-codemirror-lite
```

来安装它。

然后我们可以通过写来使用它:

```
<template>
  <codemirror :value="code"></codemirror>
</template>

<script>
import { codemirror } from "vue-codemirror-lite";export default {
  components: {
    codemirror
  },
  data() {
    return {
      code: 'const str = "hello world"'
    };
  }
};
</script>
```

我们只是使用`codemirror`组件来显示代码。

此外，我们可以分配一个 ref 并调用它的方法:

```
<template>
  <codemirror ref="codeEditor" :value="code"></codemirror>
</template>

<script>
import { codemirror } from "vue-codemirror-lite";export default {
  components: {
    codemirror
  },
  data() {
    return {
      code: 'const str = "hello world"'
    };
  },
  computed: {
    editor() {
      return this.$refs.codeEditor.editor;
    }
  },
  mounted() {
    this.editor.focus();
  }
};
</script>
```

# Vue 棱镜编辑器

Vue Prism Editor 是 Vue 应用程序的另一个代码编辑器。

为了使用它，我们运行:

```
npm i vue-prism-editor prism
```

来安装它。

Prism 是必需的依赖项。

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import "prismjs";
import "prismjs/themes/prism-tomorrow.css";
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <prism-editor class="my-editor" :code="code" language="js"></prism-editor>
</template>

<script>
import PrismEditor from "vue-prism-editor";
export default {
  components: {
    PrismEditor
  },
  data() {
    return {
      code: `const a = 'foo'`
    };
  }
};
</script><style>
.my-editor {
  height: 300px;
}
</style>
```

我们使用`prism-editor`组件。

为了显示它，我们设置编辑器的高度。

这个`code`道具有密码。

`language`设置为`js`表示我们显示 JavaScript。

我们可以用`linenumbers`道具显示行号:

```
<template>
  <prism-editor lineNumbers class="my-editor" :code="code" language="js"></prism-editor>
</template>

<script>
import PrismEditor from "vue-prism-editor";
export default {
  components: {
    PrismEditor
  },
  data() {
    return {
      code: `const a = 'foo'`
    };
  }
};
</script><style>
.my-editor {
  height: 300px;
}
</style>
```

# v-jsoneditor

v-jsoneditor 是一个为 Vue 应用程序设计的 JSON 编辑器。

为了使用它，我们运行:

```
npm i v-jsoneditor
```

来安装它。

然后我们可以通过写来使用它:

```
<template>
  <div>
    <v-jsoneditor v-model="json" :plus="false" height="400px" @error="onError"></v-jsoneditor>
  </div>
</template>

<script>
import VJsoneditor from "v-jsoneditor";export default {
  name: "app",
  components: {
    VJsoneditor
  },
  data() {
    return {
      json: {
        hello: "world"
      }
    };
  },
  methods: {
    onError() {
      console.log("error");
    }
  }
};
</script>
```

我们使用`v-jsoneditor`组件来添加它。

`v-model`将 JSON 值绑定到编辑器。

发出了`error`事件，我们可以用`onError`方法处理它。

我们可以用`options`道具设置一些选项。

为了使用它，我们写:

```
<template>
  <div>
    <v-jsoneditor v-model="json" :options="options" :plus="false" height="400px" @error="onError"></v-jsoneditor>
  </div>
</template>

<script>
import VJsoneditor from "v-jsoneditor";export default {
  name: "app",
  components: {
    VJsoneditor
  },
  data() {
    return {
      json: {
        hello: "world"
      },
      options: {
        templates: [
          {
            text: "Person",
            title: "Insert a Person Node",
            className: "jsoneditor-type-object",
            field: "PersonTemplate",
            value: {
              firstName: "james",
              lastName: "smith"
            }
          }
        ]
      }
    };
  },
  methods: {
    onError() {
      console.log("error");
    }
  }
};
</script>
```

这允许我们添加预置节点，因为我们添加了带有这些字段的`templates`数组。

`plus`让我们切换到全屏。

# Vue2 时间选择器

Vue2 时间选择器是一个方便的时间选择器。

为了使用它，我们运行:

```
npm i vue2-timepicker
```

来安装它。

然后我们可以写:

```
<template>
  <div>
    <vue-timepicker></vue-timepicker>
  </div>
</template>

<script>
import VueTimepicker from "vue2-timepicker";
import "vue2-timepicker/dist/VueTimepicker.css";export default {
  name: "app",
  components: {
    VueTimepicker
  }
};
</script>
```

使用时间选择器。

我们只是添加了`vue-timerpicker`组件。

可以设置格式，我们可以将选定的值绑定到一个状态:

```
<template>
  <div>
    <vue-timepicker v-model="time" format="HH:mm:ss"></vue-timepicker>
    <p>{{time}}</p>
  </div>
</template>

<script>
import VueTimepicker from "vue2-timepicker";
import "vue2-timepicker/dist/VueTimepicker.css";export default {
  name: "app",
  components: {
    VueTimepicker
  },
  data() {
    return {
      time: undefined
    };
  }
};
</script>
```

我们使用`v-model`进行绑定，使用`format`设置时间格式。

![](img/fa4a741c981e9908683bf16e826abc78.png)

马丁·莫雷诺在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

Vue-Codemirror-Lite 和 Vue Prism Editor 是 Vue 应用程序的代码编辑器。

v-jsoneditor 是一个针对 Vue 应用的 JSON 编辑器。

Vue2 Timepicker 是一个时间选择器。