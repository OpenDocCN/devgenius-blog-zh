# 使用 Chakra UI Vue 的 UI 开发—图标

> 原文：<https://blog.devgenius.io/ui-development-with-chakra-ui-vue-icons-61540c38908e?source=collection_archive---------7----------------------->

![](img/24880acc2138f9b9827c1df665ab46a3.png)

亚历山大·沙托夫在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Chakra UI Vue 是一个为 Vue.js 制作的 UI 框架，让我们可以将好看的 UI 组件添加到我们的 Vue 应用程序中。

本文将介绍如何开始使用 Chakra UI Vue 进行 UI 开发。

# 图标

我们可以用`c-icon`道具添加一个图标。

例如，我们可以写:

```
<template>
  <c-box>
    <c-icon name="phone" :mr="2" />
    <c-icon name="check-circle" size="24px" :mr="2" />
    <c-icon name="warning" size="32px" color="red.500" :mr="2" />
  </c-box>
</template><script>
import { CBox, CIcon } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CIcon,
  },
};
</script>
```

`name`设置要显示的图标名称。

`size`有大小。

`color`有颜色。

我们还可以添加带有`c-icon`的第三方图标。

为此，我们写道:

`main.js`

```
import Vue from "vue";
import Chakra, { CThemeProvider } from "@chakra-ui/vue";
import { faGlobeAfrica } from "@fortawesome/free-solid-svg-icons";import App from "./App.vue";
Vue.use(Chakra, {
  icons: {
    iconPack: "fa",
    iconSet: {
      faGlobeAfrica
    }
  }
});Vue.config.productionTip = false;new Vue({
  render: (h) => h(CThemeProvider, [h(App)])
}).$mount("#app");
```

`App.vue`

```
<template>
  <c-box>
    <c-icon name="globe-africa" />
  </c-box>
</template><script>
import { CBox, CIcon } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CIcon,
  },
};
</script>
```

我们安装了`@fortawesome/free-solid-svg-icons`包，并在`Vue.use`的第二个参数中添加了一些选项。

`iconPack`设置要使用的图标包。

`iconSet`有我们要用的图标。

然后我们设置`name`来显示`App.vue`中的图标。

要添加自定义图标，我们可以将我们的图标添加到 Chakra UI 选项中。

为此，我们写道:

`main.js`

```
import Vue from "vue";
import Chakra, { CThemeProvider } from "@chakra-ui/vue";
import App from "./App.vue";const customIcons = {
  fortranIcon: {
    path: `
      <path d="M19.536 0H4.464A4.463 4.463 0 0 0 0 4.464v15.073A4.463 4.463 0 0 0 4.464 24h15.073A4.463 4.463 0 0 0 24 19.536V4.464A4.463 4.463 0 0 0 19.536 0zm1.193 6.493v3.871l-.922-.005c-.507-.003-.981-.021-1.052-.041-.128-.036-.131-.05-.192-.839-.079-1.013-.143-1.462-.306-2.136-.352-1.457-1.096-2.25-2.309-2.463-.509-.089-2.731-.176-4.558-.177L10.13 4.7v5.82l.662-.033c.757-.038 1.353-.129 1.64-.252.306-.131.629-.462.781-.799.158-.352.262-.815.345-1.542.033-.286.07-.572.083-.636.024-.116.028-.117 1.036-.117h1.012v9.3h-2.062l-.035-.536c-.063-.971-.252-1.891-.479-2.331-.311-.601-.922-.871-2.151-.95a11.422 11.422 0 0 1-.666-.059l-.172-.027.02 2.926c.021 3.086.03 3.206.265 3.465.241.266.381.284 2.827.368.05.002.065.246.065 1.041v1.039H3.271v-1.039c0-.954.007-1.039.091-1.041.05-.001.543-.023 1.097-.049.891-.042 1.033-.061 1.244-.167a.712.712 0 0 0 .345-.328c.106-.206.107-.254.107-6.78 0-6.133-.006-6.584-.09-6.737a.938.938 0 0 0-.553-.436c-.104-.032-.65-.07-1.215-.086l-1.026-.027V2.622h17.458v3.871z"/>
    `,
    viewBox: "0 0 40 40"
  }
};Vue.use(Chakra, {
  icons: {
    extend: {
      ...customIcons
    }
  }
});Vue.config.productionTip = false;new Vue({
  render: (h) => h(CThemeProvider, [h(App)])
}).$mount("#app");
```

`App.vue`

```
<template>
  <c-box>
    <c-icon name="fortranIcon" />
  </c-box>
</template><script>
import { CBox, CIcon } from "@chakra-ui/vue";export default {
  components: {
    CBox,
    CIcon,
  },
};
</script>
```

我们有一个`customIcons`对象，它的 SVG `path`元素被设置为`path`的值。

然后我们将`customIcons`对象合并到`extend`中。

在`App.vue`中，我们将`name`设置为`customIcons`中图标的属性名。

# 结论

我们可以使用 Chakra UI Vue 轻松添加各种图标。