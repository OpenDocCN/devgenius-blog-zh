# 使用 Chakra UI Vue 开发 UI——可编辑文本输入

> 原文：<https://blog.devgenius.io/ui-development-with-chakra-ui-vue-editable-text-input-507547c9ec85?source=collection_archive---------7----------------------->

![](img/6e93b6de65701dc496792d6d04a98911.png)

照片由 [Victória Kubiaki](https://unsplash.com/@vikubi?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Chakra UI Vue 是一个为 Vue.js 制作的 UI 框架，让我们可以将好看的 UI 组件添加到我们的 Vue 应用程序中。

本文将介绍如何开始使用 Chakra UI Vue 进行 UI 开发。

# 可编辑文本输入

我们可以用`placement`道具设置抽屉的位置。

例如，我们可以写:

```
<template>
  <c-box>
    <c-editable default-value="Hello world" font-size="2xl">
      <c-editable-preview />
      <c-editable-input />
    </c-editable>
  </c-box>
</template><script>
import {
  CBox,
  CEditable,
  CEditableInput,
  CEditablePreview,
} from "@chakra-ui/vue";
export default {
  components: {
    CBox,
    CEditable,
    CEditableInput,
    CEditablePreview,
  },
};
</script>
```

我们注册了`CEditable`、`CEditableInput`、`CEditablePreview`组件来添加可编辑的文本输入。

`CEditable`是主输入包装器。

`CEditableInput`是输入本身。

`CEditablePreview`预览输入的文本。

我们可以为输入添加定制组件来代替默认组件。

为此，我们写道:

```
<template>
  <c-box>
    <c-editable default-value="Hello World" font-size="2xl">
      <template v-slot="{ isEditing, onSubmit, onCancel, onRequestEdit }">
        <c-editable-preview />
        <c-editable-input /><c-flex mt="3">
          <c-button-group v-if="isEditing" size="sm">
            <c-icon-button icon="check" color="green" [@click](http://twitter.com/click)="onSubmit" />
            <c-icon-button icon="close" [@click](http://twitter.com/click)="onCancel" />
          </c-button-group>
          <c-icon-button v-else icon="edit" size="sm" [@click](http://twitter.com/click)="onRequestEdit" />
        </c-flex>
      </template>
    </c-editable>
  </c-box>
</template><script>
import {
  CBox,
  CEditable,
  CEditableInput,
  CEditablePreview,
  CFlex,
  CIconButton,
  CButtonGroup,
} from "@chakra-ui/vue";
export default {
  components: {
    CBox,
    CEditable,
    CEditableInput,
    CEditablePreview,
    CFlex,
    CIconButton,
    CButtonGroup,
  },
};
</script>
```

我们用按钮组中的一些按钮填充默认槽。

老虎机道具有一个`onSubmit`方法，当我们点击 check 按钮时，这个方法就会运行。

`onCancel`是我们点击关闭按钮时运行的。它会将`isEditing`设置为`false`。

当我们按下 Esc 键时就会调用它。

`onRequestEdit`是将`isEditing`设置为`true`的功能。

当我们按回车键时它被调用。

# 结论

我们可以通过 Chakra UI Vue 轻松添加一个可编辑的文本输入。