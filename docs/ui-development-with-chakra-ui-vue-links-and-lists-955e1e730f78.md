# 使用 Chakra UI Vue 的 UI 开发—链接和列表

> 原文：<https://blog.devgenius.io/ui-development-with-chakra-ui-vue-links-and-lists-955e1e730f78?source=collection_archive---------4----------------------->

![](img/353f5485090572f22747ec68c32759b1.png)

[澳门图片社](https://unsplash.com/@macauphotoagency?utm_source=medium&utm_medium=referral)拍摄于 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Chakra UI Vue 是一个为 Vue.js 制作的 UI 框架，让我们可以将好看的 UI 组件添加到我们的 Vue 应用程序中。

本文将介绍如何开始使用 Chakra UI Vue 进行 UI 开发。

# 环

我们可以用`c-link`组件在我们的 Vue 应用程序中添加一个链接。

例如，我们可以写:

```
<template>
  <c-box>
    <c-link href="https://vue.chakra-ui.com" is-external>
      Chakra Design system <c-icon name="external-link" mx="2px" />
    </c-link>
  </c-box>
</template><script>
import { CBox, CLink, CIcon } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CLink,
    CIcon,
  },
};
</script>
```

`is-external`让我们在应用程序外创建链接。

`href`有网址可去。

我们可以设置`color`道具来改变链接颜色:

```
<template>
  <c-box>
    <c-link href="https://vue.chakra-ui.com" is-external color="green.500">
      Chakra Design system
    </c-link>
  </c-box>
</template><script>
import { CBox, CLink } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CLink,
  },
};
</script>
```

我们还可以通过将`as`属性设置为这些值，将链接渲染为 Vue 路由器`router-link`或 Nuxt `nuxt-link`。

# 目录

我们可以使用`c-list`组件来呈现一个列表。

例如，我们可以写:

```
<template>
  <c-box>
    <c-list styleType="disc">
      <c-list-item>Lorem ipsum dolor sit amet</c-list-item>
      <c-list-item>Consectetur adipiscing elit</c-list-item>
      <c-list-item>Integer molestie lorem at massa</c-list-item>
      <c-list-item>Facilisis in pretium nisl aliquet</c-list-item>
    </c-list>
  </c-box>
</template><script>
import { CBox, CList, CListItem } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CList,
    CListItem,
  },
};
</script>
```

添加列表。

`styleType`设置项目符号类型。

同样，我们可以添加`c-list-icon`组件来改变列表项图标:

```
<template>
  <c-box>
    <c-list spacing="3">
      <c-list-item>
        <c-list-icon icon="check-circle" color="green.500" />
        Lorem ipsum dolor sit amet, consectetur adipisicing elit
      </c-list-item>
      <c-list-item>
        <c-list-icon icon="check-circle" color="green.500" />
        Assumenda, quia temporibus eveniet a libero incidunt suscipit
      </c-list-item>
      <c-list-item>
        <c-list-icon icon="check-circle" color="green.500" />
        Quidem, ipsam illum quis sed voluptatum quae eum fugit earum
      </c-list-item>
      <c-list-item>
        <c-list-icon icon="settings" color="green.500" />
        Quidem, ipsam illum quis sed voluptatum quae eum fugit earum
      </c-list-item>
    </c-list>
  </c-box>
</template><script>
import { CBox, CList, CListItem, CListIcon } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CList,
    CListItem,
    CListIcon,
  },
};
</script>
```

我们可以像使用`as`属性一样改变列表的标签:

```
<template>
  <c-box>
    <c-list as="ol" style-type="decimal">
      <c-list-item>Lorem ipsum dolor sit amet</c-list-item>
      <c-list-item>Consectetur adipiscing elit</c-list-item>
      <c-list-item>Integer molestie lorem at massa</c-list-item>
      <c-list-item>Facilisis in pretium nisl aliquet</c-list-item>
    </c-list>
  </c-box>
</template><script>
import { CBox, CList, CListItem } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CList,
    CListItem,
  },
};
</script>
```

# 结论

我们可以使用 Chakra UI Vue 轻松添加链接和列表。