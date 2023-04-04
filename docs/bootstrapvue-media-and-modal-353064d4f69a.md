# BootstrapVue —媒体和模式

> 原文：<https://blog.devgenius.io/bootstrapvue-media-and-modal-353064d4f69a?source=collection_archive---------15----------------------->

![](img/5448c8976f77e94c937ed61b3a07dc16.png)

照片由[迈克尔·布朗宁](https://unsplash.com/@michaelwb?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

为了制作好看的 Vue 应用，我们需要设计组件的样式。

为了使我们的生活更容易，我们可以使用内置样式的组件。

在本文中，我们将研究如何添加媒体和模态组件。

# 媒体

我们可以添加组件来显示位于另一个组件旁边的媒体。

为此，我们添加了`b-media`组件。

我们可以如下使用它:

```
<template>
  <div id="app">
    <b-media>
      <template v-slot:aside>
        <b-img blank blank-color="#ccc" width="50" alt="placeholder"></b-img>
      </template> <h5 class="mt-0">Title</h5>
      <p>foo</p>
      <p>bar</p>
    </b-media>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script>
```

我们在`aside`槽中放了一些东西，如`v-slot:aside`指令所示。

我们在里面放了一张图片。

在它之外，我们添加一些文本。

`aside`将图像放在左边，文本放在右边。

我们也可以嵌套它们，这样我们就可以写:

```
<template>
  <div id="app">
    <b-media>
      <template v-slot:aside>
        <b-img blank blank-color="#ccc" width="50" alt="placeholder"></b-img>
      </template> <h5 class="mt-0">Title</h5>
      <p>foo</p>
      <b-media>
        <template v-slot:aside>
          <b-img blank blank-color="green" width="50" alt="placeholder"></b-img>
        </template> <h5 class="mt-0">Nested Media</h5>
        <p class="mb-0">
          bar
        </p>
      </b-media>
    </b-media>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script>
```

然后，我们在外层组件中加入了另一个`b-media`组件。

# 对齐图像

我们可以按照自己喜欢的方式排列图像。

为此，我们使用带有`vertical-align`支柱的`b-media-aside`组件。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-media no-body>
      <b-media-aside vertical-align="center">
        <b-img blank blank-color="#ccc" width="64" height="128" alt="placeholder"></b-img>
      </b-media-aside> <b-media-body class="ml-3">foo</b-media-body>
    </b-media>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script>
```

我们有`b-media-aside`组件将图像放在左侧。

`vertica-align='center'`将使图像垂直居中。

`b-media-body`在图像的右边创建一个身体。

# 命令

我们可以用`right-align`道具翻转图像和文本的顺序。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-media right-align>
      <template v-slot:aside>
        <b-img blank blank-color="#ccc" width="30" alt="placeholder"></b-img>
      </template><p>foo</p>
    </b-media>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script>
```

现在我们在右边显示了`b-img`组件，而不是文本。

# 媒体列表

这个`tag`道具让我们用我们想要的标签来渲染我们的媒体`b-media`组件。

例如，我们可以写:

```
<template>
  <div id="app">
    <ul>
      <b-media tag="li">
        <template v-slot:aside>
          <b-img blank blank-color="#ccc" width="30" alt="placeholder"></b-img>
        </template><p>foo</p>
      </b-media> <b-media tag="li">
        <template v-slot:aside>
          <b-img blank blank-color="blue" width="30" alt="placeholder"></b-img>
        </template> <p>bar</p>
      </b-media> <b-media tag="li">
        <template v-slot:aside>
          <b-img blank blank-color="green" width="30" alt="placeholder"></b-img>
        </template> <p>baz</p>
      </b-media>
    </ul>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script>
```

然后，我们将`b-media`组件呈现为 li 元素。

# 情态动词

模态是由 JavaScript 和 CSS 驱动的对话框。

我们可以用`b-modal`组件创建一个简单的。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-button v-b-modal.modal>open modal</b-button> <b-modal id="modal" title="Hello">
      <p>Hello!</p>
    </b-modal>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script>
```

我们有带`v-b-modal.modal`指令的`b-button`。

`modal`修饰符是我们想要打开的模态的`id`属性的值。

`b-modal`组件是模态本身。

`title`是模态的标题，显示在顶部。

内容在标签之间。

默认情况下，它还有“确定”和“取消”按钮。

我们可以用`ok-only`道具隐藏取消按钮。

`ok-title`道具让我们改变 OK 按钮的文本。

`cancel-title`道具让我们改变取消按钮的文本。

我们可以使用`modal-ok`和`modal-cancel`插槽来改变按钮内容。

`modal-title`槽让我们改变标题的内容。

`modal-header`槽让我们改变模态头。

`modal-footer`槽让我们改变页脚内容。

![](img/13bc5f701dc1dfdf93a7c66973aa4179.png)

照片由[jger](https://unsplash.com/@jaegerbande?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

`b-media`组件让我们添加一个带有并排文本的图像。

模态是覆盖我们主要内容的对话框。