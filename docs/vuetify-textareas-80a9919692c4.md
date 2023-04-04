# 如何用 Vuetify 创建 Textareas

> 原文：<https://blog.devgenius.io/vuetify-textareas-80a9919692c4?source=collection_archive---------19----------------------->

![](img/a417c404db9c63d670190dea935ed20c.png)

Vuetify 提供了 textarea 组件，非常类似于[文本字段，](https://codingbeautydev.com/blog/vuetify-text-fields/)但是更适合收集用户输入的大量文本。让我们在本文中探究一下这个组件的一些特性。

# v 文本区域组件

我们在 Vuetify 中用`v-text-area`组件创建 textareas。

```
<template>
  <v-app>
    <v-row class="ma-2" justify="center">
      <v-col cols="6">
        <v-textarea
          label="Default style"
          value="Learning about textareas at Coding Beauty!"
        >
        </v-textarea>
      </v-col>
    </v-row>
  </v-app>
</template><script>
export default {
  name: 'App',
};
</script>
```

![](img/bfaa99442d73b9ca59cc2c490f2c8acc.png)

# Vuetify 文本区 Solo 变体

要为文本区域使用可选的 solo 设计，我们可以将`solo` 道具设置为`true`:

```
<template>
  <v-app>
    <v-row class="ma-2" justify="center">
      <v-col cols="6">
        <v-textarea label="Solo textarea" solo> </v-textarea>
      </v-col>
    </v-row>
  </v-app>
</template><script>
export default {
  name: 'App',
};
</script>
```

![](img/a91d661d6a6bbbe3c44b6655cdbaa235.png)

独奏文本区。

![](img/1202e2a1837e2ed6e2a321d0d974bce5.png)

有焦点的单独文本区。

# 验证文本区域填充变体

类似于 Vuetify [文本字段](https://codingbeautydev.com/blog/vuetify-text-fields/)，我们可以使用`filled`属性来激活文本区域的填充变量。

```
<template>
  <v-app>
    <v-row class="ma-2" justify="center">
      <v-col cols="6">
        <v-textarea
          label="Filled textarea"
          value="Learning about textareas at Coding Beauty!"
          filled
        >
        </v-textarea>
      </v-col>
    </v-row>
  </v-app>
</template><script>
export default {
  name: 'App',
};
</script>
```

![](img/d7fd95fad37d6ae10564cf9e4c6aee56.png)

# Vuetify 文本区域轮廓变体

我们也有概述的变体。我们可以用`outlined`道具将一个文本区域改变成这种风格:

```
<template>
  <v-app>
    <v-row class="ma-2" justify="center">
      <v-col cols="6">
        <v-textarea
          label="Outlined textarea"
          value="Learning about textareas at Coding Beauty!"
          outlined
        >
        </v-textarea>
      </v-col>
    </v-row>
  </v-app>
</template><script>
export default {
  name: 'App',
};
</script>
```

![](img/a4d3ac2455e3c6fc93b8252272bbbcb4.png)

# 用美化来美化

使用 Vuetify 材料设计框架创建优雅 web 应用程序的完整指南。

![](img/ff271935eabc3e42d8f111285dca7821.png)

在这里免费下载[](https://mailchi.mp/583226ee0d7b/beautify-with-vuetify)****！****

# **使文本区域自动增长**

**使用`auto-grow`道具，我们可以让一个文本区域在文本超出其高度时自动增加大小。**

**![](img/69c699e97eb84514cddd19d84eed714c.png)**

**有四行的 Textarea。**

**![](img/76a53ad5426cda4b22f716eaefce614a.png)**

**Textarea 会扩展以容纳六行文本。**

# **自定义文本区域颜色**

**我们可以用`background-color`和`color`道具来设计文本区域[的颜色](https://codingbeautydev.com/blog/vuetify-colors/):**

```
<template>
  <v-app>
    <v-row class="ma-2" justify="center">
      <v-col cols="6">
        <v-textarea label="Label" background-color="gray lighten-2" color="indigo">
        </v-textarea>
      </v-col>
    </v-row>
  </v-app>
</template><script>
export default {
  name: 'App',
};
</script>
```

**![](img/abad45cb911a73ecc1a9eeaf87f40a7b.png)**

# **Vuetify 中可清除的文本区域**

**我们可以使用`clearable`道具清除文本区中的文本，并自定义与`clearable-icon`道具一起使用的图标:**

```
<template>
  <v-app>
    <v-row class="ma-2" justify="center">
      <v-col cols="6">
        <v-textarea
          clearable
          clear-icon="mdi-close"
          label="Text"
          value="You can clear all the text in this textarea."
        ></v-textarea>
      </v-col>
    </v-row>
  </v-app>
</template><script>
export default {
  name: 'App',
};
</script>
```

**![](img/4b80ed61ca3149b0114aee4672dbda54.png)**

**可清除的文本区域。**

**![](img/25ae19694e4d4d48d95a3e28a715d3d7.png)**

**单击图标会清除文本。**

# **验证文本区域字符限制**

**我们可以用`counter`属性通知用户在文本区域中输入的字符数。这对于实施字符限制很有用。**

```
<template>
  <v-app>
    <v-row class="ma-2" justify="center">
      <v-col cols="6">
        <v-textarea counter label="Text"></v-textarea>
      </v-col>
    </v-row>
  </v-app>
</template><script>
export default {
  name: 'App',
};
</script>
```

**![](img/38ebcd4ebd4075ae113e1ebfb659e64b.png)**

**包含 27 个字符的文本区。**

**![](img/7f2071d5436dbef85a086b09e1a718a0.png)**

**添加“编码美”会使 textarea 的字符数达到 42。**

# **验证文本区域图标**

**我们可以在文本区中包含一个图标来添加更多的上下文。在这里，我们使用`preprend-icon`道具在文本区域前包含图标:**

```
<template>
  <v-app>
    <v-row class="ma-2" justify="center">
      <v-col cols="6">
        <v-textarea
          prepend-icon="mdi-note"
          label="prepend-icon"
          value="Learning about textareas at Coding Beauty!"
        ></v-textarea>
      </v-col>
    </v-row>
  </v-app>
</template><script>
export default {
  name: 'App',
};
</script>
```

**![](img/a4c539f4a9347a237b0cf8d60a16808e.png)**

**`append-icon`道具显示文本区后的图标:**

```
<template>
  <v-app>
    <v-row class="ma-2" justify="center">
      <v-col cols="6">
        <v-textarea
          append-icon="mdi-note"
          label="append-icon"
          value="Learning about textareas at Coding Beauty!"
        ></v-textarea>
      </v-col>
    </v-row>
  </v-app>
</template><script>
export default {
  name: 'App',
};
</script>
```

**![](img/b436f19ed4d52b5daa16cfa782db6a3b.png)**

**我们还有`prepend-inner-icon`和`append-inner-icon`道具:**

# **阻止文本区域增长**

**`no-resize`道具使文本区域无论包含多少内容都保持相同的大小:**

```
<template>
  <v-app>
    <v-row class="ma-2" justify="center">
      <v-col cols="6">
        <v-textarea
          label="no-resize"
          rows="1"
          no-resize
          value="Just as we can make Vuetify textareas grow automatically, we can also make them to always remain the same size."
        ></v-textarea>
      </v-col>
    </v-row>
  </v-app>
</template><script>
export default {
  name: 'App',
};
</script>
```

**![](img/2ae794576aa55590eae3c163f7d8bf71.png)**

# **摘要**

**文本区域像[文本字段](https://codingbeautydev.com/blog/vuetify-text-fields/)一样接受文本数据，但是数量更多。Vuetify 提供了`v-textarea`组件。**

**[*注册*](http://eepurl.com/hRfyJL) *订阅我们的每周时事通讯，了解我们最新的精彩内容！***

***在*[*codingbeautydev.com*](https://codingbeautydev.com/blog/vuetify-textarea/)*获取更新文章。***