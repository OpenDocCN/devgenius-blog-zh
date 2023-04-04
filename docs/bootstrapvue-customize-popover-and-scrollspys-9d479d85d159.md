# BootstrapVue —自定义弹出窗口和滚动窗口

> 原文：<https://blog.devgenius.io/bootstrapvue-customize-popover-and-scrollspys-9d479d85d159?source=collection_archive---------28----------------------->

![](img/ee47c51e4ca40547436f5047d7751129.png)

照片由[莎伦·麦卡琴](https://unsplash.com/@sharonmccutcheon?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

为了制作好看的 Vue 应用，我们需要设计组件的样式。

为了使我们的生活更容易，我们可以使用内置样式的组件。

在本文中，我们看看如何定制`v-b-popover`指令。

此外，我们还将了解如何添加一个 scrollspy 来突出显示视口中具有给定 ID 的元素的链接。

# 变体和自定义类别

我们可以使用`variant`道具来改变样式变体。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-button v-b-popover="options">Hover Me</b-button>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      options: {
        title: "This is the <b>title</b>",
        content: "This is the <i>content<i>",
        html: true,
        variant: 'info'
      }
    };
  }
};
</script>
```

现在弹出窗口的背景是绿色的，因为我们将`variant`属性设置为`'info'`。

# ScrollSpy

scrollspy 是根据用户所在页面的位置更新导航值的指令。

要添加一个 scrollspy，我们可以使用`v-b-scrollspy`指令。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-card no-body>
      <b-nav pills card-header slot="header" v-b-scrollspy:nav-scroller>
        <b-nav-item href="#foo">foo</b-nav-item>
        <b-nav-item-dropdown text="dropdown" right-alignment>
          <b-dropdown-item href="#one">one</b-dropdown-item>
          <b-dropdown-item href="#two">two</b-dropdown-item>
          <b-dropdown-divider></b-dropdown-divider>
          <b-dropdown-item href="#three">three</b-dropdown-item>
        </b-nav-item-dropdown>
        <b-nav-item href="#bar">bar</b-nav-item>
      </b-nav><b-card-body
        id="nav-scroller"
        ref="content"
        style="position:relative; height:300px; overflow-y:scroll;"
      >
        <p>{{ text }}</p>
        <h4 id="foo">foo</h4>
        <p v-for="i in 3" :key="i">{{ text }}</p>
        <h4 id="one">one</h4>
        <p v-for="i in 2" :key="i">{{ text }}</p>
        <h4 id="two">two</h4>
        <p>{{ text }}</p>
        <h4 id="three">three</h4>
        <p v-for="i in 2" :key="i">{{ text }}</p>
        <h4 id="bar">bar</h4>
        <p v-for="i in 3" :key="i">{{ text }}</p>
      </b-card-body>
    </b-card>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      text: "lorem ipsum"
    };
  }
};
</script>
```

我们有`v-b-scrollspy`指令。

它添加了`nav-scroller`修饰符，让它观察`nav-scroller`组件，该组件被设置为`b-card-body`的 ID。

因此，它会观察卡体。

项目由 ID 标识，因为`href`属性被设置为以 hash 开头的内容，这表明我们导航到具有给定 ID 的内容。

scrollspy 监视的链接可以在下拉列表中。

现在，当我们上下滚动时，我们会看到指向具有给定 ID 的元素的链接被突出显示。

# 嵌套 nav

Scrollspys 可以与嵌套的 nav 一起使用。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-container fluid>
      <b-row>
        <b-col cols="4">
          <b-navbar v-b-scrollspy:nav-scroller>
            <b-nav pills vertical>
              <b-nav-item href="#foo">foo</b-nav-item>
              <b-nav pills vertical>
                <b-nav-item class="ml-3 my-1" href="#one">one</b-nav-item>
                <b-nav-item class="ml-3 my-1" href="#two">two</b-nav-item>
                <b-nav-item class="ml-3 my-1" href="#three">three</b-nav-item>
              </b-nav>
              <b-nav-item href="#bar">bar</b-nav-item>
            </b-nav>
          </b-navbar>
        </b-col><b-col
          cols="8"
          id="nav-scroller"
          ref="content"
          style="position:relative; height:300px; overflow-y:scroll;"
        >
          <p>{{ text }}</p>
          <h4 id="foo">foo</h4>
          <p v-for="i in 3" :key="i">{{ text }}</p>
          <h4 id="one">one</h4>
          <p v-for="i in 2" :key="i">{{ text }}</p>
          <h4 id="two">two</h4>
          <p>{{ text }}</p>
          <h4 id="three">three</h4>
          <p v-for="i in 2" :key="i">{{ text }}</p>
          <h4 id="bar">bar</h4>
          <p v-for="i in 3" :key="i">{{ text }}</p>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      text: "lorem ipsum"
    };
  }
};
</script>
```

我们添加了带有`class=”ml-3 my-1"`属性的缩进。

此外，我们有一个`b-navbar`，我们将`b-nav`组件嵌套在其中。

我们有`v-b-scrollspy:nav-scroller`来观察 ID 为`nav-scroller`的`b-col`的滚动位置。

现在，当我们滚动时，我们会看到链接高亮显示。

# 结论

我们可以使用 scrollspy 突出显示视口中带有 ID 的项目。

可以用`variant`属性来设计 Popovers 的样式。