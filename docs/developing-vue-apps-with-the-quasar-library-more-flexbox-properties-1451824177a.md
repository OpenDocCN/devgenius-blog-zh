# 使用 Quasar 库开发 Vue 应用程序—更多 Flexbox 属性

> 原文：<https://blog.devgenius.io/developing-vue-apps-with-the-quasar-library-more-flexbox-properties-1451824177a?source=collection_archive---------7----------------------->

![](img/b90c513b7006525d9886d190fda8da53.png)

照片由[普里西拉·杜·普里兹](https://unsplash.com/@priscilladupreez?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Quasar 是一个流行的 Vue UI 库，用于开发好看的 Vue 应用程序。

在本文中，我们将了解如何使用 Quasar UI 库创建 Vue 应用程序。

# 自我对齐

我们可以使用`self-*`类将`align-self` CSS 属性添加到我们的子元素中。

# 水平线向

我们可以使用`justify-*`类来水平对齐子元素。

例如，我们可以写:

```
<!DOCTYPE html>
<html>
  <head>
    <link
      href="https://fonts.googleapis.com/css?family=Roboto:100,300,400,500,700,900|Material+Icons"
      rel="stylesheet"
      type="text/css"
    />
    <link
      href="https://cdn.jsdelivr.net/npm/quasar@1.12.13/dist/quasar.min.css"
      rel="stylesheet"
      type="text/css"
    />
  </head> <body class="body--dark">
    <script src="https://cdn.jsdelivr.net/npm/vue@^2.0.0/dist/vue.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/quasar@1.12.13/dist/quasar.umd.min.js"></script>
    <div id="q-app">
      <div class="row justify-center">
        <div class="col-4">
          1
        </div>
        <div class="col-4">
          2
        </div>
      </div>
    </div> <script>
      new Vue({
        el: "#q-app",
        data() {
          return {};
        },
        methods: {}
      });
    </script>
  </body>
</html>
```

我们添加了`justify-center`类来使子 div 居中。

此外，我们可以添加`justify-start`来对齐左边的子 div。

`justify-end`将子 div 对齐右侧。

`justify-around`以相等的间距隔开子 div。

并且`justify-between`将子 div 与它们之间的空间隔开。

# 列换行

如果宽度超过 12 列，列将自动换行到下一行。

例如，我们可以写:

```
<!DOCTYPE html>
<html>
  <head>
    <link
      href="https://fonts.googleapis.com/css?family=Roboto:100,300,400,500,700,900|Material+Icons"
      rel="stylesheet"
      type="text/css"
    />
    <link
      href="https://cdn.jsdelivr.net/npm/quasar@1.12.13/dist/quasar.min.css"
      rel="stylesheet"
      type="text/css"
    />
  </head> <body class="body--dark">
    <script src="https://cdn.jsdelivr.net/npm/vue@^2.0.0/dist/vue.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/quasar@1.12.13/dist/quasar.umd.min.js"></script>
    <div id="q-app">
      <div class="row">
        <div class="col-6 col-sm-3">.col-6 .col-sm-3</div>
        <div class="col-6 col-sm-3">.col-6 .col-sm-3</div>
        <div class="col-6 col-sm-3">.col-6 .col-sm-3</div>
        <div class="col-6 col-sm-3">.col-6 .col-sm-3</div>
      </div>
    </div> <script>
      new Vue({
        el: "#q-app",
        data() {
          return {};
        },
        methods: {}
      });
    </script>
  </body>
</html>
```

如果屏幕点击了`xs`断点，我们需要`col-6`使 div 的宽度为 6 列。

如果屏幕达到`sm`断点或更高，则`col-sm-3`会生成 div 3 列。

# 偏移列

我们可以使用`offset-*`类将 div 移动给定数量的列。

例如，我们可以写:

```
<!DOCTYPE html>
<html>
  <head>
    <link
      href="[https://fonts.googleapis.com/css?family=Roboto:100,300,400,500,700,900|Material+Icons](https://fonts.googleapis.com/css?family=Roboto:100,300,400,500,700,900|Material+Icons)"
      rel="stylesheet"
      type="text/css"
    />
    <link
      href="https://cdn.jsdelivr.net/npm/quasar@1.12.13/dist/quasar.min.css"
      rel="stylesheet"
      type="text/css"
    />
  </head> <body class="body--dark">
    <script src="https://cdn.jsdelivr.net/npm/vue@^2.0.0/dist/vue.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/quasar@1.12.13/dist/quasar.umd.min.js"></script>
    <div id="q-app">
      <div class="row">
        <div class="col-md-4">.col-md-4</div>
        <div class="col-md-4 offset-md-4">.col-md-4 .offset-md-4</div>
      </div>
    </div> <script>
      new Vue({
        el: "#q-app",
        data() {
          return {};
        },
        methods: {}
      });
    </script>
  </body>
</html>
```

如果屏幕点击了`md`断点或更宽的断点，我们需要`offset-md-4`将 div 4 列向右移动。

# 嵌套

此外，我们可以嵌套 flexbox 行和列。

例如，我们可以写:

```
<!DOCTYPE html>
<html>
  <head>
    <link
      href="https://fonts.googleapis.com/css?family=Roboto:100,300,400,500,700,900|Material+Icons"
      rel="stylesheet"
      type="text/css"
    />
    <link
      href="https://cdn.jsdelivr.net/npm/quasar@1.12.13/dist/quasar.min.css"
      rel="stylesheet"
      type="text/css"
    />
  </head> <body class="body--dark">
    <script src="https://cdn.jsdelivr.net/npm/vue@^2.0.0/dist/vue.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/quasar@1.12.13/dist/quasar.umd.min.js"></script>
    <div id="q-app">
      <div class="row">
        <div class="col-sm-9">
          <p>Level 1</p>
          <div class="row">
            <div class="col-8 col-sm-6">
              Level 2
            </div>
            <div class="col-4 col-sm-6">
              Level 2
            </div>
          </div>
        </div>
      </div>
    </div> <script>
      new Vue({
        el: "#q-app",
        data() {
          return {};
        },
        methods: {}
      });
    </script>
  </body>
</html>
```

将一行嵌套在另一列中。

# 结论

我们可以用 Quasar 提供的类来应用各种 flexbox CSS 属性。