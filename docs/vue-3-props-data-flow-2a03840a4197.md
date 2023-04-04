# Vue 3 — Props 数据流

> 原文：<https://blog.devgenius.io/vue-3-props-data-flow-2a03840a4197?source=collection_archive---------4----------------------->

![](img/dbc1892b9899216199eae0650a08296c.png)

[Franck V.](https://unsplash.com/@franckinjapan?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

**Vue 3 处于测试阶段，可能会有变化。**

Vue 3 是 Vue 前端框架的最新版本。

它建立在 Vue 2 的普及性和易用性之上。

在这篇文章中，我们将看看如何使用 Vue 3 的道具。

# 单向数据流

道具在父组件和子组件之间有一个单向向下绑定。

当父属性更新时，更新通过 props 传递给子属性。

这可以防止子组件意外改变父组件的状态。

这使得我们的应用程序更容易理解。

我们不应该改变道具。

如果我们需要改变它们的值，那么我们应该先给它们分配一个新的属性。

例如，如果我们需要改变用道具值设置的初始值的值，那么我们应该首先把它分配给一个状态。

所以我们应该写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
  </head>
  <body>
    <div id="app">
      <counter :initial-count="5"></counter>
    </div>
    <script>
      const app = Vue.createApp({}); app.component("counter", {
        props: ["initialCount"],
        data() {
          return {
            count: this.initialCount
          };
        },
        template: `
          <div>
            <button @click='count++'>increment</button>
            <p>{{count}}</p>
          </div>
        `
      }); app.mount("#app");
    </script>
  </body>
</html>
```

我们有`initialCount`属性，我们用它来设置`counter`组件中`count`状态的初始值。

那我们就可以随心所欲了。

如果值需要转换，那么我们可以将它作为计算属性放入。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
  </head>
  <body>
    <div id="app">
      <counter :initial-count="5"></counter>
    </div>
    <script>
      const app = Vue.createApp({}); app.component("counter", {
        props: ["initialCount"],
        data() {
          return {
            count: this.initialCount
          };
        },
        computed: {
          doubleCount() {
            return this.count * 2;
          }
        },
        template: `
          <div>
            <button @click='count++'>increment</button>
            <p>{{doubleCount}}</p>
          </div>
        `
      }); app.mount("#app");
    </script>
  </body>
</html>
```

我们有`initial-count`道具，通过返回`this.count * 2`转化为`doubleCount`。

现在我们不必对属性值本身做任何事情。

我们只需在`data`方法和`counter`组件的 computed 属性中将状态更改为我们想要的状态。

# 正确验证

我们可以通过检查数据类型等来验证 props。

我们将`props`属性的值设置为一个构造函数。

或者我们可以用一个函数来验证它。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
  </head>
  <body>
    <div id="app">
      <blog-post v-for="post of posts"></blog-post>
    </div>
    <script>
      const app = Vue.createApp({
        data() {
          return {
            posts: [{ author: "james", likes: 100 }]
          };
        }
      });
      app.component("blog-post", {
        props: {
          title: {
            type: String,
            default: "default title"
          }
        },
        template: `<p>{{title}}</p>`
      });
      app.mount("#app");
    </script>
  </body>
</html>
```

然后我们会显示“默认标题”文本，因为我们从未将值传递给`title`属性。

`default`有默认值。

`validator`具有道具验证器功能。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
  </head>
  <body>
    <div id="app">
      <blog-post v-for="post of posts" type="news"></blog-post>
    </div>
    <script>
      const app = Vue.createApp({
        data() {
          return {
            posts: [{ author: "james", likes: 100 }]
          };
        }
      });
      app.component("blog-post", {
        props: {
          type: {
            validator(value) {
              return ["news", "announcement"].indexOf(value) !== -1;
            }
          }
        },
        template: `<p>{{type}}</p>`
      });
      app.mount("#app");
    </script>
  </body>
</html>
```

为`type`属性添加一个验证器。

当我们传入具有给定名称的道具时，就会运行`validator`方法。

`value`是我们传入的值。

所以如果我们传入除了`'new'`或`'announcement'`之外的东西，比如:

```
<blog-post v-for="post of posts" type="foo"></blog-post>
```

然后我们会得到警告。

我们还可以添加`required`属性并将其设置为`true`来制作所需的道具，如下所示:

```
prop: {
  type: String,
  required: true
}
```

![](img/2e0c1a754b60c11b5a01728cdd8bc2e2.png)

照片由[布雷特·乔丹](https://unsplash.com/@brett_jordan?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 结论

我们可以用构造函数和验证函数来验证 props。

此外，我们可以添加`default`属性来设置道具的默认值。